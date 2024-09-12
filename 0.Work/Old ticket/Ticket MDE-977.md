
##
`MDESystemInfo::RegisterRequestHandler` register function để handle request từ UI
(F:\work_space\vscode\grsc-gsdk\shared\cross-platform\modules\ms\src\utils\win32\verify_signature_utils.cpp) : đọc file certificate và manifest

đã làm: manifestPlatformSpec::getCertificateFilePath()
=> nhớ đổi directory theo yêu cầu

# Service
`std::vector<std::wstring> getAllOpswatCertificatePaths()`
loop through file path to `createCertificateContext(LPCWSTR filePath)` 
=> use context to get 
				- `std::string GetCertSimpleName(PCCERT_CONTEXT pCertContext)`
				- `pCertContext->pCertInfo->Issuer`
				-  `pCertContext->pCertInfo->NotAfter` (expiration date)
				-  gom lại cùng file path thành 1 struct `push_back` vào vector
# handle request từ UI
Trong IPC search `mde_unmanaged_get_about_info` nhét thêm một cái map 

```cpp
(std::vector<CertInfoStruct>,CertInfoVec)
```
vào `OUTPUT`
```cpp
REQUEST(mde_unmanaged_get_about_info)
OUTPUT
(
	
    (std::string, hostName, ""),
    (std::string, userName, ""),
    (std::string, osInfo, ""),
    (std::string, sdkVersion, ""),
    (std::string, version, ""),
    (std::string, allowlist, ""),
    (bool, hidStatus, false),
    (int, trustDays, 0),
    (int, duration,0)
)
```
trong `MDESystemInfo::RegisterRequestHandler` trả
```cpp
		std::vector<CertInfoStruct> CertInfoVect = getOpswatCertInfo();
        if (CertInfoVect) {
          aboutInfo->set_certMap(CertInfoVec);
        }
```


# read certificate name
dựa vào Phind search ngày 24/7/2024  ta đã tạo được Certificate Context `PCCERT_CONTEXT cert ` từ certificate file path
Bây giờ cần viết 1 function nhận Certificate context đó nhét vào `CertGetNameStringA()` (API của window)  trả về `pszNameString`  đã có search trong phind.com ngày 23/7/24
Tìm hiều xem  cần truyền `dwType` parameter nào vào `CertGetNameStringA()` để `pszNameString` trả về  **CERT_NAME_SIMPLE_DISPLAY_TYPE**


# Open certificate file
include
```cpp
#include <windows.h>
#include <iostream>
```

This function uses `ShellExecute` to open the certificate file with the default application associated with its file extension.
```cpp
void OpenCertificateViewer(const std::wstring& filePath) {
    ShellExecute(NULL, L"open", filePath.c_str(), NULL, NULL, SW_SHOWNORMAL);
}
```

# get all file path inside OPSWAT/.ssh

```cpp
std::vector<std::wstring> getAllOpswatCertificatePaths() {
    std::vector<std::wstring> certPaths;
    std::string directory = owc::ms::StringUtils::toString(expandEnvironmentStrings(L"%ALLUSERSPROFILE%")) + "\\OPSWAT\\.ssh\\";
    
    std::error_code ec;
    for (const auto& entry : fs::recursive_directory_iterator(directory, fs::directory_options::skip_permission_denied, ec)) {
        if (fs::is_regular_file(entry.path(), ec)) {
            std::wstring filePath( entry.path().string().begin(), entry.path().string().end() );
            if( VerifyFileSignature( filePath.c_str())){
	            certPaths.pushback( filePath);
            }
        }
    }
    
    return certPaths;
}

```

# verify certificate file
### Using `WinVerifyTrust` to Check for a Valid Signature
The `WinVerifyTrust` function can be used to verify the signature of a PE file. This function checks if the file's signature is valid and if it chains up to a trusted root certificate. If the signature is valid and trusted, it implies that the file could be considered a certificate file, especially if it's signed by a known Certificate Authority (CA).
This code snippet initializes a `WINTRUST_FILE_INFO` structure with the path to the file you want to check. It then sets up a `WINTRUST_DATA` structure to specify the verification policy and action. The `WinVerifyTrust` function is called to perform the verification. Depending on the outcome, it prints whether the file is signed and the signature is verified, not signed, or if an unknown error occurred.
This approach is particularly useful for executable files (.exe, .dll, etc.) and helps in identifying files that are signed with valid certificates. For other types of certificate files (e.g., .cer, .pfx), you might need to use different functions such as `CryptQueryObject` to parse and validate the certificate content directly.


```cpp
#include <windows.h>
#include <Softpub.h>
#include <wincrypt.h>
#include <wintrust.h>

#pragma comment(lib, "wintrust.lib")

BOOL VerifyFileSignature(LPCWSTR filePath) {
    WINTRUST_FILE_INFO fileInfo = { sizeof(WINTRUST_FILE_INFO), filePath };
    GUID policyGUID = WINTRUST_ACTION_GENERIC_VERIFY_V2;
    WINTRUST_DATA trustData = { sizeof(WINTRUST_DATA), WTD_REVOKE_NONE, WTD_CHOICE_FILE, WTD_STATEACTION_VERIFY, NULL, &fileInfo };

    LONG status = WinVerifyTrust(NULL, &policyGUID, &trustData);

    switch (status) {
        case ERROR_SUCCESS:
            wprintf(L"The file \"%s\" is signed and the signature was verified.\n", filePath);
            break;
        case TRUST_E_NOSIGNATURE:
            wprintf(L"The file \"%s\" is not signed.\n", filePath);
            break;
        default:
            wprintf(L"An unknown error occurred trying to verify the signature of the file \"%s\".\n", filePath);
            break;
    }

    trustData.dwStateAction = WTD_STATEACTION_CLOSE;
    WinVerifyTrust(NULL, &policyGUID, &trustData); // Clean up

    return status == ERROR_SUCCESS;
}

int main() {
    LPCWSTR filePath = L"path_to_your_file";
    VerifyFileSignature(filePath);
    return 0;
}

```



# Show Certificate inside :IsWindowsStoreTrusted
1. After building the certificate chain with CertGetCertificateChain, iterate through the chain context.
2. For each certificate in the chain, you can use CertGetNameString to get readable information about the certificate.
3. Store this information in a string or data structure.
4. Use the Windows API function CryptUIDlgViewContext to display a dialog with the certificate details.
```cpp
    if (CertGetCertificateChain(...)) {
        for (DWORD i = 0; i < chainContext->cChain; i++) {
            PCERT_SIMPLE_CHAIN simpleChain = chainContext->rgpChain[i];
            for (DWORD j = 0; j < simpleChain->cElement; j++) {
                PCCERT_CONTEXT certContext = simpleChain->rgpElement[j]->pCertContext;
                
                // Get and store certificate information
                CHAR szName[256];
                CertGetNameStringA(certContext, CERT_NAME_SIMPLE_DISPLAY_TYPE, 0, NULL, szName, sizeof(szName));
                
                // Display certificate
                CRYPTUI_VIEWCERTIFICATE_STRUCT viewInfo = {0};
                viewInfo.dwSize = sizeof(CRYPTUI_VIEWCERTIFICATE_STRUCT);
                viewInfo.hwndParent = NULL;  // Use NULL for non-modal dialog
                viewInfo.dwFlags = 0;
                viewInfo.szTitle = L"Certificate in Trust Chain";
                viewInfo.pCertContext = certContext;
                CryptUIDlgViewContext(CERT_STORE_CERTIFICATE_CONTEXT, &viewInfo, NULL, NULL, NULL, NULL);
            }
        }
    }
```



```cpp
typedef struct _CERT_CHAIN_CONTEXT CERT_CHAIN_CONTEXT, *PCERT_CHAIN_CONTEXT;
typedef const CERT_CHAIN_CONTEXT *PCCERT_CHAIN_CONTEXT;

struct _CERT_CHAIN_CONTEXT {
    DWORD                   cbSize;
    CERT_TRUST_STATUS       TrustStatus;
    DWORD                   cChain;
    PCERT_SIMPLE_CHAIN*     rgpChain;

    // Following is returned when CERT_CHAIN_RETURN_LOWER_QUALITY_CONTEXTS
    // is set in dwFlags
    DWORD                   cLowerQualityChainContext;
    PCCERT_CHAIN_CONTEXT*   rgpLowerQualityChainContext;

    // fHasRevocationFreshnessTime is only set if we are able to retrieve
    // revocation information for all elements checked for revocation.
    // For a CRL its CurrentTime - ThisUpdate.
    //
    // dwRevocationFreshnessTime is the largest time across all elements
    // checked.
    BOOL                    fHasRevocationFreshnessTime;
    DWORD                   dwRevocationFreshnessTime;    // seconds

    // Flags passed when created via CertGetCertificateChain
    DWORD                   dwCreateFlags;

    // Following is updated with unique Id when the chain context is logged.
    GUID                    ChainId;
};
```

PCCERT_CHAIN_CONTEXT*   rgpLowerQualityChainContext;` trỏ tới `_CERT_CHAIN_ELEMENT`

```cpp
typedef struct _CERT_CHAIN_ELEMENT {

    DWORD                 cbSize;
    PCCERT_CONTEXT        pCertContext;
    CERT_TRUST_STATUS     TrustStatus;
    PCERT_REVOCATION_INFO pRevocationInfo;

    PCERT_ENHKEY_USAGE    pIssuanceUsage;       // If NULL, any
    PCERT_ENHKEY_USAGE    pApplicationUsage;    // If NULL, any

    LPCWSTR               pwszExtendedErrorInfo;    // If NULL, none
} CERT_CHAIN_ELEMENT, *PCERT_CHAIN_ELEMENT;
typedef const CERT_CHAIN_ELEMENT* PCCERT_CHAIN_ELEMENT;
```

 `PCCERT_CONTEXT        pCertContext;` trỏ tới `_CERT_CONTEXT`

```cpp
//+-------------------------------------------------------------------------
//  Certificate context.
//
//  A certificate context contains both the encoded and decoded representation
//  of a certificate. A certificate context returned by a cert store function
//  must be freed by calling the CertFreeCertificateContext function. The
//  CertDuplicateCertificateContext function can be called to make a duplicate
//  copy (which also must be freed by calling CertFreeCertificateContext).
//--------------------------------------------------------------------------
// certenrolls_begin -- CERT_CONTEXT
typedef struct _CERT_CONTEXT {
    DWORD                   dwCertEncodingType;
    BYTE                    *pbCertEncoded;
    DWORD                   cbCertEncoded;
    PCERT_INFO              pCertInfo;
    HCERTSTORE              hCertStore;
} CERT_CONTEXT, *PCERT_CONTEXT;
typedef const CERT_CONTEXT *PCCERT_CONTEXT;
```

`PCERT_INFO              pCertInfo;` trỏ tới `_CERT_INFO`
```cpp
//+-------------------------------------------------------------------------
//  Information stored in a certificate
//
//  The Issuer, Subject, Algorithm, PublicKey and Extension BLOBs are the
//  encoded representation of the information.
//--------------------------------------------------------------------------
// certenrolls_begin -- CERT_CONTEXT
typedef struct _CERT_INFO {
    DWORD                       dwVersion;
    CRYPT_INTEGER_BLOB          SerialNumber;
    CRYPT_ALGORITHM_IDENTIFIER  SignatureAlgorithm;
    CERT_NAME_BLOB              Issuer;
    FILETIME                    NotBefore;
    FILETIME                    NotAfter;
    CERT_NAME_BLOB              Subject;
    CERT_PUBLIC_KEY_INFO        SubjectPublicKeyInfo;
    CRYPT_BIT_BLOB              IssuerUniqueId;
    CRYPT_BIT_BLOB              SubjectUniqueId;
    DWORD                       cExtension;
    PCERT_EXTENSION             rgExtension;
} CERT_INFO, *PCERT_INFO;
```
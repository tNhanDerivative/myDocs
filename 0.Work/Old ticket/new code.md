trong MDESystemInfo.cpp

```cpp

std::wstring expandEnvironmentStrings(const std::wstring& source) {
  wchar_t buff[MAX_PATH] = {0};
  ExpandEnvironmentStrings(source.c_str(), buff, MAX_PATH);
  std::wstring dest = buff;

  // still contains environment variable as the env var didn't exist
  // so return false
  if (dest.find(L"%") != std::wstring::npos) dest = L"";
  return dest;
}

BOOL VerifyFileSignature(LPCWSTR filePath) {
  WINTRUST_FILE_INFO fileInfo = {sizeof(WINTRUST_FILE_INFO), filePath};
  GUID policyGUID = WINTRUST_ACTION_GENERIC_VERIFY_V2;
  WINTRUST_DATA trustData{0};
  trustData.cbStruct = sizeof(WINTRUST_DATA);
  trustData.pPolicyCallbackData = NULL;
  trustData.pSIPClientData = NULL; 
  trustData.dwUIChoice = WTD_UI_NONE;
  trustData.dwUnionChoice = WTD_CHOICE_FILE;
  trustData.dwStateAction = WTD_STATEACTION_VERIFY;
  trustData.hWVTStateData = NULL;
  trustData.pwszURLReference = NULL;
  trustData.pFile = &fileInfo;

  LONG status = WinVerifyTrust(NULL, &policyGUID, &trustData);

  switch (status) {
    case ERROR_SUCCESS:
      wprintf(L"The file \"%s\" is signed and the signature was verified.\n",
              filePath);
      break;
    case TRUST_E_NOSIGNATURE:
      wprintf(L"The file \"%s\" is not signed.\n", filePath);
      break;
    default:
      wprintf(
          L"An unknown error occurred trying to verify the signature of the "
          L"file \"%s\".\n",
          filePath);
      break;
  }

  trustData.dwStateAction = WTD_STATEACTION_CLOSE;
  WinVerifyTrust(NULL, &policyGUID, &trustData);  // Clean up

  return status == ERROR_SUCCESS;
}

void OpenCertificateViewer(const std::wstring& filePath) {
  ShellExecute(NULL, L"open", filePath.c_str(), NULL, NULL, SW_SHOWNORMAL);
}

std::vector<std::wstring> getAllOpswatCertificatePaths() {
  std::vector<std::wstring> certPaths;
  std::wstring directory = expandEnvironmentStrings(L"%ALLUSERSPROFILE%") + L"\\OPSWAT\\.ssh\\";

  std::error_code ec;
  for (const auto& entry : fs::recursive_directory_iterator(
           directory, fs::directory_options::skip_permission_denied, ec)) {
    if (fs::is_regular_file(entry.path(), ec)) {
      std::wstring filePath(entry.path().string().begin(),
                            entry.path().string().end());
      if (VerifyFileSignature(filePath.c_str())) {
        certPaths.push_back(filePath);
      }
    }
  }

  return certPaths;
}


```
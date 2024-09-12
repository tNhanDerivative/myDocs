
# Verify certificate work flow
workflow for the main IsTrusted functions:

1. ManifestWorker::IsTrusted(const fs::path &cert):

- First, it calls manifestPlatformSpec::IsTrusted(const std::string &cert)
- If that returns an empty string (indicating trust), the function returns an empty string
- If not trusted, it then calls manifestPlatformSpec::IsWindowsStoreTrusted(cert, true)
- This allows for checking trust in both the file system and the Windows certificate store
1. manifestPlatformSpec::IsTrusted(const std::string &cert):

- Extracts the public key from the certificate
- Sets a default error message "Not trusted"
- Checks a set of predefined directories for matching certificates
- For each file in these directories, it:
	- Reads the certificate file
	- Extracts the public key
	- Compares the public keys using RsaEq
- If a matching public key is found, it returns an empty string (indicating trust)
- If no match is found, it returns the error message
1. manifestPlatformSpec::IsTrusted(const RsaHandle &rsa1, std::string cert2):

- Extracts the public key from cert2
- Compares the extracted public key with rsa1 using RsaEq
- Returns an empty string if the keys match (indicating trust), otherwise returns "Not trusted"
1. manifestPlatformSpec::IsWindowsStoreTrusted(std::wstring cert, bool checkTime):

- Converts the certificate to binary
- Creates a certificate context
- Builds and verifies the certificate chain
- Checks for various trust issues (revocation, untrusted root, invalid signature, etc.)
- Returns an empty string if trusted, or a string describing trust issues if not trusted

This multi-layered approach allows for thorough trust verification using both file system certificates and the Windows certificate store, providing a robust trust checking mechanism
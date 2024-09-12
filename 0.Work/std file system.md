
# extension base search
```cpp
std::vector<std::wstring> getAllCertInfoFrom(const std::wstring& folderPath) {
  std::vector<std::wstring> certInfoVec;
  const fs::path dirPath{folderPath};
  // need check whether it had enough extension
  std::set<std::string> certificateExtensions = {".pfx",
                                                    ".p12",
                                                    ".cer",
                                                    ".crt",
                                                    ".crl",
                                                    ".csr"
                                                    ".der",
                                                    ".pem",
                                                    ".p7b",
                                                    ".p7r",
                                                    ".spc"};

  std::error_code ec;
  for (const auto& entry : fs::directory_iterator(dirPath, ec)) {
    if (entry.is_regular_file()) {
        if (auto pos = certificateExtensions.find(entry.path().extension().string());
            pos != certificateExtensions.end()) {
          certInfoVec.push_back(entry.path().filename().wstring());
        }
    }
  }
  return certInfoVec;
}
```


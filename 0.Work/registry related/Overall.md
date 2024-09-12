
`MediaShared.cpp` will compose only the part start with  "media-security" to the hkey of the path
`featureName` is hard coded as `media-security`
example:
```cpp
auto path = std::wstring(featureName) + L"\\" + childPath;
```

`MediaShared.cpp` will then pass that part to function in `Shared.cpp` to open HKEY;
with `FeaturesRegistryPath = _T(R"(SOFTWARE\OPSWAT\Gears Client\features)");`
`OWC_ROOT_REGKEY = HKEY_LOCAL_MACHINE;`

example
```cpp
bool deleteFeatureRegistryKeyChild(const FeatureNameType &name) {
  try {
    RegKey key;
    auto featureKeyPath = tstring{FeaturesRegistryPath} + _T("\\") + name;
    key.Open(OWC_ROOT_REGKEY, featureKeyPath);
    ...
  ...
```


`MediaShared.cpp` also pass key value or `subKey` name we want to manipulate IF NEEDED
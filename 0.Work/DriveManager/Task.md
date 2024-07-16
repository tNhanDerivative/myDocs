
# Query bus Type
Chú ý: DriveLetter là DriveID[0]
Thêm hàm dưới đây vào DriveManager. Sau đó search  `DriveType::DVD` để xem nó có những chỗ nào update `DriveType::DVD`. Và ta sẽ check xem nó có phải ISO không
```cpp

bool queryStorageBusType(wchar_t driveLetter) {
  bool type = false;
  //STORAGE_BUS_TYPE::BusTypeUnknown;
  wchar_t deviceName[MAX_PATH];
  std::wstring devicePath = std::wstring(1, driveLetter) + L":";

  QueryDosDevice(devicePath.c_str(), deviceName, MAX_PATH);

  std::wstring globalRootDeviceName = L"\\\\.\\GLOBALROOT";
  globalRootDeviceName += deviceName;

  HANDLE hDevice = CreateFile(globalRootDeviceName.c_str(), 0,
                              FILE_SHARE_READ | FILE_SHARE_WRITE, NULL,
                              OPEN_EXISTING, 0, NULL);
  if (!hDevice) {
    return type;
  }

  STORAGE_PROPERTY_QUERY storagePropertyQuery;
  ZeroMemory(&storagePropertyQuery, sizeof(STORAGE_PROPERTY_QUERY));
  storagePropertyQuery.PropertyId = StorageDeviceProperty;
  storagePropertyQuery.QueryType = PropertyStandardQuery;

  STORAGE_DESCRIPTOR_HEADER storageDescriptorHeader = {0};
  DWORD dwBytesReturned = 0;
  if (!DeviceIoControl(
          hDevice, IOCTL_STORAGE_QUERY_PROPERTY, &storagePropertyQuery,
          sizeof(STORAGE_PROPERTY_QUERY), &storageDescriptorHeader,
          sizeof(STORAGE_DESCRIPTOR_HEADER), &dwBytesReturned, NULL)) {
    CloseHandle(hDevice);
    return type;
  }

  DWORD dwOutBufferSize = storageDescriptorHeader.Size;
  BYTE* pOutBuffer = new BYTE[dwOutBufferSize];
  ZeroMemory(pOutBuffer, dwOutBufferSize);

  if (!DeviceIoControl(hDevice, IOCTL_STORAGE_QUERY_PROPERTY,
                       &storagePropertyQuery, sizeof(STORAGE_PROPERTY_QUERY),
                       pOutBuffer, dwOutBufferSize, &dwBytesReturned, NULL)) {
    delete[] pOutBuffer;
    CloseHandle(hDevice);
    return type;
  }

  if (pOutBuffer != NULL && ((STORAGE_DEVICE_DESCRIPTOR*)pOutBuffer)->Size >=
                                sizeof(STORAGE_DEVICE_DESCRIPTOR)) {
    type =
        ((STORAGE_DEVICE_DESCRIPTOR*)pOutBuffer)->BusType == STORAGE_BUS_TYPE::BusTypeFileBackedVirtual;
  }

  delete[] pOutBuffer;
  CloseHandle(hDevice);

  return type;
}
```



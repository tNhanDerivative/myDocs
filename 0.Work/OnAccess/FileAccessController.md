





# Dependency

## DriveHelper
`DriveHelper.cpp`, provides an interface for interacting with the underlying drive control driver (`IWadcd`). It exposes methods like `allowProcess`, `allowFile`, `protectPath`, and `blockFile`, which allow the `FileAccessController` to control access to processes, files, and paths.

example
```cpp
void FileAccessController::filterISODriveByConfig() {
  auto excludeImage = config().get("config/media_security/exclude_disk_image");
  auto exludeISO = excludeImage["iso"] == 1;
  if (!exludeISO) return;
  auto drives = ListOfUsbDrive(DriveFilter::Active, true);
  for (auto& drive : drives) {
    if (drive.get_drive_type() == DriveType::ISO) {
      RMP_LOGGER_INFO("Found ISO drive: {}", drive.get_driveID());
      auto wDriveID = string_cast<std::wstring>(drive.get_driveID());
      DCDHelper::instance()->protectPath(wDriveID.c_str(), 0, false);        //DriveHelper.cpp
    }
  }
  RMP_LOGGER_INFO("Excluded virtual ISO drive!");
}

```
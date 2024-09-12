`DriveManager::refreshDrives()` refactor lại đoạn này trong BusType
```cpp
	auto type = vol["type"].string_value();
	
    drive.type = DriveType::USB;

    if (type == "Hard Disk") {
      if (vol["interface"].string_value() == "other") {
        drive.type = DriveType::GFS;
      } else {
        drive.type = DriveType::HardDisk;
      }
    } else if (USBEncryption::IsBitLocker(drive.id)) {
      drive.type = DriveType::EncryptedDrive;
    }

    if (type == "Mobile") {
      drive.type = DriveType::Mobile;
    }

    if (type == "CD-ROM") {
      drive.type = DriveType::DVD;
      if (isDVDHasISOBusType(drive.id[0])) {
        drive.type = DriveType::ISO;
      }
    }

```
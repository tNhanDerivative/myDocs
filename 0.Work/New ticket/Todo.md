Cherry pick qua release-2

MDE-1094  OAFS | Add device information in blocked event when sending to the server 
chờ chốt message. Hỏi Thắng vụ thay đổi logic

MDE-1096| Don't show tooltip with USB information 
QT
MDE-1110| Secure Download & OAFS| Can't cancel scanning process 
QT

MDE-1120 not 1-1; Format fail → wipe data successfully
MDE-1118 tìm root cause, 



987: Update text


MDE-933 check new infrastructure log before finalize how to log on CLI


When `ProcUtil::RunProcess(info)` fail inside ` Internal::quickFormat()` will return `formatSuccess=fail`
Causing we have to run `Internal::wipeDrivePri(driveID)` again very inefficient
```cpp  
do {
    wipeResult = Internal::wipeDrivePri(driveID);
    formatSuccess = Internal::quickFormat(driveID, oldInfo);
    ++currentRetryTime;
  } while (wipeResult == WipeResult::DiskSizeErr && formatSuccess &&
           currentRetryTime <= maxTryTime);
```

`GetMetaData()` fail dẫn đến
Invalid allocation unit size: If the `formatAllocationUnitSize(meta.allocationUnitSize)` function returns an invalid or unsupported value, the format command could fail.
dẫn đến `ProcUtil::RunProcess()` fail => format fail => wipe lại

suggest:
```cpp
do {
    wipeResult = Internal::wipeDrivePri(driveID);
    do{
		formatSuccess = Internal::quickFormat(driveID, oldInfo);
		++currentRetryTime;   
    }Until( formatSuccess || currentRetryTime > maxTryTime)
    
  } while (wipeResult == WipeResult::DiskSizeErr && WipeResult::WriteFileErr &&
           currentRetryTime <= maxTryTime);
```

Trong `onDriveBlock` sự kiện block "through the drive manager"
# Wiping

2. There's no check for write-protection on the drive
3.  The progress update inside the wiping loop could potentially slow down the process
4. There's no timeout mechanism in service, so if the drive becomes unresponsive ...

Solution: 
we should separate check condition and run again of `wipeDrivePri()` and `quickFormat()`
	- Check `WipeResult` if we should do it again for `Wipe`
	- give `DriveMetadata` a default value if get error while `getDriveMetadata()`



#
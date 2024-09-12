
# 

service.log search for `WAAPI_MID_DRIVER_IS_LOCKED` to investigate when does it block that drive
Looking for path "path": "G:\\" of the drive you looking for
Google OESIS document  search the method in the log ( "method": 29023 ) to understand drive is lock or not
1 is blocked, 2 is not blocked
```json
[OESIS]: WAAPI_MID_DRIVER_IS_LOCKED IN: {"input": {"method": 29023, "path": "G:\\"}} - OUT: {"result": {"code": 2, "method": 29023, "timestamp": "1725940757", "timing": 0}}
[owc]: Bitlocker check, drive E:\ is not encrypted
[owc]: BitLocker check, no more enumerated devices found - 00F430401
[OESIS]: WAAPI_MID_DRIVER_IS_LOCKED IN: {"input": {"method": 29023, "path": "E:\\"}} - OUT: {"result": {"code": 1, "method": 29023, "timestamp": "1725940912", "timing": 0}}
[DriveManager]: Parse vendor/prod exception: No match found.
[OESIS]: WAAPI_MID_DRIVER_IS_LOCKED IN: {"input": {"method": 29023, "path": "\\\\?\\Volume{d7c49ccd-d738-4081-a730-79f574f1d3b5}\\"}} - OUT: {"result": {"code": 1, "method": 29023, "timestamp": "1725940912", "timing": 16}}
[OESIS]: WAAPI_MID_DRIVER_IS_LOCKED IN: {"input": {"method": 29023, "path": "D:\\"}} - OUT: {"result": {"code": 1, "method": 29023, "timestamp": "1725940912", "timing": 0}}
[DriveManager]: Parse vendor/prod exception: No match found.
[OESIS]: WAAPI_MID_DRIVER_IS_LOCKED IN: {"input": {"method": 29023, "path": "G:\\"}} - OUT: {"result": {"code": 1, "method": 29023, "timestamp": "1725940912", "timing": 16}}
```
look the log between drive is **not block** and drive is **blocked**
is there any call to manually block that drive
search for:  `WAAPI_MID_DRIVER_BLOCK IN` to look for manually call OESIS to block drive

```json
[OESIS]: WAAPI_MID_DRIVER_BLOCK IN:
```
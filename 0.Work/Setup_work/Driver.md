
# ntifs
`ntifs.h` This header file is used by Windows file system and filter driver developers. For a complete list of associated header files see:

- [File system and minifilter drivers](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/_ifsk/)

For the programming guide, see the [File system and minifilter design guide](https://learn.microsoft.com/en-us/windows-hardware/drivers/ifs).

## Source
[microsoft doc](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntifs/)


# DriverEntry
## source
[ microsoft doc ]](https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/driverentry-for-kmdf-drivers)
## Overview
**DriverEntry** is the first driver-supplied routine that is called after a driver is loaded. It is responsible for initializing the driver.
```cpp
NTSTATUS DriverEntry(
  _In_ PDRIVER_OBJECT  DriverObject,
  _In_ PUNICODE_STRING RegistryPath
);
```

## Parameters

_DriverObject_ [in]  
A pointer to a **DRIVER_OBJECT** structure that represents the driver's WDM driver object.

_RegistryPath_ [in]  
A pointer to a **UNICODE_STRING** structure that specifies the path to the driver's Parameters key in the registry.

## Return value

If the routine succeeds, it must return STATUS_SUCCESS. Otherwise, it must return one of the error status values that are defined in _ntstatus.h_.

# String
Most kernel API work with UNICODE_STRING structure (ntdef.h)
```cpp
typedef struct _UNICODE_STRING {
  USHORT Length;
  USHORT MaximumLength;
  PWSTR  Buffer;
} UNICODE_STRING, *PUNICODE_STRING;
```
MaximumLength >= length show us how much more space available to increase without allocate

## Common function
RtlInitUnicodeString
RtlCopyUnicodeString
RtlCompareUnicodeString
RTL_CONSTANT_STRING macro to initialize with a constant string

# IoCreateDevice 
The IoCreateDevice routine creates a device object for use by a driver.
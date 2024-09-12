# Sign in Sign out bug
Vì HID apply cho cả FullBlock và OnAccess như nhau nên vấn đề sẽ nằm ở intergrate.
Bug sẽ nằm ở UI. UI process start chậm so với request insert even

[info] [07-23-24] (13:51:22) GMT+07 (25996:3256) [IPC]: VERBOSE :    Broadcast signal `usb_connection_status_scanner_server.signal`: 

[info] [07-23-24] (13:51:17) +07:00 (25996:12460) [RMP]: Received HID blocked event: \\?\hid#vid_0461&pid_4002&mi_01&col03#8&3aae597c&0&0002#{4d1e55b2-f16f-11cf-88cb-001111000030}\kbd


[info] [07-23-24] (13:51:25) GMT+07 (13268:13396) [OWC_NTF]: "[Application]" Register restart success

đối với broadcast signal `maf` event sẽ xóa ngay sau khi broadcast
còn đối với set status vùng nhớ sẽ ko mất đi, nó sẽ chờ process khác pick up dcs




# Change Mode FullBlock to OnAccess

When change mode on USBfeature.cpp call disable() method of Mode (FullBlock or OnAccess)
disable() FullBlock feature it call `deleteFeatureRegKeyChild();` but when init() FileAccess we didn't init HID
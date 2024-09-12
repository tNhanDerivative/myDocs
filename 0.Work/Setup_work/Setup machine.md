# highlight
[video tutorial](https://www.youtube.com/watch?v=n463QJ4cjsU&t=3438s)
Installing Visual Studio
Installing WDK (Windows Driver Kit)
Installing VMWare Player
Obtaining an OS Disk Image

### Setting up the VM (Virtual Machine)
### Configuring Windows & Installing VMWare Tools
### Setting up VM for Kernel Debugging
### Installing WinDbg
### Configuring VM Windows for Debugging
### Disabling Anti-Virus
### Setting up the Host for Kernel Debugging
### Setting up WinDbg
Testing kernel debugging [19:57](https://www.youtube.com/watch?v=n463QJ4cjsU&t=1197s) Acquiring kdmapper


# meta access
> Mục đích là để lấy tên file cài đặt.

Link admin tài khoản test [Devices | MetaDefender IT Access (opswat.com)](https://metaaccess-testing.opswat.com/inventory/devices/all) chứa tất cả các device manage bởi tài khoản metaaccess cá nhân
=> tải vể file cài đặt để cài meta. dựa vào tên file installer sẽ config

đăng nhập vào tk của anh QA để full feature:
son.tran@opswat.com
P@ssw0rd123456
tạo policies, 
tạo groups device trong Device Group trong Inventory. 
# Cài bản release của meta defender để test
vào link [Windows: Persistent — TeamCity (opswat.com)](https://tcbuild.opswat.com/buildConfiguration/MetaAccess_OpswatClient_Qa_WindowsPersistent?mode=builds#all-projects))

![[TeamCity1.png]]

![[TeamCity2.png]]

## Đối với Meta Endpoint
đổi tên file tải về y hệt tên file mà mình tải trên MetaAccess thì mới cài đặt dc
Sau đó mình bê file `GearAgentService.exe` đã build code moi trong [source_name]/bin sang ben máy ảo. Lưu ý phải dùng Task Manager tắt hết các Opswat process đi

# Đối với Unmanaged
Không cần đổi tên
Copy file `mde-service.exe` để cài
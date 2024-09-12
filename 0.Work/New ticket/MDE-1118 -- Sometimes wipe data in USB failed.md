
# Write file error
check `WipeResult` 
WriteFile Error 433: 
WriteFile Error 1110

![[3_mde1118.png|1200]]
![[1_mde-1118.png]]
![[0_mde-1118.png]]


# Internal :: wipeDrivePri()
1. The code doesn't check if the drive is removable before attempting to wipe it. Wiping a non-removable drive could lead to system instability.
2. There's no check for write-protection on the drive, which could cause the wipe to fail.
3. The progress update inside the wiping loop could potentially slow down the process on very fast drives.
4. There's no timeout mechanism, so if the drive becomes unresponsive, the wipe process could hang indefinitely.
5. The code doesn't handle power loss scenarios, which could leave the drive in an inconsistent state if the wipe is interrupted.
6. There's no verification step after wiping to ensure all data was actually overwritten.
7. The code uses a fixed buffer size of 32 MiB, which might be too large for some systems with limited memory.

Solution check `WipeResult`


## Disfragmenting

https://wiki.tnonline.net/w/Btrfs/Balance#:~:text=btrfs%20balance%20is%20used%20to,normal%20usage%20of%20the%20filesystem.


```bash
lsblk -f
```

btrfs
```
sudo btrfs filesystem usage /
```
|Type                                   |Description|
|--------------------------|-----------------------------------------------------------------------------------------------|
|DATA                                  |Stores normal user file data.                                                                                                              |
|METADATA                         |Stores internal metadata. Small files can also stored inline.                                                          |
|SYSTEM                             |Stores mapping between physical devices and the logical space representing the filesystem.|
|UNALLOCATED                 |Any unallocated space in the filesystem.                                                                                      |
|_**NOTE**: Only the type of data that the chunk is allocated for can be stored in that block group._|                                                   

### disfragmenting
```
sudo btrfs balance start /
```



## Resize

shrink partitions 2 Gigabyte
```
 sudo btrfs filesystem resize -2G /data
```

extend partitions 1 Gigabyte
```
 sudo btrfs filesystem resize +1G /data
```




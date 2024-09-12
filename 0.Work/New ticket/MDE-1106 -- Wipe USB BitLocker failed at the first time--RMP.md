
root cause: 
	- Ko lấy dc Metadata 
	- Ko write file dc
=> Có thể check quyền write
[error] [08-14-24] (22:28:51) +07:00 (15916:3240) [RMP]: Get GetDiskFreeSpace Error 2150694912
[error] [08-14-24] (22:28:51) +07:00 (15916:3240) [RMP]: Get GetVolumeInformation Error 2150694912
[info] [08-14-24] (22:28:51) +07:00 (15916:3240) [RMP]: Start wipe drive I:\ size: 209715200 
[error] [08-14-24] (22:28:51) +07:00 (15916:3240) [RMP]: WriteFile Error 2150694912

trong `FullBlockController::wipeDrive()`
```cpp
  do {
    wipeResult = Internal::wipeDrivePri(driveID);
    formatSuccess = Internal::quickFormat(driveID, oldInfo);
    ++currentRetryTime;
  } while (wipeResult == WipeResult::DiskSizeErr && formatSuccess &&
           currentRetryTime <= maxTryTime);
```
ta thêm điều kiện để check `wipeResult == WipeResult::DiskSizeErr`



# Solution Unlock bitlocker on UI

In the provided codebase context, the wipe action is initiated in the UI through the following flow:

1. In FeatureHome.qml, there's a component called `comAction` which is responsible for showing a context menu with various actions, including the wipe action.
2. The wipe action is added to the context menu in the `prepareOptiom` function of `comAction`:

```qml
if (isWipe)
    listAction.modelAction.push(CONST.WIPE_ID)
```

```qml
        function prepareOptiom(driveID, driveName, driveType, driveIndex, isEject, isErase, isWipe, actionAllowed, hasActionRunning, dialogShowed, decrypted) {
            resetData()
            comAction.dialogShowed = dialogShowed
            comAction.driveIndex = driveIndex
            comAction.driveID = driveID
            comAction.driveName = driveName
            comAction.driveType = driveType
            comAction.supportEject = isEject
            comAction.supportErase = isErase
            comAction.supportWipe = isWipe
            comAction.actionAllowed = actionAllowed
            comAction.hasActionRunning = hasActionRunning
            if (isEject)
                listAction.modelAction.push(CONST.EJECT_ID)
            if (isErase)
                listAction.modelAction.push(CONST.ERASE_ID)
            if (isWipe)
                listAction.modelAction.push(CONST.WIPE_ID)
            listAction.model = listAction.modelAction
            comAction.visible = true
            comAction.decrypted= decrypted
        }
```

3. When the user selects the wipe action from this menu, it triggers the `showWipeDialog` function:

```qml
case CONST.WIPE_ID:
    comAction.showWipeDialog(comAction.driveID, comAction.driveName)
    comAction.resetData()
```
```qml
        ListView {
            id: listAction
            property var modelAction: []
            model: modelAction
            delegate: Item {

                MouseArea {
                    id: maAction
                    anchors.fill: parent
                    hoverEnabled: true
                    enabled: parent.enabled
                    visible: enabled
                    cursorShape: enabled ? Qt.PointingHandCursor : Qt.ArrowCursor
                    onClicked: {
                        switch (modelData) {   // in delegate
                        case CONST.WIPE_ID:
                            if (comAction.decrypted) {
                                comAction.showWipeDialog(comAction.driveID, comAction.driveName)
                            } else {
                                mediaSecurity.checkEncryptedDrive(comAction.driveIndex)
                            }
                            comAction.resetData()
                            break
                        }
                    }
                }
            }
        }
```

4. The `showWipeDialog` function creates a dialog component for wiping:

```qml
function showWipeDialog(driveID, driveName) {
    var componentt = Qt.createComponent(CONST.WIPE_DIALOG_PATH)
    if (componentt.status === Component.Ready) {
        iUSBHome.wipeDialog = componentt.createObject(iFeatureHome, {
            "parent": iFeatureHome,
            "driveID": driveID,
            "driveName": driveName
        })
    }
}
```


This dialog component would then contain the UI elements for confirming the wipe action and initiating the actual wipe process, which would then call the `wipeDrive` function in the USBBlockedMode class.

# Solution
Yes, we can pass iItem.decrypted into ComAction. In the Home.qml file, when preparing the options for ComAction, we can include the decrypted status as a property. This can be done by modifying the prepareOptiom function call to include the decrypted status:

```qml
function prepareOptiom(driveID, driveName, driveType, driveIndex, isEject, isErase, isWipe, actionAllowed, hasActionRunning, dialogShowed, decrypted) {
    // ... existing code ...
    comAction.decrypted = decrypted
    // ... rest of the function ...
}
```

Then, when `iItem` calling `comAction.prepareOptiom` , include `iItem.decrypted` as the last parameter:

```qml
comAction.prepareOptiom(driveID, driveName, driveType, index, isEject, isErase, isWipe, actionAllowed, hasActionRunning, dialogShowed, iItem.decrypted)
```

Now, ComAction will have access to the decrypted status, and you can use it directly in the WIPE_ID case:

```qml
case CONST.WIPE_ID:
    if (comAction.decrypted) {
        comAction.showWipeDialog(comAction.driveID, comAction.driveName)
    } else {
        mediaSecurity.checkEncryptedDrive(comAction.driveIndex)
    }
    comAction.resetData()
    break
```

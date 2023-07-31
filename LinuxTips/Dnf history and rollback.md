
## Dnf history
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/assembly_handling-package-management-history_managing-software-with-the-dnf-tool#:~:text=1.-,Reverting%20a%20single%20DNF%20transaction%20by%20using%20dnf%20history%20undo,history%20undo%20installs%20it%20back.

1. Show 10 lines of dnf history table
```bash
sudo dnf history list| head -10
```
 ID     | Command line                                                                                                                              | Date and time    | Action(s)      | Altered
------|---------------------------------------------------------------------------------------------|-----------------|--------------|---
    59 | update                                                                          | 2023-07-19 16:25 | Upgrade        |   13   
    58 | -y install --disablerepo=* /tmp/akmods.VuQewjud/results/kmod-nvidia-6.3.12-200. | 2023-07-16 17:21 | Install        |    1   
    57 | update                                                                          | 2023-07-16 17:14 | C, E, I, U     |   66 E<
    56 | -y install --disablerepo=* /tmp/akmods.0CyCQNDI/results/kmod-nvidia-6.3.11-200. | 2023-07-09 21:45 | Install        |    1 > 
    55 | update                                                                          | 2023-07-09 21:36 | C, E, I, O, U  |  200 EE
    54 | -y install --disablerepo=* /tmp/akmods.hbLXFZyp/results/kmod-nvidia-6.3.6-200.f | 2023-07-09 21:12 | Upgrade        |    1   
    53 | -y install --disablerepo=* /tmp/akmods.gn1KDB8f/results/kmod-nvidia-6.3.7-200.f | 2023-06-29 17:23 | Upgrade        |    1   
    52 | -y install --disablerepo=* /tmp/akmods.x8lOiaUA/results/kmod-nvidia-6.3.8-200.f | 2023-06-29 17:20 | Install        |    1   

2. Show info of the transaction
```bash
sudo dnf history info [transaction_id]
```

```
Packages Altered:
    Install kmod-nvidia-6.3.12-200.fc38.x86_64-3:535.54.03-1.fc38.x86_64 @@commandline
```

3. Undo the transaction
```bash
dnf history undo transaction_id
```
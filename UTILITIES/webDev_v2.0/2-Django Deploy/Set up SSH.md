[Connect to Google cloud link](https://cloud.google.com/compute/docs/instances/connecting-advanced#thirdpartytools)

## Window



>- Set up Public key Auth

>- Dissable password based auth

### 1.1 Check for existing SSH key

**Win10** <br>

- Verify if OpenSSH client is installed <br>
![win10 Check SSH key](win10CheckSSHkey.PNG "Check SSH key on win10")
<br>

- Check for existing SSH key

**LINUX** <br>
<https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys>



  

## Linux
<https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604>

The first step is to create a key pair on the client machine (usually your computer):

```
ssh-keygen

```
After entering the command, you should see the following output:

```
OutputGenerating public/private rsa key pair.
Enter file in which to save the key (/your_home/.ssh/id_rsa):
```
Type in your new file `/home/trongnhan/.ssh/< project_name>`
then copy to the server
The syntax is:

```
ssh-copy-id username@remote_host

```

You may see the following message:
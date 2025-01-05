## Generating ssh key
https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604
The first step is to create a key pair on the client machine (usually your computer):

```bash
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

```bash
ssh-copy-id username@remote_host

```

You may see the following message:



## ssh-agent
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys
Most use for github
It's a program that runs in the background and keeps your key loaded into memory, so that you don't need to enter your passphrase every time you need to use the key. The nifty thing is, you can choose to let servers access your local `ssh-agent` as if they were already running on the server. This is sort of like asking a friend to enter their password so that you can use their computer.

1. Start the ssh-agent in the background.
    
    ```shell
    $ eval "$(ssh-agent -s)"
    > Agent pid 59566
    ```
    
    Depending on your environment, you may need to use a different command. For example, you may need to use root access by running `sudo -s -H` before starting the ssh-agent, or you may need to use `exec ssh-agent bash` or `exec ssh-agent zsh` to run the ssh-agent.
    
2. Add your SSH private key to the ssh-agent. If you created your key with a different name, or if you are adding an existing key that has a different name, replace _id_ed25519_ in the command with the name of your private key file.
    
    ```shell
    ssh-add ~/.ssh/nhanGithub
    ```
    
3. Add the SSH public key to your account on GitHub.
### automatically launches `ssh-agent` 
On most computers, the operating system automatically launches `ssh-agent` for you.
But on server or window you have to add some bash code to .bashrc
https://gist.github.com/darrenpmeyer/e7ad217d929f87a7b7052b3282d1b24c
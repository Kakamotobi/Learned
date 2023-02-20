# Development Environment

## Table of Contents
- [What is a Development Environment?](#what-is-a-development-environment)
- [Local, Virtual, Remote Machine/Computer](#local-virtual-remote-machinecomputer)
  - [Local and Remote Computers](#local-and-remote-computers)
  - [Virtual Machine](#virtual-machine)
    - [SSH into Ubuntu ft. VirtualBox](#ssh-into-ubuntu-ft-virtualbox)
      - [SSH and Ubuntu Tips and Trouble Shooting](#ssh-and-ubuntu-tips-and-trouble-shooting)
        - [Key Fingerprint](#key-fingerprint)
        - [First Time Connecting to SSH Host](#first-time-connecting-to-ssh-host)
        - [SSH Fingerprint Mismatch](#ssh-fingerprint-mismatch)
    - [Ubuntu Configurations](#ubuntu-configurations)
- [Remote Development over SSH using VSCode](#remote-development-over-ssh-using-vscode)

## What is a Development Environment?
> A development environment is a set of tools and functionalities that enable a programmer to develop, test, and debug the source code of an application or a program. | educative.io

- A development environment is not limited to a developer's **local environment**. It also includes **virtual environments** and **remote environments**.

## Local, Virtual, Remote Machine/Computer
### Local and Remote Computers
- A **Local Computer** refer to a developer's computer at hand, which can be accessed without a network.
- A **Remote Computer** refers to a computer that normally needs to be accessed through a network.
- Code written in local (host) needs to be copied, and/or built/compiled, executed on a a remote computer (target) such as a server to be deployed.
  - Local environments can be different from the remote environment. Therefore, we can install Virtual Machines in our local to confirm that the code runs well on the target system.
### Virtual Machine
- We don't always have access to a physical device and/or it may not be feasible to use a physical devices of all systems for development.
- A **Virtual Machine** is basically running an operating system in your computer (Ex: in an application window) while behaving like a separate computer.
  - i.e. running multiple operating systems at the same time from one hardware.
  - **VirtualBox** is a free virtual environment that we can use to run virtual machines.
- _Note_
  - **A virtual machine running on a host system does not have access to the host's hardware.**
    - i.e. it is merely running on a virtual drive that has been allocated to it.
#### SSH into Ubuntu ft. VirtualBox
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Development%20Environment/refImg/virtual-machine-ssh-connection.png" alt="SSH into Virtual Machine" width="80%" />
</p>

- In VirtualBox, setup a virtual machine with the desired settings.
  - Insert an Ubuntu disk image file(iso) in the virtual CD drive. Run the VM.
- Setup SSH to communicate between the VM and localhost computer.
  - **Install an SSH server on the VM**: `$ sudo apt install openssh-server`
    - Check SSH install status: `$ sudo systemctl status ssh`
    - If status is inactive, enable SSH: `$ sudo systemctl enable ssh --now`
  - **Open an SSH port on the VM**.
    - Check if firewall is enabled: `$ sudo ufw status`
    - If firewall is enabled, allow SSH for firewall: `$ sudo ufw allow ssh`
    - Port 22 is open for SSHD by default.
      - Check which ports are open on system: `$ sudo lsof -i -P -n | grep LISTEN`
  - **Add the SSH connection to VirtualBox Network Settings**.
    - VM should be attached to NAT.
    - Advanced &rarr; **Port Forwarding** &rarr; Add SSH Connection
      - Name: ssh (name of this port forwarding rule)
      - Protocol: TCP
      - Host Port: 2222 (any unused port greater than 1000)
      - Guest Port: 22
        - The port that the sshd server will be listening on for connections.
      - Host IP: 127.0.0.1 (localhost)
      - Guest IP: 10.0.2.15
    - Basically, only TCP packets coming to the VM from 127.0.0.1 (host) in the local network will be redirected to 10.0.2.15.
  - **Install SSH client on host**.
    - SSH is built into Terminal on MacOS.
  - **Start SSH Session with the guest OS from host**: `$ ssh -p 2222 ubuntuLoginUsername@localhost`
    - *Important: the remote host is set to "localhost" or "127.0.0.1" here because the VM we're trying to connect to is on local, which is then redirected to "10.0.2.15" as specified above. If instead of a VM, we need to connect to a remote server, this should be the IP address or domain name of that remote server.*
    - Enter password (Ubuntu user).
    - You have an SSH connection into the Ubuntu VM now.
    - On remote, use `chmod` to modify access/permission for files/directories.
      - Check access mode for directory: `$ stat directory`
  - ***Optional but recommended:***
    - Use **SSH key-based authentication** instead of using username and password.
      - Generate a pair of SSH keys from the SSH client machine: `$ ssh-keygen -t rsa`
        - This creates two keys `id_rsa.pub` and `id_rsa` at `~/.ssh`.
        - Keep the private key on the SSH client machine.
        - Move the public key to the SSH server machine: `ssh-copy-id remote_host`
          - The public key will be copied to the remote's authorized keys file.
      - When trying to connect, the server uses the public key to generate a message that the client can read using the private key. Then, the client sends an appropriate message to the server, which the server checks and then starts the connection.
    - Disable password only authentication.
      - Open the SSHD configuration file at `/etc/ssh/sshd_config` and set `PasswordAuthentication no`.
      - Reload the SSH daemon: `$ sudo systemctl reload ssh`.
##### SSH and Ubuntu Tips and Trouble Shooting
- `man sshd` for more information.
###### Key Fingerprint
- Each host has its own unique key, which is used to identify itself to clients trying to connect to it.
- When a client attempts to connect, the host presents its public host key.
  - The host usually stores it in its `/etc/ssh/ssh_host.pub` file.
- The client compares that key against its list of keys in its own database(file) to verify that everything's the same.
###### First Time Connecting to SSH Host
- The purpose is to check if you agree to use the specified key fingerprint to connect.
- If `yes`, type in password of the host account.
  - If the login was successful, the key fingerprint is saved in the `~/.ssh/known_hosts` file and won't ask again.
- Example
  ```zsh
  The authenticity of host '<host>' can't be established.
  ED25519 key fingerprint is <key fingerprint>.
  This key is not known by any other names
  Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
  Warning: Permanently added '<host>' (ED25519) to the list of known hosts.
  <username>@localhost's password: 
  ```
###### SSH Fingerprint Mismatch
- This occurs when the key fingerprint that the host presented and the key fingerprint that the client has saved in its list of SSHs does not match.
- Possible Causes
  - SSH was re-installed.
  - You tried to connect to a different machine at the same IP (Ex: connecting through a load balancer).
  - You are being attacked by a man-in-the-middle attack, in which is intercepting and rerouting your SSH connection to a different host of their own.
- Example
  ```zsh
  @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
  @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
  Someone could be eavesdropping on you right now (man-in-the-middle attack)!
  It is also possible that a host key has just been changed.
  The fingerprint for the ED25519 key sent by the remote host is
  <key fingerprint presented by remote host>.
  Please contact your system administrator.
  Add correct host key in /Users/clientusername/.ssh/known_hosts to get rid of this message.
  Offending ED25519 key in /Users/clientusername/.ssh/known_hosts:6
  Host key for [localhost]:2223 has changed and you have requested strict checking.
  Host key verification failed.
  ```
#### Ubuntu Configurations
- **Setting Ubuntu Root Password.**
  - _Note: root is different from the account that you created when installing Ubuntu._
  - Set password for root.
    ```zsh
    $ sudo passwd
    ```
  - Login as root.
    ```zsh
    $ su
    ```
    ```zsh
    $ whoami
    ```
- **Add User Account(s) with Regular Account Privileges.**
  - These accounts will be used instead of root for most purposes.
  ```zsh
  # adduser <new username>
  ```
- **Grant Privileges to User(s).**
  - i.e. add user to the `sudo` group.
  - Users can now use the `sudo` command before to run them with superuser privileges.
  ```zsh
  # usermod -aG sudo <username>
  ```
- **Set Up a Basic Firewall.**
  - The UFW firewall can be used to make sure that connections are limited to only certain services.
  - Check application profiles that are currently registered with UFW.
    ```zsh
    # ufw app list
    ```
  - Allow OpenSSH since we need to use SSH to connect to this remote server.
    ```zsh
    # ufw allow OpenSSH
    ```
  - Enable firewall
    ```zsh
    # ufw enable
    ```
  - Check firewall status to confirm that the firewall is allowing SSH connections.
    ```zsh
    # ufw status
    ```
- **Configure SSH Access for New Users*.*
  ```zsh
  ssh <username>@<server ip address>
  ```

## Remote Development over SSH using VSCode
- Editting and debugging on a remote machine from local using VSCode.
### Process
- Install the "Remote - SSH" VSCode extension.
- Use the Remote Status bar (bottom left corner in VSCode) for quick Remote - SSH commands.
  - Click "Connect to Host..." and type in "remote_username@remote_host".
  - Or, set an SSH config file `~/.ssh/config`.
    ```
    Host alias/abbreviation for SSH server (remote)
      HostName ip address or domain name of remote
      User username on remote
      Port 2222
    ```
### Some Troubleshooting
- Specify port number in `~/.ssh/config`.
- VSCode `settings.json`
  - Increase connect timeout: `"remote.SSH.connectTimeout": 60`
  - Specify platform for particular host: `"remote.SSH.remotePlatform": { "ubuntu-VirtualBox": "linux" }"`
  - Check if VSCode is waiting on a prompt: `"remote.SSH.showLoginTerminal": true`
  - Specify that the SSH host is not running Windows: `"remote.SSH.useLocalServer": false`

## Reference
[What is a development environment? - educative.io](https://www.educative.io/answers/what-is-a-development-environment)  
[system installation - Is it safe to answer "erase disk and install Ubuntu" on a virtual machine? - Ask Ubuntu](https://askubuntu.com/questions/499894/is-it-safe-to-answer-erase-disk-and-install-ubuntu-on-a-virtual-machine)  
[How to Install Ubuntu on VirtualBox](https://www.makeuseof.com/install-ubuntu-virtualbox/)  
[How to SSH Into a VirtualBox Ubuntu Server](https://www.makeuseof.com/how-to-ssh-into-virtualbox-ubuntu/)  
[How To Use SSH to Connect to a Remote Server | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server)  
[What is a SSH key fingerprint and how is it generated? - Super User](https://superuser.com/questions/421997/what-is-a-ssh-key-fingerprint-and-how-is-it-generated)  
[Initial Server Setup with Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)  
[Connect over SSH with Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh-tutorial)  
[Developing on Remote Machines using SSH and Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh)  

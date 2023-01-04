# Development Environment

## Table of Contents
- [What is a Development Environment?](#what-is-a-development-environment)
- [Local, Virtual, Remote Machine/Computer](#local-virtual-remote-machinecomputer)
  - [Local and Remote Computers](#local-and-remote-computers)
  - [Virtual Machine](#virtual-machine)
    - [SSH into Ubuntu ft. VirtualBox](#ssh-into-ubuntu-ft-virtualbox)
- [Remote Development](#remote-development)

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
[How to Install Ubuntu on VirtualBox](https://www.makeuseof.com/install-ubuntu-virtualbox/)  
[How to SSH Into a VirtualBox Ubuntu Server](https://www.makeuseof.com/how-to-ssh-into-virtualbox-ubuntu/)  
[How To Use SSH to Connect to a Remote Server | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server)  
[Connect over SSH with Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh-tutorial)  
[Developing on Remote Machines using SSH and Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh)  

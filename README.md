# SSH Back and Forth

## Story

You've previously learned that the key to a dev's heart is through SSH.
The time has come to use your key!

## What are you going to learn?

- What are services in Ubuntu
- How to start, stop, restart enable/diable services
- How to check that a service is enabled or disabled to load on boot
- Configure port forwaring in VirtualBox
- How to connect your host machine to your vitrual machine with `ssh`
- How to copy your SSH key to a server
- How to configure SSH server, modify port, disable login with password

## Tasks

1. Download [the image](https://github.com/CodecoolBase/short-admin-vms/releases/latest/download/ubuntu-18.04-nossh.ova) and import it. Since this is a *server* image there's no GUI, clipboard or copy-pasta this time.
    - The image file is downloaded
    - The image file imported as a VM

2. Start the server VM and and login with using `ubuntu` for both username and password.
    - Logged in as `ubuntu`, the prompt in the terminal is ubuntu&commat;vm

3. Install the SSH server using `apt-get`.
    - `apt-get update` is executed before installation to refresh available package information
    - `ssh -V 2>&1 | cut -d' ' -f1` displays the installed version of the SSH package, e.g `OpenSSH_7.6p1` or similar

4. Enable the SSH daemon to start on boot using `systemctl`.
    - The status of the `sshd` service is `enabled`

5. Configure the guest VM's port forwarding and forward port 22 from the guest to port 2222 on the host.
    - The VM is using NAT networking
    - Port 2222 on the host is forwarded port 22 of the guest

6. Copy your SSH public key to the guest OS in order to be able to login without ever typing a password, using only key-based authentication.
    - Able to SSH into the guest via `localhost` using port 2222 with the `ubuntu` user by typing its password
    - `ssh-copy-id` is used to copy the public SSH key to the guest for the `ubuntu` user
    - Able to SSH into the guest via `localhost` using port 2222 with the `ubuntu` user *without* typing any password

7. Modify the SSH server's default port to from 22 to 22222.
    - Nano editor is used to open the config file
    - The port has modifyed to 22222
    - Password authentication is disabled
    - You saved your changes, and closed nano
    - The SSH service has restarted and now listens on the new port

8. Modify your VM's port forwarding rules so that port 22222 from the guest is forwarded to port 2222 on the host.
    - Port 2222 on the host is forwarded port 22222 on the guest
    - Still able to SSH into the guest via `localhost` using port 2222 with the `ubuntu` user *without* typing any password

## General requirements

None

## Hints

- If you have an SSH server listening on port 22 on your _host machine_ you won't be able to map to this port in your VirtualBox machine's network settings, because it's already taken (watch out for this if you have a Linux or macOS box), try this command to see if the port is taken: `sudo lsof -i -P | grep LISTEN | grep 22`
- Check that the `ssh service` is loading on boot with the following command:

```
sudo systemctl list-unit-files --type service --all | grep sshd
```

- The default port of SSH server is `22`. To modify it, you need to edit the `sshd_config` file in the `/etc/ssh/` folder
- To open `sshd_config` file use `nano` editor as a superuser:

```bash
sudo nano /etc/ssh/sshd_config
```

- In the `sshd_config` file `#` is used for commenting a line. You should delete it if you want to modify the default `22` port

- **When you are reading background materials:**
  - [Install/enable SSH server](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/#enabling-ssh-on-ubuntu) - Focus on the _Enabling SSH on Ubuntu_ section, but you can skip the part on the `ufw` firewall command

## Background materials

- <i class="far fa-exclamation"></i> [Managing services in Ubuntu](project/curriculum/materials/pages/unix/managing-services.md)
- [Install/enable SSH server](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/#enabling-ssh-on-ubuntu)
- [Set port forwarding on VirtualBox](https://www.techrepublic.com/article/how-to-use-port-forwarding-in-virtualbox/)
- [How `ssh-copy-id` works](https://www.ssh.com/ssh/copy-id)
- [How to change SSH server port](https://www.ubuntu18.com/ubuntu-change-ssh-port/)

# Virtual Machines Init Setups

## VirtualBox Setting
1) Add a `Host-only Adapter` network as second adapter. (first one used for internet access)
2) In `Adapter 1` add a new `Port Forwarding`

    | Name | Protocol | Host Port | Guest Port |
    | ---- | -------- | --------- | ---------- |
    | ssh  |TCP       | 4001      | 22         |


login with root user and do ass following:

## Update
    apt update

## Upgrade
If there was any packages for upgrade:

    apt upgrade

## Install SSH

    apt install openssh-server

## Set Static IP
First see your device name:

    ip addr show
Normally the first one is `lo`, second one is your `Adapter 1` which is used to access the internet and the third one should be `Adapter 2` which has not an ip. In my case, my second adapter is `enp0s8`, in the following where ever I use `enp0s8` you should use yours.

Then open `network/interfaces`:

    nano /etc/network/interfaces
Then add these lines at the end of the file:

    # The secondary network interface (host-only)
    auto enp0s8
    iface enp0s8 inet static
        address 192.168.56.101
        netmask 255.255.255.0

## SSH Config
Uncomment `Port 22` in ssh_config. You can use your prefered port too, but it should be the exact port in `Adapter 1 Port Forwarding`.

    nano /etc/ssh/ssh_config

Then find `#Port 22` and remove the `#`.

## Root Access To Remote
If you need to use your root user to connect to your virtual machine, do as following.

    nano /etc/ssh/sshd_config

Then find the `#PermitRootLogin prohibit-password` line. (Instead of `prohibit-password` it may to be something else, that's not an issue)

Uncomment that line and change its value:

    PermitRootLogin yes

Now you can connect to your virtual machine server.
 

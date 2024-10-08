## Setup -- Windows 11 Host System + WSL (Ubuntu 22.04). VirtualBox for VMs. Ubuntu 22.04 Server ISO used.

* Setup the Virtual Box VM. Make sure the VM has Bridged Adapter and not the NAT.

* Boot up the VM. You will find the IP address in the startup logs, if not, use either ip addr or ifconfig (sudo apt-get install net-tools)

* Go to the Host System and ping the remote VM. If it succeeds, great, else something is not correct.

* Now, set up the static IP address. First detect the Host DNS Server Address (`netsh interface ipv4 show dnsservers`)

 One can also use the commands to check the IP details of the Default Gateway and DNS Server on Windows with the commands -- 

 `ipconfig /all` 
 
 `nslookup`

 __NOTE: Here, the default gateway IP address and DNS may change frequently if one is using a Hotspot from the Mobile and have set the static IP, default gateway, and DNS addresses in the VMs. In such scenarios, just check the host network details as mentioned above, and change them manually in the VM. Even the static IP assigned will need to be changed to same range as the gateway.__
 
* After doing so, go to the /etc/netplan/00-*.yaml (just press tab to autofill) file, and edit the content like so -- 

```
network:
    ethernets:
        <port>: # whatever is mentioned by default say enp0s3, etc
            addresses: [] # whatever static ip address you want to give. say 192.168.100.100
            gateway4: # preferably the first ip of the above mentioned ip address. NOTE this can differ, so just run ipconfig in the Host system which will mention the wireless or ethernet gateway ip address, just add it here. If incorrectly configured, it will be an irritating situation.
            nameservers:
                addresses: [] # Add that IP address from the netsh command, choosing the correct value for the ethernet or the wifi module.
    version: 2
```

* run `sudo netplan apply`. Ignore warnings, but if there is an error, something is not correct. Check if indentation mistakes or incorrectly set values.

* If everything is correct, you should be able to ping the static ip address from the Host to the Remote system or any other system to the VM on the same network. Also, you should be able to connect to the internet from the VM.

* This guy's [Link](https://www.youtube.com/watch?v=haufmkuKq9A&t=874s) is very helpful! 

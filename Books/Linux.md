### Centos7 connect the Internet via command-line (must first use "sudo -s" to change the root mode)

* edit file "/etc/sysconfig/network-scripts/ifcfg-wlp3s0"
  - the name of wlan interface is "wlp3s0"
  ```
    DEVICE=wlp3s0
    TYPE=Wireless
    ONBOOT=yes
    MM_CONTROLLED=no
    BOOTPROTO=dhcp
    ssid="CU_87QA"
    psk="nat7b9vw"
  ```
* and the command as follow; if the step follow cannot work, then reboot the machine
  - ip addr                               -> check the state of network device  
  - ip link set wlp3s0 up                 -> open wlp3s0(wlan device) for ifconfig can print it
  - ip link show wlp3s0                   -> print status of wlp3s0
  - iw dev                                -> print the device which can be used
  - iw wlp3s0 scan | grep -i ssid
  - vim /etc/sysconfig/network-scripts/ifcfg-wlp3s0       -> config wlp3s0 (necessary)
  - systemctl restart NetworkManager      -> active the configure
  - wpa\_supplicant -B -i wlp3s0 -c <(wpa\_passphrase "CU\_87QA" "nat7b9vw")      -> active wlp3s0 (necessary) 
  ```
    afte this step, the state of wlp3s0 should be "up" 
    state of wlp3s0 can get the state via "ip addr"
  ```
  - dhclient wlp3s0                       -> auto allocate ip address (necessary)

### configure vim
* update origin vim to vim 8.1 for ycm
```c++
```

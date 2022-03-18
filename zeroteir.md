# Setting up Zerotier on USG4

1. Create a new zeroteir network. Nothing besides the default at this point
2. Install the zerotier binary on the USG. The setup script may work but it is easy enough to repackage the binary without XZ compression
    ```
    mkdir -p /config/data/firstboot/install-packages
    cd /config/data/firstboot/install-packages
    curl https://download.zerotier.com/dist/ubiquiti/zerotier-one_mips64.deb --output /config/data/firstboot/install-packages/zerotier-one.deb
    
    mv zerotier-one.deb zerotier-one-xz.deb
    ar -x zerotier-one-xz.deb
    xz --decompress control.tar.xz
    xz --decompress data.tar.xz
    gzip control.tar
    gzip data.tar
    ar -r zerotier-one.deb debian-binary control.tar.gz data.tar.gz
    sudo dpkg -i zerotier-one.deb
    ```
3. Join the network on the USG
  ```
  zerotier-cli join XXXXXXX
  ```
4. On the zeroteir client configuration, disable DHCP for that USG client and set a static IP.
5. Add a static route in zeroteir to your lan network and set the next hop to the static IP you set for the USG
6. Add a new network in the USG that matches the zeroteir network and disable DHCP. 
  * You don't want to set the VLAN here so use a different LAN interface besides LAN1. I had LAN2 and LAN3 open so I just set it to 3.
7. Add a static route to the network and set the next hop to the zeroteir network interface.
  * This step may not be necessary but that is what I did.
8. Add some VPS as a client or any other device and you should be able to ping going into and out of your home LAN.
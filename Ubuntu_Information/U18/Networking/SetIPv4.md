Files are located under /etc/netplan

You should see a file in there called 50-cloud-init.yaml. Edit it.

You should already see your interface listed there. If this is a bran new install, it likely has dhcp settigns applied.
To hard set IPs add/change the following:

```
network:
  ethernets:
    INTERFACE_NAME:
      dhcp4: no
      addresses: [X.X.X.X/XX]    #IP Address/Netmask
      gateway4: X.X.X.X
      nameservers:
        addresses: [X.X.X.X] #multilples seperated by a comma, no space [x.x.x.x,x.x.x.x]  
```            

Setting multiple interfaces just lists each interface with its settings under ethernets:

```
network:
  ethernets:
    INTERFACE_NAME:
      dhcp4: no
      addresses: [X.X.X.X/XX]    #IP Address/Netmask
      gateway4: X.X.X.X
      nameservers:
        addresses: [X.X.X.X] #multilples seperated by a comma, no space [x.x.x.x,x.x.x.x]
    INTERFACE_NAME:
      dhcp4: no
      addresses: [X.X.X.X/XX]    #IP Address/Netmask
      gateway4: X.X.X.X
      nameservers:
        addresses: [X.X.X.X] #multilples seperated by a comma, no space [x.x.x.x,x.x.x.x]  
```

Apply changes with: netplan apply

1. Apt install bind9

2. Set it for IPv4 mode. Edit /etc/default/bind9 and add a -4 to OPTIONS:
```
    OPTIONS=-4 -u bind"
```

3. Add ACLs. Edit /etc/bind/named.conf.options and add the following above options.
```
    acl "trusted" {
      X.X.X.X/XX;        #Network IP/Netmask... Can also just list IPs (X.X.X.X). Multiple entries entered under with new line.
    };                     #End all lines with a semicolon.
```

4. In the same file, under options add the following:
```
    options {
        directory "/var/cache....

        recursion yes;                 
        allow-recursion { trusted; };  
        listen-on { X.X.X.X; };         #Interface(s) to listen on   
        allow-transfer { none; };      

        forwarders {              #Forwards to Google. If closed network, just leave commented out (unless forwarding somewhere else...)
                8.8.8.8;
                8.8.4.4;
        };

        .....
    };
```

5. Now add the zones by editing /etc/bind/named.conf and adding the following:
```
    zone "YOUR_DOMAIN" {          #i.e. example.local
        type master;
        file "/etc/bind/zones/db.YOUR_DOMAIN";   #i.e. /etc/bind/zones/db.example.local
        allow-transfer { X.X.X.X; };    #secondary DNS -- if wont have, leave out.
    };

    zone "X.X.X.in-addr.arpa" {        #backwards of network IP. i.e. 192.168.100.0 network would be 100.168.192.in-addr.arpa.
        type master;                   #Also, lower/add as needed to match mask. i.e. 16 subnet would be 168.192.in-addr.arpa.
        file "/etc/bind/zones/db.X.X.X";    #Same as above but in correct order. i.e. db.192.168.100
        allow-transfer { X.X.X.X; };    #secondary DNS -- if wont have, leave out.
    }
```

6. Make the zones folder for easier management and organization: mkdir /etc/bind/zones

7. Now copy the forward lookup template file: cp /etc/bind/db.local /etc/bind/zones/db.YOUR_DOMAIN. The name should match what you specified in step 5's forward lookup config.

8. Edit the copied file and add/edit the following:
    - The line that says "@       IN      SOA     localhost. root.localhost. ("
        - Edit localhost. and make it the current servers FQDN. End with a period. i.e. ns1.example.local.
        - Edit root.localhost and make it admin.YOUR_DOMAIN. End with a period. i.e. admin.example.local.
    - In the next line, where it says Serial, increment the number by 1. i.e. if 2, make it 3 #You should do this anytime you edit this.
    - Delete the three lines at the end of the file that start with the @ sign.
    - At the end of the file, add all nameservers in the domain. End them with a period. Format is like this:
        - IN NS FQDN        #i.e. IN NS ns2.example.local.
    - After the nameservers, add the A records, ending the FQDN with a period. Format is like this:
        - FQDN IN A X.X.X.X     #i.e. host1.example.local. IN A 192.168.100.101
        - You will need to add the DNS servers A record as well.

9. Copy the reverse lookup template file: cp /etc/bind/db.127 /etc/bind/zones/db.X.X.X. Name should match what you specified in step 5's reverse lookup zone config.

10. Edit the copied file and add/edit the following:
    - The line that says "@       IN      SOA     localhost. root.localhost. ("
        - Edit localhost. and make it the current servers FQDN. End with a period. i.e. ns1.example.local.
        - Edit root.localhost and make it admin.YOUR_DOMAIN. End with a period. i.e. admin.example.local.
    - In the next line, where it says Serial, increment the number by 1. i.e. if 2, make it 3 #You should do this anytime you edit this.
    - Delete the two lines at the end of the file -- one starts with an @ sign and the other a 1.0.0
    - At the end of the file, add all nameservers in the domain. End them with a period. Format is like this:
        - IN NS FQDN        #i.e. IN NS ns2.example.local.
    - After the nameservers, add the PTR records, ending the FQDN with a period. First column is the octets of the node in reverse order. Do not include the network portion. i.e. if a /16 network, it'd be the octet4.octet3. Format is like this:
        - X IN PTR FQDN ; X.X.X.X     #i.e. 101 IN PTR host1.example.local. ; 192.168.100.101  
        - You will need to add the DNS servers A record as well.

11. Run "named-checkconf" to check the configs. If nothing returns you are good to go! If you have errors, fix what it says.

12. Restart the bind9 service with systemctl restart bind9

13. Enable the service to start on bootup: systemctl enable bind9

14. Configure clients to point to it and you should be good to go!

# Route-Based

## Install StrongSwan 

To install strongSwan on Ubuntu, use the following commands:  
```
sudo apt update
sudo apt install strongswan
```  

## System Configuration  
The server that hosts strongSwan acts as a gateway, uncomment or add the following line to /etc/sysctl.conf or seperated config file under /etc/sysctl.d/  
```
net.ipv4.ip_forward = 1
```  

Please check apparmor if there is any security related issues, the following commands can be used to disable apparmor:  
```
apparmor_parser -R /etc/apparmor.d/usr.lib.ipsec.charon
apparmor_parser -R /etc/apparmor.d/usr.lib.ipsec.stroke
ln -s /etc/apparmor.d/usr.lib.ipsec.charon /etc/apparmor.d/disable/
ln -s /etc/apparmor.d/usr.lib.ipsec.stroke /etc/apparmor.d/disable/
```

## IPSec  
The following is a sample configuration for /etc/ipsec.conf:  
```
# ipsec.conf - strongSwan IPsec configuration file

config setup
    # strictcrlpolicy=yes
    # uniqueids = no
conn azure
        authby=secret
        type=tunnel
        left=1.1.1.1 # local ip address of the instance
        leftid=20.X.X.X # Public ip address of the instance
        leftsubnet=10.200.1.0/24 # local subnet connecting to the vpn
        right=2.2.2.2 # virtual gateway address
        rightsubnet=10.100.1.0/24 # subnet behind virtual gateway 
        rightid=20.124.160.209
        auto=start
        dpdaction=restart
        keyexchange=ikev2
        ike=aes256-sha1-modp1024
        esp=aes256-sha1
```

/etc/ipsec.secrets   
```
1.1.1.1 2.2.2.2 : PSK 'ishare' 
```


# Reference  
[Google Cloud](https://cloud.google.com/community/tutorials/using-cloud-vpn-with-strongswan)  
[Azure VPN Example COnfiguration](https://github.com/Azure/Azure-vpn-config-samples)  
[cloudnetworking.io](https://cloudnetworking.io/2018/01/03/azure-routed-vpn-with-strongswan-on-linux/)
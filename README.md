# SSH

When setting up SSH on a Cisco device, you don't necessarily need to configure a new IP address specifically for SSH. SSH will be available on any interface with an IP address (as long as it's not blocked by an Access Control List or other security feature).

SSH Server
the name of the RSA keypair will be the hostname and domain name of the router. Let’s configure a hostname:
```
Router(config)#hostname R1
```
And a domain name:
```
R1(config)#ip domain-name aditya.local
```
Now we can generate the RSA keypair:
```
R1(config)#crypto key generate rsa
The name for the keys will be: R1.aditya.local
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable...
[OK] (elapsed time was 3 seconds)
```
When you use the crypto key generate rsa command, it will ask you how many bits you want to use for the key size. How much should you pick?

It’s best to check the next generation encryption article from Cisco for this.  At this moment, a key size of 2048 bits is acceptable. Key sizes of 1024 or smaller should be avoided. Larger key sizes also take longer to calculate.

Once the keypair has been generated, the following message will appear:
```
R1#
%SSH-5-ENABLED: SSH 1.99 has been enabled
```
As you can see above, SSH version 1 is the default version. Let’s switch to version 2:
```
R1(config)#ip ssh version 2
```
SSH is enabled but we also have to configure the VTY lines:
```
R1(config)#line vty 0 4
R1(config-line)#transport input ssh
R1(config-line)#login local
```
This ensures that we only want to use SSH (not telnet or anything else) and that we want to check the local database for usernames. Let’s create a user:
```
R1(config)#username admin password my_password
```
now uou ready
```
PC1#ssh -l admin 192.168.12.1(ip is just example)
```

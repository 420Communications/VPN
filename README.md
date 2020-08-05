# VPN
VPN Configuration Ubuntu 18.0.4

Clone the repo in /etc/
run the following commands to make sure that the files are at the right directories

    cd VPN/
    cp setup.sh /etc/
    cp ipsec.conf /etc/
    cp ipsec.secrets /etc/
    
Now go to /etc/ufw/ and run these commands
    
    rm before.rules
    rm sysctl.conf
    
Now go back to the clone directory and run these 2 commands

    cp before.rules /etc/ufw/
    cp sysctl.conf /etc/ufw/
    cd ..
    rm -r VPN
    chmod +x setup.sh

Now use ./setup to run the bash file and turn on your VPN. Dont forget to edit the IP address of your VPS to the ipsec.conf and change username and password in ipsec.secrets.
Copy the certificate (shown in the end) including begin and end and save it on your system in file named ca-cert.pem 


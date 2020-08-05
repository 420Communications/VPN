# VPN
VPN Configuration Ubuntu 18.0.4


Clone the repo in /etc/
run the following commands to make sure that the files are at the right directories

cd VPN/
cp setup.sh /etc/
cp ipsec.conf /etc/
cp ipsec.secrets /etc/
cd ..
rm -r VPN

now go to /etc/ufw and edit these 2 files:

1. before.rules

  Near the top of the file (before the *filter line), add the following configuration block:
  
    *nat
    -A POSTROUTING -s 10.10.10.0/24 -o eth0 -m policy --pol ipsec --dir out -j ACCEPT 
    -A POSTROUTING -s 10.10.10.0/24 -o eth0 -j MASQUERADE
    COMMIT

    *mangle
    -A FORWARD --match policy --pol ipsec --dir in -s 10.10.10.0/24 -o eth0 -p tcp -m tcp --tcp-flags SYN,RST SYN -m tcpmss --mss 1361:1536 -j TCPMSS --set-mss 1360
    COMMIT

    *filter
    :ufw-before-input - [0:0]
    :ufw-before-output - [0:0]
    :ufw-before-forward - [0:0]
    :ufw-not-local - [0:0]
    
    -A ufw-before-forward --match policy --pol ipsec --dir in --proto esp -s 10.10.10.0/24 -j ACCEPT
    -A ufw-before-forward --match policy --pol ipsec --dir out --proto esp -d 10.10.10.0/24 -j ACCEPT
 
 2. sysctl.conf
 
    The changes you need to make are as follows:
    
      # Uncomment the following line
      net/ipv4/ip_forward=1

      # Ensure the following line is set
      net/ipv4/conf/all/accept_redirects=0

      # Add the following lines along with the others (Lines in step B)
      net/ipv4/conf/all/send_redirects=0
      net/ipv4/ip_no_pmtu_disc=1


## BB10 Devices VPN 

this is a docker container that I built and configured to work as a strongswan VPN for devices. If you wish to duplicate this functionality you may need custom firewall rules. You can view the rules in my Blackberry-10-StrongSwan-VPN-Setup repo

### Run the container as follow
notice I forward the ports that I want my VPN to have access ti
```
sudo docker run --cap-add=NET_ADMIN -d -t -p 80:80 -p 443:443 -p 2121:2121 -p 500:500/udp -p 4500:4500/udp bb10-vpn

```
### Edit ipsec.conf & ipsec.secrets

you will need to edit the ipsec.conf & ipsec.secrets with your own accounts and in the ipsec.conf you will need to adjust

```
conn BB10
  rekey=no
   leftsubnet=0.0.0.0/0
   leftauth=psk
   leftid=54.39.41.29  // replace with your docker host server ip

```
then update ipsec.secrets and add your own users, there are two example ones in there 


## Possible Host firewall rules 

edit the ethernet and ip details for your setup
```
sudo iptables -I DOCKER -p tcp --dport 500 -j ACCEPT
sudo iptables -I DOCKER -p udp --dport 4500 -j ACCEPT
sudo iptables -t nat -I POSTROUTING -s 172.17.0.0/16 -d 10.0.0.0/24 -j SNAT --to-source 54.39.41.29
sudo iptables -A DOCKER-USER -i ens34 -j FILTERS
```


### Docker HUB Link

```
docker pull sw7ft/bb10-vpn

```

[docker hub link](https://hub.docker.com/r/sw7ft/bb10-vpn)

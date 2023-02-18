<h2>This is an ongoing project. I am trying to build a VPN server so that we can host our own VPS and there will e no 3rd party vpn provider.</h2>

vpn setup

go to openwrt portal

if not custom compiled with these packages then go for it otherwise go to 

network > interfaces > system > software

update lists

search for wireguard

>luci-app-wireguard

system > reboot

login again

network > interfaces

add new interface..

name wg0

protocol wireguard vpn

create interface


<h3>get your ovpn file from your vpn provider.</h3> 

if want free use https://opentunnel.net/wireguard/ make your wireguard conf file and open it in wordpad

in general settings paste the private key and ipv4 and ipv6 from the conf file

in advanced settings uncheck use dns server advertized by peer

put dns server from conf

in firewall settings create a firewall zone name vpn

in peers tab add peers

paste public key, allowed ips from file

check off route allowed ip

paste endpoint host and port from config

save

edit your DHCP client wwan

in advanced settings uncheck use dns server advertized by peer

put dns server from conf

now configure vpn zone

go to network > firewall

we will want the vpn zone look like wan zone

because we want to chane the ip that leaves this vpn

wan and vpn input reject*

edit 1st zone from lan to wan

allow forward to destination zone and add vpn zone

save

save and apply. if useed proper vpn and the tx and rx shows changes your vpn is ready and working

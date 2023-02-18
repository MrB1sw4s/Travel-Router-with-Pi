Do you have compiled your own custom openwrt firmware.. it means you have done almost everything... 

let's bake our pi

use raspberry pi imager and bake your custom firmware on a sd card and it will just take a couple of seconds.

now set the sd card in the pi and connect it to any lan. no need to dhcp setup cause all is ready and dhcp will be spitting ip. 

just connect it to powersupply and fire it up. you will see a new wifi. connect to it 

connect it with your custom ip over ssh as root and set up your passwd

>$ passwd

now we will be needing our pi to use it's inbuilt wifi adapter to broadcast as a router and also install pacakges to run wireless adapter. to do this we need to configure our wireless. HAH! you already did it in firmware customization part. all we need to do is to set up passwd and ssid of our wireless interfaces and choose which one to use in which case.

that's the beauty of your own custom firmware. 

>$ nano /etc/config/wireless

okk now your radio0 is yor pi's wifi module. we will use it to connect to our phone. in interface. change the ssid as you desire. set option encryption to psk2 and add option key '<your passwd>' radio1 is your wireless adapter.

change radio1 option disabled to '0'

change wifi interface ssid as you like

change encryption 'psk2'

add line > option key '<passwd>'

save and exit 

so now to connect 

>$ uci commit wireless
>$ wifi

now go to your phone and check for openwrt (by default or anything else you changed your SSID to) in wifi. 
it should be there

now go to your browser, put in your custom openwrt ip, and you will be in the router config. there will bw 2 radio interfaces in wireless under network.

now follow the steps -

network > wireless > radio1 scan > join network using passwd and tick the replace wireless config box

now ping google from terminal. if sucess then all is set for good

>$ ping google.com


your portable travel wifi router is ready. if not you want to use vpn, then go over next phase


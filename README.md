# GFWList for RouterOS DNS with EVERYTHING included Update daily 23:55:00

Import RouterOS scheduled tasks  
Starts 00:00 every day
```
/system scheduler
add name=AutoUpdateGFWList start-date=jan/01/2022 start-time=00:00:00 interval=1d on-event="/tool fetch url=\"https://raw.githubusercontent.com/AutoUpdateDaily/gfwlist/main/gfwlist.rsc\" mode=https\r\nimport gfwlist.rsc"
```

# some customization options

If you don't want a domain name to go through your VPN gateway proxy, you can delete it, such as ddns.net.
```
/ip/dns/static/remove [find name=ddns.net]
```

If you want a domain name to go through your gateway proxy, you can add it, such as sm.ms.
```
/ip/dns/static/add type=FWD match-subdomain=yes address-list=VPN forward-to=1.1.1.1 name=sm.ms
```

Clear RouterOS IPV4 IPV6 firewall gfwlist dynamic address-list
```
/ip/firewall/address-list/remove [find dynamic]
/ipv6/firewall/address-list/remove [find dynamic]
```

Clear RouterOS DNS resolution cache
```
/ip/dns/cache/flush
```

# RouterOS Recommended configuration

IPV4 IPV6 Mark routing
```
/ip firewall mangle
add action=mark-routing chain=prerouting dst-address-list=VPN new-routing-mark=VPN passthrough=yes


/ipv6 firewall mangle
add action=mark-routing chain=prerouting dst-address-list=VPN new-routing-mark=VPN passthrough=yes
```

Change TTL time 1 day
```
/ip/firewall/mangle/
add chain=prerouting action=add-dst-to-address-list connection-state=new dst-address-list=VPN address-list=VPN address-list-timeout=1d

/ipv6/firewall/mangle/
add chain=prerouting action=add-dst-to-address-list connection-state=new dst-address-list=VPN address-list=VPN address-list-timeout=1d
```

Add route (PPPOE distance=255 VPN distance=100)
```
/ip route
add comment=VPN routing-table=VPN distance=100 dst-address=0.0.0.0/0 gateway=Your_VPN_GW

/ipv6 route
add comment=VPN routing-table=VPN distance=100 dst-address=::/0 gateway=Your_VPN_GW
```


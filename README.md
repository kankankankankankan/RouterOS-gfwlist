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

If you don't want a domain name to go through your VPN gateway proxy, you can Add it, such as ddns.net.
```
/ip/dns/static/add type=FWD match-subdomain=yes address-list=gfwlist forward-to=1.1.1.1 name=sm.ms
```

Clear RouterOS DNS resolution cache
```
/ip/dns/cache/flush
```

# RouterOS Recommended configuration

IPV4 IPV6 Mark routing
```
/ip firewall mangle
add action=mark-routing chain=prerouting dst-address-list=gfwlist new-routing-mark=gfwlist passthrough=yes


/ipv6 firewall mangle
add action=mark-routing chain=prerouting dst-address-list=gfwlist new-routing-mark=gfwlist passthrough=yes
```

Change TTL time 1 day
```
/ip/firewall/mangle/
add chain=prerouting action=add-dst-to-address-list connection-state=new dst-address-list=gfwlist address-list=gfwlist address-list-timeout=1d

/ipv6/firewall/mangle/
add chain=prerouting action=add-dst-to-address-list connection-state=new dst-address-list=gfwlist address-list=gfwlist address-list-timeout=1d
```

Add route (PPPOE distance=255 gfwlist distance=10 gfwlist first)
```
/ip route
add comment=gfwlist routing-table=gfwlist distance=10 dst-address=0.0.0.0/0 gateway=Your_VPN_GW

/ipv6 route
add comment=gfwlist routing-table=gfwlist distance=10 dst-address=::/0 gateway=Your_VPN_GW
```


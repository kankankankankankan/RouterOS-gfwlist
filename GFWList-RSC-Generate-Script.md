# Get GFWList Domain name list

```
# OpenWrt Install the wget coreutils-base64 curl package
opkg update
opkg install wget coreutils-base64 curl

# Create /root/gfwlist/ folder
mkdir /root/gfwlist/

# Open the /root/gfwlist/ folder
cd /root/gfwlist/

# Download the GFWList to Domain name list script
wget https://raw.githubusercontent.com/cokebar/gfwlist2dnsmasq/master/gfwlist2dnsmasq.sh

# Add execute permissions
chmod +x gfwlist2dnsmasq.sh

# Get GFWList Domain name list to gfwlist.txt file
sh gfwlist2dnsmasq.sh -l -o /root/gfwlist/gfwlist.txt
```

# GFWList for RouterOS DNS with EVERYTHING included

```
Open the /root/gfwlist/ folder
```
cd /root/gfwlist/
```

Add GFWList for RouterOS DNS with EVERYTHING included --> gfwlist.rsc
```
echo "# GFWList for RouterOS DNS with EVERYTHING included" > gfwlist.rsc
```

Add Current date time -->> gfwlist.rsc
```
echo "# Last Modified: $(date "+%Y-%m-%d %H:%M:%S")" >> gfwlist.rsc
```

Add # -->> gfwlist.rsc
```
echo "#">> gfwlist.rsc
```

Add /ip/dns/static/remove [find type=FWD] -->> gfwlist.rsc
```
echo "/ip/dns/static/remove [find type=FWD]" >> gfwlist.rsc
```

# Add /ip dns static -->> gfwlist.rsc
```
echo "/ip dns static" >> gfwlist.rsc
```

Add add type=FWD match-subdomain=yes address-list=gfwlist forward-to=1.1.1.1 name= -->> gfwlist.txt -->> gfwlist.rsc
```
sed "s/^/add type=FWD match-subdomain=yes address-list=gfwlist forward-to=1.1.1.1 name=&/g" gfwlist.txt >> gfwlist.rsc
```


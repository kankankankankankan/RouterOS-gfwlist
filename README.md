# GFWList for RouterOS DNS with EVERYTHING included Update daily 23:55:00 CST

Import RouterOS scheduled tasks  
Starts 00:00:00 CST every day
```
/system scheduler
add name=AutoUpdateGFWList start-date=jan/01/2022 start-time=00:00:00 interval=1d on-event="/tool fetch url=\"https://raw.githubusercontent.com/vmkfstools/gfwlist/main/gfwlist.rsc\" mode=https\r\nimport gfwlist.rsc"
```

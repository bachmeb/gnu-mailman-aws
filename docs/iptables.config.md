# iptables config

This guide explains the process of blocking traffic on an EC2 instance from entire countries and known malicious hosts. 

### References
* http://www.ipdeny.com/ipblocks/data/countries/
* http://www.cyberciti.biz/faq/block-entier-country-using-iptables/
* http://www.parkansky.com/china.htm
* http://www.wizcrafts.net/iptables-blocklists.html
* https://mattwilcox.net/web-development/unexpected-ddos-blocking-china-with-ipset-and-iptables
* https://wiki.centos.org/HowTos/Network/IPTables
* 


##### Inspect the current load rules for iptables
	sudo iptables -vnL
```c
// Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
// pkts bytes target     prot opt in     out     source               destination
// 
// Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
//  pkts bytes target     prot opt in     out     source               destination
// 
// Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
//  pkts bytes target     prot opt in     out     source               destination
```

##### Stop iptables
	sudo /etc/init.d/iptables stop
```c
// iptables: Flushing firewall rules:                         [  OK  ]
// iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
// iptables: Unloading modules:                               [  OK  ]
```
	sudo iptables -vnL
	sudo /etc/init.d/iptables start

##### Add a rule to drop traffic from the malcious IP address
	sudo /sbin/iptables -I INPUT -s [ --- malicious ip ---] -j DROP

	sudo iptables -vnL
	sudo /etc/init.d/iptables save
	
	sudo cat /etc/sysconfig/iptables
	sudo /etc/init.d/iptables stop
	sudo iptables -vnL
	sudo /etc/init.d/iptables start
	sudo iptables -vnL

##### Refer to publicly available block lists for additional IP addresses. 
	http://www.wizcrafts.net/iptables-blocklists.html

<!---
ad ae af ag ai al am ao ap ar as at au aw az ba bb bd be bf bg bh bi bj bl bm bn bo bq br bs bt bw by bz ca cd cf cg ch ci ck cl cm cn co cr cu cv cw cy cz de dj dk dm do dz ec ee eg er es et eu fi fj fm fo fr ga gb gd ge gf gg gh gi gl gm gn gp gq gr gt gu gw gy hk hn hr ht hu id ie il im in io iq ir is it je jm jo jp ke kg kh ki km kn kp kr kw ky kz la lb lc li lk lr ls lt lu lv ly ma mc md me mf mg mh mk ml mm mn mo mp mq mr ms mt mu mv mw mx my mz na nc ne nf ng ni nl no np nr nu nz om pa pe pf pg ph pk pl pm pr ps pt pw py qa re ro rs ru rw sa sb sc sd se sg si sk sl sm sn so sr ss st sv sx sy sz tc td tg th tj tk tl tm tn to tr tt tv tw tz ua ug us uy uz va vc ve vg vi vn vu wf ws ye yt za zm zw
-->

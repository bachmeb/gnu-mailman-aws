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


##### See if iptables is running
	sudo service iptables status
	
##### List all the rules in all the chains (-L). Avoid long reverse DNS lookups (-n). Verbose output (-v).
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

##### See if iptables is set to start in runlevel 3
	sudo chkconfig


##### Stop iptables
	sudo service iptables stop
```c
// iptables: Flushing firewall rules:                         [  OK  ]
// iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
// iptables: Unloading modules:                               [  OK  ]
```


```c
// iptables: Firewall is not running.
```
	sudo /etc/init.d/iptables start

##### Add a rule to drop traffic from a malcious IP address
	sudo /sbin/iptables -I INPUT -j DROP -s [ --- malicious ip ---]

	sudo iptables -vnL
	sudo /etc/init.d/iptables save
	
	sudo cat /etc/sysconfig/iptables
	sudo /etc/init.d/iptables stop
	sudo iptables -vnL
	sudo /etc/init.d/iptables start
	sudo iptables -vnL

	
##### List all the rules in all the chains (-L). Avoid long reverse DNS lookups (-n). Verbose output (-v).
	sudo iptables -vnL
```c
/*
Chain INPUT (policy ACCEPT 69 packets, 3087 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 DROP       all  --  *      *       [ --- malicious ip ---]       0.0.0.0/0

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 85 packets, 4923 bytes)
 pkts bytes target     prot opt in     out     source               destination
*/
```

##### Refer to publicly available block lists for additional IP addresses. 
	http://www.wizcrafts.net/iptables-blocklists.html

##### Make a bash script to read through the wizcrafts file and add DROP rules for every IP address
	cd ~
	mkdir firewall
	cd firewall
	vim apply.rules.sh
```bash
#!/bin/bash
# Reference: http://140.120.7.21/LinuxRef/Network/BlockIP.html

# Ask for the name of the list
read -p "Enter the file name: " file_name


if [ -f $file_name ]; then
   # Flush the rules in memory
   iptables -F
   # Get the list
   BLOCKDB=./$file_name

   # Grep the contents of the file, ignore comment lines
   IPS=$(grep -Ev "^#" $BLOCKDB)

   # Loop through every ip address
   for i in $IPS
   do
      # Add drop rules for input and output to and from every ip
      iptables -A INPUT -s $i -j DROP
      iptables -A OUTPUT -d $i -j DROP
   done
fi
```

<!---
ad ae af ag ai al am ao ap ar as at au aw az ba bb bd be bf bg bh bi bj bl bm bn bo bq br bs bt bw by bz ca cd cf cg ch ci ck cl cm cn co cr cu cv cw cy cz de dj dk dm do dz ec ee eg er es et eu fi fj fm fo fr ga gb gd ge gf gg gh gi gl gm gn gp gq gr gt gu gw gy hk hn hr ht hu id ie il im in io iq ir is it je jm jo jp ke kg kh ki km kn kp kr kw ky kz la lb lc li lk lr ls lt lu lv ly ma mc md me mf mg mh mk ml mm mn mo mp mq mr ms mt mu mv mw mx my mz na nc ne nf ng ni nl no np nr nu nz om pa pe pf pg ph pk pl pm pr ps pt pw py qa re ro rs ru rw sa sb sc sd se sg si sk sl sm sn so sr ss st sv sx sy sz tc td tg th tj tk tl tm tn to tr tt tv tw tz ua ug us uy uz va vc ve vg vi vn vu wf ws ye yt za zm zw
-->

# spamassassin


## References
* 

```
sudo yum install spamassassin
nano /etc/mail/spamassassin/local.cf
groupadd spamd
useradd -g spamd -s /bin/false -d /var/log/spamassassin spamd
ls -la /var/log/spamassassin/
ls -la /var/log/
chown spamd:spamd /var/log/spamassassin
ls -la /var/log/
nano /etc/postfix/main.cf
nano /etc/postfix/master.cf
sa-update && /etc/init.d/spamassassin reload
/etc/init.d/postfix reload
/etc/init.d/spamassassin reload
/etc/init.d/spamassassin restart
sa-update && /etc/init.d/spamassassin restart
/etc/init.d/postfix reload
/etc/init.d/spamassassin restart
cat /var/log/maillog
```

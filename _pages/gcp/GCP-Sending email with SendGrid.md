# GCP - Sending email with SendGrid
ref: https://cloud.google.com/compute/docs/tutorials/sending-mail/using-sendgrid


https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-on-ubuntu-16-04


If you need to ever return to re-adjust these settings, you can do so by typing:

sudo dpkg-reconfigure postfix



- Generate the SASL password map using the API key you generated in the Before you begin section:

echo [smtp.sendgrid.net]:2525 apikey:[YOUR_API_KEY] >> /etc/postfix/sasl_passwd

- Use the postmap utility to generate a .db file:

postmap /etc/postfix/sasl_passwd

- Restart postfix
```
# /etc/init.d/postfix restart
[ ok ] Restarting postfix (via systemctl): postfix.service.
```

- install mailutils or mailx
```
apt-get install mailutils -y
```

echo "Hello" | mail -s Test106 -r 'gini@colorvibes.com'  net.gini@gmail.com

echo "Hello" | mail -s Test107 --append="FROM:gini@colorvibes.com" -r 'gini@colorvibes.com'  net.gini@gmail.com
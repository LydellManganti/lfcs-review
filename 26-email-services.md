## Setup Email Services
---

### Setup Postfix Config
- Create `user1` and `user2` to test email
- Create postfix config /etc/postfix/main.cf from main.cf.proto
- Set `example.com.ar` as the domain
- Set `myorigin`, `mydestination`, `mynetworks`, `mydomain`, `relay_domains`, `inet_interfaces`, `mailbox_size_limit`, `message_size_limit`
- Restrict access to the smtp server
- Setup tls cert and key for smtp
- Comment out other entries without a value to fix error during postfix reload.
- Update /etc/aliases to `.db`
- Create `/etc/postfix/transport`
- Update `transport` to `.db`
<details><summary>show</summary>
<p>

```bash
# Setup email users
sudo useradd user1
sudo useradd user2
sudo mkdir /home/user1
sudo mkdir /home/user2
sudo chown user1:user1 /home/user1
sudo chown user2:user2 /home/user2
# password to use when setting up email client
sudo passwd user1
sudo passwd user2
# Create a Config file
sudo cp /etc/posfix/main.cf.proto /etc/postfix/main.cf
# create domain file
sudo vi /etc/mailname
# add the line
example.com.ar
# open main.cf config and add the lines
myhostname = $(uname -n)
myorigin = /etc/mailname
mynetworks = subnet
mydomain = example.com.ar
mydestination = $myhostname, localhost.$mydomain, localhost, hash:/etc/postfix/transport
relay_domains = $mydestination
inet_interfaces = all
mailbox_size_limit = 51200000
message_size_limit = 5120000
smtpd_use_tls = yes
smtpd_tls_cert_file = /etc/dovecot/private/dovecot.pem
smtpd_tls_key_file = /etc/dovecot/private/dovecot.key
smtpd_helo_required = yes
smtpd_helo_restrictions = permit_mynetworks, reject_invalid_helo_hostname
smtpd_sender_restrictions = permit_mynetworks, reject_unknown_sender_domain
smtpd_recipient_restrictions = permit_mynetworks, reject_unauth_destination
#
sudo postalias /etc/aliases
#
sudo vi /etc/postfix/transport
# add the lines
example.com.ar local:
.example.com.ar local:
#
sudo postmap /etc/postfix/transport
```
</p>
</details>

---

### Setup Dovecot Config
- Set `mail_location`, `mail_privileged_group` in mail.conf
- Set `ssl_cert`, `ssl_key` in ssl.conf
- Set permission to `/var/mail` folder
- Restart postfix
- Restart dovecot
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/dovecot/conf.d/10-mail.conf
# Add the lines if not yet existing
mail_location = mbox:~/mail:INBOX=/var/mail/%u
mail_privileged_group = mail
# Set protocols in dovecot.conf if not yet existing
sudo vi /etc/dovecot/dovecot.conf
# add the line
protocol = imap pop3
sudo vi /etc/dovecot/conf.d/10-ssl.conf
# add the following lines
ssl_cert = </etc/dovecot/private/dovecot.pem
ssl_key = </etc/dovecot/private/dovecot.key
# set /var/mail permission
sudo chmod a+rwxt /var/mail/
# restart sevices
sudo postfix reload
sudo systemctl restart dovecot
```
</p>
</details>

---

### Setup Ubuntu Desktop (Email Client)
- Update /etc/hosts to point domain to the email server
- Enable gui on an ec2 ubuntu instance
- Setup vncserver
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/hosts
# add the line
172.31.5.5 example.com.ar mailserver
#
sudo apt-get update
sudo apt-get install -y ubuntu-desktop gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal
# add the following lines to ~/.vnc/xstartup
#!/bin/sh
export XKL_XMODMAP_DISABLE=1
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
gnome-terminal &
#
sudo apt-get install -y vnc4server
vncserver
```
</p>
</details>

---

### Setup Thunderbird Client
- Open thunderbird
- Set user, mail, 
- Test sending email
<details><summary>show</summary>
<p>

```bash
sudo apt-get install -y mailutis
echo "Test email" | sudo mail -s "Testing email" user1@example.com.au
```
</p>
</details>

---


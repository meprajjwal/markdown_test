# SMTP Enmuration

### Setting up lab environment

With root privilege add a entry as shown in /etc/hosts file.

```text
127.0.1.2    mail.prajjwal.lab
```

![Adding domain name](.gitbook/assets/image%20%2816%29.png)

Installing Postfix

```text
$ sudo apt-get install postfix
```

![installing postfix](.gitbook/assets/image%20%2810%29.png)

After this a postfix configuration dialog box is shown prompting to choose the type of mail server needed.

![postfix config](.gitbook/assets/image%20%283%29.png)

![Internal Site](.gitbook/assets/image%20%282%29.png)

![domain name](.gitbook/assets/image.png)

Open **/etc/postfix/main.cf** file and make the following changes

```text
mynetworks = 127.0.0.0/8 192.168.43.1/24
inet_protocols = ipv4
home_mailbox = Maildir/
```

![edit /etc/postfix/main.cf](.gitbook/assets/image%20%2818%29.png)

Restart postfix and view the open ports, note that port 25 is opn and its STATE is LISTEN.

```text
$ sudo service postfix restart
$ netstat -tnl
```

![restart postfix](.gitbook/assets/image%20%2815%29.png)

![open ports](.gitbook/assets/image%20%284%29.png)

Install  & Configure Dovecot

```text
$ sudo apt-get install dovecot-imapd dovecot-pop3d
```

```text
$ sudo gedit /etc/dovecot/conf.d/10-auth.conf

Uncomment the following lines
#disable_plaintext_auth = yes

Change auth_mechanish from plain to plain login
auth_mechanism = plain login

$ sudo gedit /etc/dovecot/conf.d/10-mail.conf

change the mail location as
mail_location = maildir:/home/%u/Maildir

$ sudo gedit /etc/dovecot/conf.d/10-master.conf

Uncomment inet_listner imap port
port = 143

Uncomment inet_listner pop3 port
port = 110

Give permission to unix_listner auth-userdb
unix_listner auth-userdb {
    mode = 0600
    user = postfix
    group = postfix
}
```

changes in 10-auth.conf

![disable\_plaintext\_auth](.gitbook/assets/image%20%2811%29.png)

![auth mechanism ](.gitbook/assets/image%20%286%29.png)

changes in 10-mail.conf

![changing the mail\_location](.gitbook/assets/image%20%2814%29.png)

changes in 10-master.conf

![uncomment inet\_listener imap port](.gitbook/assets/image%20%281%29.png)

![uncomment inet\_listner pop3 port](.gitbook/assets/image%20%2819%29.png)

![give permission for unix\_listner auth-userdb](.gitbook/assets/image%20%289%29.png)

restart dovecot and check for open ports

```text
$ sudo service dovecot restart
$ netstat -tnl
```

![restart dovecot and check open ports](.gitbook/assets/image%20%2821%29.png)

Setting Mail accounts in Thunderbird manual settings

![manual setting and re-test](.gitbook/assets/image%20%288%29.png)

![](.gitbook/assets/image%20%2820%29.png)

![Wrong configuration.. to check what went wrong..](.gitbook/assets/image%20%2812%29.png)


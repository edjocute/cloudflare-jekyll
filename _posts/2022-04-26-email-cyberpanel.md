---
title: "Setting up email on CyberPanel"
date: 2022-04-30T15:00:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Web
toc: false
usemathjax: false
---

## 1. Create mail domain 

- This can be automatically done while adding a new website.

- Alternatively, add an email address at the website, and prompt will show.

Here, we'll assume that the website is `domain.com`, with mail domain `mail.domain.com`.
Note that both the website and the mail domain be on different servers (i.e. IP address).

## 2. Change DNS records

Modify existing, or add a new DNS record:

- If mail server is linked to an existing target URL, a CNAME record can be used:
```white
CNAME  from  mail.domain.com  to  domain.com
```

- Else, add an A record (for IPv4) and/or AAAA record for (IPv6):
```white
A  from  mail.domain.com  to  x.x.x.x
```

## 3. Issue MailServer SSL

In Cyberpanel, `SSL` &rarr; `'MailServer SSL` &rarr; select `mail.domain.com` &rarr; `Issue SSL`.

If successful, a notice will pop-up with `Let's Encrypt successful`.

<div class=notice markdown="1">
**Issues with mail SSL** 
e.g. If email client complains about self-signed certs, try the following bash commands [(ref)](https://community.cyberpanel.net/t/mailserver-ssl-not-working-self-signed-certificate-issued/20155):

```
postmap -F hash:/etc/postfix/vmail_ssl.map
systemctl restart postfix
systemctl restart dovecot
```

</div>

## 4. Add email addresses

In Cyberpanel, `Email` &rarr; `Create Email`.


## 5. Access Rainloop webmail and test

- E.g. `https://domain.com:8090/rainloop/`

- Use reverse proxy to access website from a subdomain e.g. `webmail.domain.com`.

Try sending emails to an address from Rainloop and it should work, provided IP address is not blacklisted. 

## 6. Other records

1. DKIM
	- Get the DKIM key from `Email` &rarr; `DKIM Manager` &rarr; Select  Website: `domain.com`.
	- Make sure to select the domain (username@**domain.com**), and not the mail server (mail.domain.com).
	- Copy the **Public Key**, which starts with `"v=DKIM1; ...`
	- In your DNS page, add a `TXT` record with
		- name: `default._domainkey.domain.com`
		- content: the public key copied from the DKIM Manager.
		
2. SPF
	- Add a `TXT` record with
		- name: `domain.com` or `@`
		- content: `v=spf1 a mx ip4:x.x.x.x ~all`


3. DMARC
	- Add a `TXT` record with
		- name: `_dmarc.domain.com`
		- content: `v=DMARC1; p=none;` or `v=DMARC1; p=quarantine;`
		
		

## 7. Check settings

1. Use [mxtoolbox](www.mxtoolbox.com) to check the MX, DKIM, SPF and DMARC settings.

2. Use [mail-tester](www.mail-tester.com) to check the quality of the emails.

## 8. (Optional): route outgoing emails through a routing service (e.g. mxroute)

Instead of sending emails directly from the mail server, perhaps we want to route it through an external mail routing host (e.g. mxroute, smtp2go, Amazon SES). This can be needed if the mail server IP address is blacklisted, or if mail ports are blocked by the host.

- Modify SPF record for the originating host (not the relay) to include the relay address:
	- name: `domain.com`
	- content: `v=spf1 a mx ip4:x.x.x.x include:mxroute.com ~all`
  Check your service for the correct address to include.

- DKIM should be that of originating host, not relay.

- Edit `/etc/postfix/main.cf` and append the following at the end, using the correct information.

	```
	relayhost = <relay_host>:587
	smtp_sasl_auth_enable = yes
	smtp_sasl_password_maps = static:<relay_user>:<relay_password>
	smtp_sasl_security_options = noanonymous
	```

- Get `<relay_user>` and `<relay_user>` from the email service.
	- E.g. If email service uses DirectAdmin, add an email account (e.g. email@domain.com) and use the specified email and password.

- Don't update anything in the email client (e.g. leave the incoming and outgoing servers as `mail.domain.com`.
The mail server (postfix) will automatically route the outgoing email through the specified service.
The outgoing email will now appear to be sent from the IP address of the routing service, which is why the SPF records have to be modified.










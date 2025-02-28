---
title: "Configure SMTP"
metaTitle: "Configure SMTP"
metaDescription: "Setup SMTP for receiving alerts, weekly reports, inviting new users to OpenReplay and resetting passwords."
---

Certain functionalities such as `alerts`, `weekly reports`, `password reset` and `inviting new users` **require** email messaging. So, unless you setup SMTP, these features won't work. We highly recommend using an Email Service Provider (ESP), like Mailgun or SendGrid, for maximum reliability and deliverability. Below we provide a step-by-step guide on how to configure Mailgun, but any other provider will do the job.

## SMTP configuration

To enable SMTP, edit `openreplay/scripts/helmcharts/vars.yaml` and update the below env variables in `email` section:

| Variable | Default | Description |
|----------|-------------|-------------|
| emailHost |  | SMTP hostname (i.e. smtp.mailgun.org) |
| emailPort | 587 | SMTP port |
| emailUser |  | SMTP username|
| emailPassword |  | SMTP password |
| emailUseTls | true | For using TLS when connecting to the SMTP host |
| emailUseSsl | false | For using SSL when connecting to the SMTP host |
| emailSslKey |  | Path to your SSL key (if applicable) |
| emailSslCert |  | Path to your SSL certificate (if applicable) |
| emailFrom | do-not-reply@openreplay.com | The sender email |

Then, reinstall the web server for the changes to take effect:

```bash
cd openreplay/scripts/helmcharts && ./openreplay-cli -I
```

You can test the setup by inviting yourself (using another email) as a new team member (in 'Preferences' > 'Users').

## Mailgun

1. Go to 'Sending' > 'Domains' then click 'Add New Domain'
2. Enter your subdomain (i.e. m.mycompany.com) in 'Domain name' and ensure 'Create DKIM Authority' is checked, with preferably a 2048 key length
3. Go to your DNS provider (specific instructions are provided by Mailgun) and add **all displayed records**
4. Once all records added, click 'Verify DNS Settings'
5. Now go to 'Sending' > 'Domains settings' > 'SMTP credentials' and click 'Add new SMTP user'. Enter 'Login' (i.e. openreplay) then click 'Create SMTP credentials'. A popup should appear, hit 'Copy' to copy the generated password.
6. Use the displayed SMTP settings and credentials to configure SMTP in OpenReplay. Edit `openreplay/scripts/helmcharts/vars.yaml` and update the below env variables in `email` section:

```yaml
emailHost: 'smtp.eu.mailgun.org' # from SMTP settings section
emailPort: '587'
emailUser: 'openreplay@mycompany.com' # from SMTP credentials section
emailPassword: 'password' # the one copied when you created SMTP credentials
emailUseTls: 'true'
emailUseSsl: 'false'
emailSslKey: ''
emailSslCert: ''
emailFrom: 'openreplay@mycompany.com # sender email, use your domain'
```

7. Reinstall the web server for the changes to take effect:

```bash
cd openreplay/scripts/helmcharts && ./openreplay-cli -I
```

8. You can test the setup by inviting yourself (using another email) as a new team member (in 'Preferences' > 'Users').

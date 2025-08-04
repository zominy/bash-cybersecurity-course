# Email Errors

Hey, errors with the Email section? Yeah.

1. **Diagnose Local Email First**

```bash
echo "Test email" | mail -s "Test" your@email.com
```
If it fails:

```bash
sudo tail -n 20 /var/log/mail.log  # Check mail server errors
```
Common fixes:

```bash
sudo apt install postfix mailutils  # Install missing packages
sudo dpkg-reconfigure postfix      # Set "Internet Site" + your domain
```
2. Set Up SMTP Relay (Mailgun/SendGrid)
For Mailgun:
```bash
# Install dependencies
sudo apt install libsasl2-modules postfix

# Configure /etc/postfix/main.cf:
relayhost = [smtp.mailgun.org]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_tls_security_level = encrypt
```
Create /etc/postfix/sasl_passwd:

`
[smtp.mailgun.org]:587 postmaster@yourdomain.com:your-api-key
`

Then:

```bash
sudo chmod 600 /etc/postfix/sasl_passwd
sudo postmap /etc/postfix/sasl_passwd
sudo systemctl restart postfix
```
3. As shown in the video, API Fallback (When SMTP Fails)

Replace the mail command in your script with:

```bash
curl -s --user "api:YOUR_MAILGUN_API_KEY" \
  https://api.mailgun.net/v3/yourdomain.com/messages \
  -F from="IDS Alert <alert@yourdomain.com>" \
  -F to="$EMAIL" \
  -F subject="ALERT: SSH attack from $ip" \
  -F text="Blocked $count attempts at $(date)"
```
Critical Notes:

The key-3f6g... in videos is a placeholder (never use example keys)

For production:

```bash
# Store keys securely:
echo 'MAILGUN_API_KEY="your-real-key"' | sudo tee -a /etc/ids_secrets >/dev/null
sudo chmod 600 /etc/ids_secrets
source /etc/ids_secrets  # Load in script
```

4. Emergency Fallbacks

If all else fails:

Local Log Alerts:

```bash
echo "[CRITICAL] SSH attack from $ip" | sudo tee -a /var/log/ids_emergency.log
```
Slack/Discord Webhook:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"text":"!!! SSH attack from '"$ip"' detected"}' \
  $WEBHOOK_URL
```
**Key Reminders📢:**

- Never hardcode API keys (use environment variables or secure config files)

- Test with non-sensitive data first (e.g., youremail+test@gmail.com)

- Monitor your email service's quotas (Mailgun/SendGrid freeze accounts after limits)

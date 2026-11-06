# SightPress notifications helper

`sightpress/plugin-notifications-helper`: the official SightPress plugin that
delivers notifications from the core and every other plugin. Licensed
GPL-2.0-or-later.

> **Status:** M0 scaffolding in place (manifest, bundle, quality gates, CI);
> features arrive in milestone M6.

Senders never email, push or text anyone themselves: they call `NotifierPort`
with a notification type, ids and recipients. Where this plugin is enabled, it
fills `NotificationDeliveryInterface` and delivers:

- **Email**, always available as a fallback.
- **Browser push** (Web Push), which needs no provider account — turning it on
  generates the keys it needs, over HTTPS.
- **Text messages**, through an SMS provider adapter (Twilio, Vonage, Amazon
  SNS, Infobip, Sinch, Brevo, Bird or OVHcloud), with a per-site monthly limit.

Each person chooses what reaches them and how, per notification type, with
daily summaries, quiet hours and a "recent notifications" list in their
account. Install-wide channel setup is `php bin/console
sightpress:notifications-helper:setup`; where this plugin is disabled, only core
service email (invitations, resets, security alerts) still goes out.

## Development

```bash
composer install
vendor/bin/phpunit          # on PluginTestCase, enabled and disabled
vendor/bin/phpstan analyse
npx playwright test         # every manifest route, with axe-core
```

Fake providers and recorded responses only; CI never uses real keys. Check out
`sightpress/core` as a sibling and require it with `COMPOSER=composer.dev.json
composer install` to run the test kernel.

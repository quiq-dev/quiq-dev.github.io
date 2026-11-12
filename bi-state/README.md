# Bi-State demo: mock web pages

These are static pages that work at phone width. They load no scripts, fonts or images from other sites. John hosts them in his GitHub pages directory.

| File | Journey | What it does |
|---|---|---|
| `inbox.html` | D (BSD-D1, BSD-006) | A mock email inbox with one unread "Reset your password" message from Metro Ride On. Opening it shows the email, and its button opens `reset.html`. |
| `reset.html` | D | Choose a new password, enter it twice, see "Password updated". The password never leaves the page: there is no form post and no network call. |

## Links the agent texts

Once the pages are hosted, set the base URL in the agent's runtime config (the key is named in the spec). Then the agent can text:

```
<base>/inbox.html?r=R-1004&sms=%2B1XXXXXXXXXX
```

- `r` is the rider id. It defaults to `R-1004` (Theo R.).
- `sms` is the demo help line in E.164 form, URL-encoded (`+` becomes `%2B`). `inbox.html` passes it on to `reset.html`.

## How the agent learns the reset finished

The success screen has a **Return to your chat** button. It opens the rider's texting app, prefilled with `I reset my password (ref PW-####)`, and the rider taps Send.

`bistate-core`'s `reset_status` recomputes the same reference from the rider id and today's UTC date, so only this page can produce it. It's a check value, not a secret. The algorithm is `demoRef()` in `reset.html`, and the Python twin in the service must stay in step with it.

If `sms` is missing, the button is hidden and the page tells the rider to go back to their texts.

## Not here

- **/signin.** Journey A uses Quiq's built-in sign-in message, and the platform hosts that page. A fallback `signin.html` gets built only if the Quiq sign-in doesn't work over SMS.
- **/supervisor-feed.** This is the webhook receiver for Max's AI Analyst (BSD-008), which Max owns. A static page can't receive POSTs.

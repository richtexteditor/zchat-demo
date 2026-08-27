# Embedding the ZChat widget

Add one script tag to any page, just before `</body>`:

```html
<script src="https://YOUR-SERVER/widget/zchat.iife.js"
        data-server-url="https://YOUR-SERVER"
        data-site-id="default"></script>
```

Or initialise it yourself for full control:

```html
<script src="https://YOUR-SERVER/widget/zchat.iife.js"></script>
<script>
  ZChat.init({
    serverUrl: 'https://YOUR-SERVER',
    siteId: 'default',
    greeting: 'Hello! How can we help you today?',
    position: 'bottom-right',
    primaryColor: '#2563eb'
  });
</script>
```

## Embed attributes

| Attribute | `init()` option | Effect |
| --- | --- | --- |
| `data-server-url` | `serverUrl` | **Required.** Your ZChat server origin. |
| `data-site-id` | `siteId` | Site identifier. |
| `data-position` | `position` | `bottom-right` (default) or `bottom-left`. |
| `data-primary-color` | `primaryColor` | Accent colour of the launcher and header. |
| `data-greeting` | `greeting` | First message shown in the panel. |
| `data-offline-message` | `offlineMessage` | Shown above the leave-a-message form. |
| `data-require-email` | `requireEmail` | Make the email field mandatory. |
| `data-show-department` | `showDepartment` | Show the department selector. |
| `data-department-options` | `departmentOptions` | Comma-separated department list. |
| `data-department` | `department` | Department this page belongs to, reported with presence. |
| `data-track-visitors` | `trackVisitors` | Set `false` to keep a page out of visitor monitoring. |
| `data-visitor-name` | `visitor.name` | Pre-fill the visitor's name (see below). |
| `data-visitor-email` | `visitor.email` | Pre-fill the visitor's email. |
| `data-visitor-department` | `visitor.department` | Pre-select a department. |

Options with no attribute equivalent — `locale`, `soundEnabled`, `rememberSession`,
`triggersEnabled`, `requireConsent`, `ratingEnabled`, `chatbotEnabled`,
`preChatFields` — are set through `ZChat.init()`.

## Portals and signed-in customers

If the page already knows who the visitor is, hand that to the widget and the
pre-chat form opens pre-filled instead of asking for details you are already
displaying:

```html
<script src="https://YOUR-SERVER/widget/zchat.iife.js"
        data-server-url="https://YOUR-SERVER"
        data-visitor-name="Dana Reyes"
        data-visitor-email="dana@contoso.example"></script>
```

or:

```html
<script>
  ZChat.init({
    serverUrl: 'https://YOUR-SERVER',
    visitor: { name: 'Dana Reyes', email: 'dana@contoso.example' }
  });
</script>
```

The visitor can still edit anything before starting the chat, and a resumed
session takes precedence over these values.

> **This is a convenience, not authentication.** The values travel from the
> browser like any other pre-chat input, and the server treats them as
> untrusted. Never use them to grant access to anything. If an agent needs to
> trust an identity, verify it server-side.

## Keeping a page out of visitor monitoring

Visitor monitoring is off by default and enabled by the operator. When it is on,
an individual page can opt out — useful for checkout or account pages you would
rather keep out of the browsing trail:

```html
<script src="https://YOUR-SERVER/widget/zchat.iife.js"
        data-server-url="https://YOUR-SERVER"
        data-track-visitors="false"></script>
```

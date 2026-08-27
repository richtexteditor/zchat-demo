# ZChat — demo & install guides

ZChat is self-hosted live chat software: a visitor widget, an agent console, an
admin dashboard, offline message capture, file uploads, webhooks and an optional
AI chatbot handoff. You download it, run it on your own server, and your
conversation data never leaves your infrastructure.

**This repository contains the demo page and the install/verification guides.**
It is not the product source — ZChat is commercial software. Downloads and
pricing live at [zchat.com](https://zchat.com/).

## Downloads

| Package | Size | For |
| --- | --- | --- |
| [zchat-install.zip](https://zchat.com/download/zchat-install.zip) | ~34 MB | **ASP.NET Core 8** edition — the current product. Linux or Windows. |
| [zchat-webforms.zip](https://zchat.com/download/zchat-webforms.zip) | ~147 MB | **ASP.NET Web Forms 4.8** edition — classic IIS on .NET Framework. |
| [zchat-agent-console.zip](https://zchat.com/download/zchat-agent-console.zip) | ~62 MB | Windows desktop agent console (optional, self-contained). |

Both server editions use the same SQL Server schema.

## Try it in one command

Unzip `zchat-install.zip` and run:

```cmd
run-local.cmd
```

On first run this creates a demo database on `.\SQLEXPRESS`, installs the schema
and seed accounts, and starts the server on <http://127.0.0.1:5050>.

- Dashboard: <http://127.0.0.1:5050/dashboard>
- Visitor widget demo: <http://127.0.0.1:5050/widget-test.html>

Seed accounts are `admin` / `admin123` and `agent1` / `agent123`. **These are
published credentials — change them before the server is reachable by anyone
else.** The dashboard shows a warning banner until you do.

Requirements: the [.NET 8 ASP.NET Core Runtime](https://dotnet.microsoft.com/download/dotnet/8.0),
SQL Server 2014+ (Express is fine), and `sqlcmd` if you want the database
created for you.

## Embedding the widget

Add this to any page, just before `</body>`:

```html
<script src="https://YOUR-SERVER/widget/zchat.iife.js"
        data-server-url="https://YOUR-SERVER"
        data-site-id="default"></script>
```

Or initialise it yourself:

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

Every embed option, plus how to pre-fill the form for a signed-in customer in a
portal, is in [docs/embedding.md](docs/embedding.md).

[`demo/index.html`](demo/index.html) is a self-contained page that does this
against a server URL you type in — useful for checking the widget against your
own install before you touch your real site.

## Guides

- [Install the ASP.NET Core edition](docs/install-core.md)
- [Verify a Core install](docs/verify-core.md) — proves the server, database, licence, dashboard and widget actually work
- [Embedding the widget](docs/embedding.md) — every option, portals, and opting a page out of monitoring
- [Verify a Web Forms install](docs/verify-webforms.md) — including the two checks that catch a broken deployment

## Licence

ZChat is commercial software under a perpetual one-time licence; see
[zchat.com/pricing.aspx](https://zchat.com/pricing.aspx). The download includes a
localhost trial licence so you can evaluate the whole product before buying.

The contents of *this repository* (guides and demo page) are provided so you can
evaluate and integrate ZChat. It contains no product source code.

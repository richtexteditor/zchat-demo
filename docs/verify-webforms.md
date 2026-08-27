# Verifying a ZChat install (Web Forms edition)

For `zchat-webforms.zip` — the ASP.NET Web Forms 4.8 edition for classic IIS.
Two of these checks exist specifically because a deployment can look fine while
the agent console or the visitor widget is dead.

Examples assume `http://localhost:5050`.

## 0. Try it locally without IIS

IIS Express can serve the `Web` folder directly:

```cmd
"C:\Program Files\IIS Express\iisexpress.exe" /path:"<full path>\Web" /port:5050 /clr:v4.0
```

Two things to know: IIS Express registers `localhost` only, so `127.0.0.1`
returns HTTP 400; and browsers block a number of low ports, so prefer something
like 5050.

## 1. Run the installer

Create a blank database on your SQL Server, then open <http://localhost:5050/>.

Expected: the site reports "Requires Installation" and offers **Install ZChat**.
The installer asks for a connection string and the admin account to create,
"Test Connection" succeeds, and submitting lands on "Installation Completed!".

The installer writes the connection string into `Web.config` for you — a shipped
`Web.config` must **not** already contain a `ConnectionString` key.

Delete the `Web\install` folder afterwards.

## 2. Dashboard

Open <http://localhost:5050/dashboard> and sign in with the admin account you
just created. Expected: `login.aspx` accepts it and the Account Console loads.

## 3. Agent web console — the check that catches a gutted package

Open <http://localhost:5050/webconsole/>.

Expected: the loader reaches 100% **and then the console UI appears** — a
Departments/Agents tree, a Request Queue reading "Request Queue is Empty.", and
a Current Visitors grid.

If it sticks at "Loading ZChat Agent Console … 100%" forever, check the browser
console. Errors like `TEXT_CS_TITLE_DEPARTMENTS is not defined` mean the
localisation string table is empty. Open
<http://localhost:5050/webconsole/settings.aspx>: if `__cc_strings` contains only
`"__end"`, then `Web\livechat2\Languages` is empty or missing. That folder is
where the application resolves its string table from, and the console cannot
start without it.

## 4. Visitor widget

Open the bundled demo page <http://localhost:5050/demo/demo1.htm>.

Expected: the page loads **and** the widget renders — the offline "Leave a
message" form when no agent is signed in, or the chat prompt when one is online.

If nothing renders, look for
`livechat2/resources2.aspx?HCCID=…` returning HTTP 500. Opening that URL directly
shows the underlying error; a `FileNotFoundException` for
`livechat2\script\newlib2.js` means `Web\livechat2\script` is empty or missing.

## 5. End-to-end

With the console open and the agent online, start a chat from the demo page. The
visitor should appear in the Request Queue, accepting it should open a chat tab,
and messages should arrive in both directions.

## 6. Desktop agent console

The `Console\ZChatWinForms` folder can be copied to each agent's Windows machine
(.NET Framework 4.8 required). Include the scheme in **App Site URL**
(`http://…` or `https://…`) and the console connects accordingly; only a bare
host name falls back to the "Use secure connection for chat (https)" option.

## 7. Before production

- Delete `Web\install`.
- Serve over HTTPS.
- Keep `Web\App_Data\zchat.lic` in place.
- Confirm `Web.config`'s connection string points at your real database.

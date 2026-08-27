# Verifying a ZChat install (ASP.NET Core edition)

Work through this after extracting `zchat-install.zip`. It proves the server,
database, licence, dashboard and visitor widget are actually working — not just
that a page loaded.

Examples assume the local runner URL `http://127.0.0.1:5050`. Substitute your own.

## 1. Start it

```cmd
run-local.cmd
```

Expected: the script finds `Web\ZChat.Web.dll` and `Web\zchat.lic`, creates the
demo database on first run, applies `ZCHAT-APP-DB.sql` and `seed-data.sql`, and
the server starts.

If `sqlcmd` is missing, create the database and run the two scripts yourself.

## 2. Health

- <http://127.0.0.1:5050/health/live> → `{"status":"live"}`
- <http://127.0.0.1:5050/health/ready> → `status: ready` with `database.ok`,
  `license.ok` and `schema.ok` all true, and the number of schema steps applied.

A database error here means the connection string, the SQL Server service, the
database name, or the install scripts. `/health/ready` names which dependency failed.

## 3. Dashboard

Open <http://127.0.0.1:5050/dashboard> and sign in as `admin` / `admin123`.

Expected: the command centre loads, and a warning banner tells you the account
is still using the demo password. That banner disappears once you change it.

## 4. Visitor widget

Open <http://127.0.0.1:5050/widget-test.html>.

Expected: API Server shows healthy with a version, the chat bubble appears in the
bottom-right corner, and the Embed Code block shows a snippet pointing at the
address you are browsing. <http://127.0.0.1:5050/widget/zchat.iife.js> returns 200.

## 5. Public widget APIs

- <http://127.0.0.1:5050/api/v1/widget/status> → online/offline information
- <http://127.0.0.1:5050/api/v1/widget/triggers> → an array, possibly empty

Both should return 200 without a token. Endpoints under `/api/v1/` that manage
agents, settings or sessions correctly return 401 without one.

## 6. End-to-end conversation

1. On the widget test page, open the widget and submit the pre-chat form.
2. In the dashboard, go to **Live Chat** and set your status to **Online**.
3. Accept the waiting visitor and exchange a message in each direction.
4. End the chat from the visitor side and submit a star rating.

Expected: the session and full transcript appear under **Chat Sessions**, and the
rating shows in the dashboard CSAT summary. With no agent online, the widget
offers a "leave a message" form instead, and submissions land in **Missed Chats**.

## 7. Before production

- Replace localhost URLs with your real HTTPS domain.
- Keep `Web\zchat.lic` in the `Web` folder.
- Move the connection string into `appsettings.json` or environment variables.
- Change or remove the seed passwords.

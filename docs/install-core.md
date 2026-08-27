# Installing ZChat (ASP.NET Core edition)

## Requirements

- [.NET 8 ASP.NET Core Runtime](https://dotnet.microsoft.com/download/dotnet/8.0) or later
- SQL Server 2014 or newer (SQL Server Express is fine)
- `sqlcmd` (SQL Server command-line tools) if you want the demo database created automatically

## Evaluate locally

```cmd
run-local.cmd
```

First run creates the `zchatdemo` database on `.\SQLEXPRESS`, applies the schema
and seed accounts, and starts on <http://127.0.0.1:5050>. Re-running is safe — an
already-provisioned database is detected and left alone.

`run-local.ps1` takes options:

| Option | Effect |
| --- | --- |
| `-Port 5060` | Serve on a different port |
| `-Server ".\SQL2019"` | Target a different SQL instance |
| `-Database "zchatdemo2"` | Use a different database name |
| `-OpenBrowser` | Open the dashboard once the server answers |
| `-SkipDatabaseSetup` | Never touch the database |
| `-ConnectionString "..."` | Use a full custom connection string (disables automatic setup) |

With `-SkipDatabaseSetup` or a custom `-ConnectionString`, nothing is
provisioned for you: the database you point at must already have the schema and
accounts, or `/health/ready` will report not-ready and sign-in will fail.

## Install on your own SQL Server

1. Create a blank database (for example `ZChat`).
2. Run `install-source\ZCHAT-APP-DB.sql` to create the tables.
3. Run `install-source\seed-data.sql` to create the default accounts.
4. Keep `Web\zchat.lic` in the `Web` folder. Restart the server after adding or
   replacing it.
5. Put your connection string in `Web\appsettings.json` (or copy
   `Web\appsettings.example.json` and edit that).

Start the server:

```cmd
cd Web
dotnet ZChat.Web.dll --urls "http://localhost:5000"
```

Then open <http://localhost:5000/dashboard>.

## Before you go live

- Change or remove the seed accounts (`admin` / `admin123`, `agent1` / `agent123`).
  The dashboard shows a warning banner while a seed password is still in use.
  Agent passwords must be 12–256 characters.
- Serve over HTTPS with your real domain, and update the widget embed URLs to match.
- Set a real `Jwt:SecretKey` in `appsettings.json` if you are not using the one
  generated for your download.
- Point `Uploads` and log paths at storage you actually back up.

## Docker

The package ships a `Dockerfile`, `docker-compose.yml` and `docker-up` scripts.
Copy `.env.example` to `.env`, fill in the connection string, then:

```bash
./docker-up.sh
```

# RunTipi SplitPro App Store

This repository contains a custom RunTipi app definition for [SplitPro](https://github.com/oss-apps/split-pro), a self-hosted alternative to Splitwise for tracking and settling shared expenses.

## Included application

### SplitPro

SplitPro supports:

* Shared expense groups
* Equal and unequal expense splits
* Multiple currencies
* Expense attachments and receipts
* Balance calculation and debt settlement
* Recurring transactions
* Authentication through Authentik using OpenID Connect

The RunTipi definition deploys:

* The SplitPro application
* PostgreSQL with the `pg_cron` extension
* Persistent storage for the database
* Persistent storage for receipt uploads

## Repository structure

```text
apps/
└── splitpro/
    ├── config.json
    ├── docker-compose.json
    └── metadata/
        ├── description.md
        └── logo.jpg
```

## Installation

Add this repository as a custom app store in RunTipi.

After refreshing the app stores, locate **SplitPro** and begin the installation.

The installer requests the following values:

| Field                   | Description                                         |
| ----------------------- | --------------------------------------------------- |
| Database password       | Generated automatically by RunTipi                  |
| NextAuth secret         | Generated automatically by RunTipi                  |
| Authentik client ID     | Client ID from the Authentik OAuth2/OpenID provider |
| Authentik client secret | Client secret from the Authentik provider           |
| Authentik issuer URL    | Authentik issuer URL for the SplitPro application   |

An example Authentik issuer URL is:

```text
https://auth.example.com/application/o/splitpro
```

Do not include a trailing slash.

## Authentik configuration

Create an OAuth2/OpenID Connect provider in Authentik with:

* Client type: `Confidential`
* Scopes: `openid`, `profile`, and `email`
* Application slug: `splitpro`

Set the redirect URI to:

```text
https://split.example.com/api/auth/callback/authentik
```

Replace `split.example.com` with the domain assigned to SplitPro in RunTipi.

## Persistent data

The app stores persistent data under the RunTipi application data directory:

```text
postgres/
uploads/
```

The `postgres` directory contains the SplitPro database.

The `uploads` directory contains receipt images and other uploaded attachments.

Both directories should be included in backups.

## Updating

The application currently uses:

```text
ossapps/splitpro:latest
```

Before updating a production installation, back up the PostgreSQL and uploads directories.

For more predictable upgrades, replace `latest` with a verified SplitPro release tag and increment `tipi_version` in `config.json` whenever the app definition changes.

## Upstream projects

* SplitPro: https://github.com/oss-apps/split-pro
* RunTipi: https://github.com/runtipi/runtipi
* Authentik: https://github.com/goauthentik/authentik

## License

This repository contains only the RunTipi deployment definition.

SplitPro, RunTipi, Authentik, PostgreSQL, and their respective container images remain subject to their own licenses.

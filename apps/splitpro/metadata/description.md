# SplitPro

SplitPro is a self-hosted application for tracking, dividing, and settling shared expenses with friends, families, households, and travel groups.

It provides an experience similar to Splitwise while allowing you to retain control over your financial and account data.

## Features

* Create groups for trips, households, events, and recurring expenses
* Split expenses equally or using custom amounts, percentages, and shares
* Track balances between participants
* Record repayments and settlements
* Simplify outstanding debts
* Use multiple currencies
* Attach receipts and other files to expenses
* Manage recurring transactions
* Install the application as a progressive web app
* Authenticate through Authentik using OpenID Connect

## Included services

This RunTipi package deploys two containers:

### SplitPro

The main web application, exposed through RunTipi’s reverse proxy.

### PostgreSQL

A dedicated PostgreSQL database using the SplitPro-compatible image with the `pg_cron` extension enabled.

`pg_cron` is used for scheduled functionality such as recurring transactions.

## Authentication

This package is configured to use Authentik as its authentication provider.

Before installing SplitPro, create a confidential OAuth2/OpenID Connect provider in Authentik and configure the following redirect URI:

```text
https://your-splitpro-domain/api/auth/callback/authentik
```

During installation, enter the provider’s:

* Client ID
* Client secret
* Issuer URL

An issuer URL normally looks like:

```text
https://auth.example.com/application/o/splitpro
```

The issuer URL must not have a trailing slash.

## Storage

The following data is persisted:

* PostgreSQL database files
* Receipt images
* Uploaded attachments

Include both the database and uploads directories in your regular RunTipi backups.

## Important notes

SplitPro does not provide a conventional local username-and-password login in this configuration. A working Authentik provider is therefore required.

The public SplitPro domain configured in RunTipi must match the redirect URI configured in Authentik exactly, including the protocol and hostname.

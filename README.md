# ISP Billing Platform — showcase

**English** · [Русский](README.ru.md) · [Română](README.ro.md)

A billing system for an internet service provider in Moldova, built from scratch to replace
an ageing WHMCS installation. It handles clients, contracts, services, monthly invoicing,
payments, debt and access limiting, and the paperwork around them, in three languages — and it
took over from the old system with every client's balance checked to the last leu.

This repository is a **showcase**: screenshots and a description of the project. The source
code is private. Every name, code, address and amount in the screenshots is invented: they
were taken from a throwaway copy of the system filled with demo data.

![Client card](screenshots/en/02_client_card.png)

---

## What it does

**Clients and services**
- Individuals and legal entities, several accounts per client (each with its payment code),
  contracts with scans, contacts, addresses, network binding (point of presence, switch,
  port, IP, MAC).
- A service catalogue (internet, intranet, TV, hosting, colocation, extra IPs, equipment
  rental, one-time and yearly services) with dated price versions: a price change never
  rewrites history.
- Per-service individual prices and quantities (8 × extra IP), internet speeds, and the exact
  service name printed on the invoice.
- **Data checks instead of refusals**: the same IP, MAC, contract number or company code at two
  clients, accounts without a contract, malformed contract numbers, yearly services without a
  payment month — saved anyway and listed in red, with links, until fixed or confirmed.
- The state register of companies loaded and compared with the client cards.
- Accounts in MDL or EUR; amounts in different currencies are never added up.

**Invoicing**
- Monthly prepaid invoicing on the 1st, with proration by days for every change in the middle
  of a month: connection, pause, debt limit, tariff or price change, closing.
- **Paid ahead**: staff mark how many months a client paid in advance — those months are not
  invoiced, optionally with one invoice for the whole period; yearly services are invoiced in
  the month the client pays each year.
- **Legal entities** are invoiced through drafts: prepared on a set day, submitted by one person
  and approved by another before an invoice is issued; one invoice per contract.
- **Dry run of any month**: every invoice with its dates, lines and total, as cards — and for a
  month the old system already invoiced, compared with it client by client.
- An **invoicing schedule** page for billing managers: when invoices are made, when they are
  emailed, when the payment files go — each with a switch and a day, and what happens next.
- An append-only ledger: issued invoices are immutable, corrections go through credit notes.
  A payment closes the oldest debt first; the rest becomes an advance. A cancelled invoice
  shows as annulled; a wrong one can be deleted together with its credit notes, the payments
  on it freed again.
- **Invoice forms in a visual editor** (GrapesJS) with versions; the classic form reproduces the
  old printed invoice measure for measure. Invoices are in Romanian, messages in the client's
  language.
- **Printing**: a month's invoices two per landscape sheet, made on the server in the
  background and opened in the browser by a link.

**Payments**
- Cash desk, terminals, bank, post office; duplicate receipt numbers are refused.
- **Bank statements** (OTP Bank `.dbf`): only incoming money is read, the company's own
  payments are skipped, each credit is matched to a client (remembered payer account, company
  code, invoice / payment code / contract number in the purpose, company name) and booked after
  one click. Loading the same statement twice adds nothing.
- **Files for payment collectors**: the list of who owes how much, in the Excel layout the
  partners already read — to Paynet by email and to the post office by FTP; made monthly,
  previewed before sending, with connection checks that send nothing and a sending log.
- **Client lookup API** by payment code for the partner terminal system, answering exactly like
  the old system, allowed only from set addresses with a key.

**Documents**
- **Contract inbox**: a batch of scanned contracts is read on the server (text layer or OCR
  in Romanian and Russian), the system suggests whose contract each one is and why, and a
  person confirms. Files get a configurable standard name.
- **Old invoices of legal entities**: buyer, company code, contract and every service line are
  read from the PDF by the position of the words; the client is suggested and checked against
  its card; lines take existing services only by their exact name, new ones become services;
  the same file is never taken twice. The scan itself is not kept.

**Everyday work**
- A **home page** like the old system's admin summary: income today / this month / this year,
  tiles that link to their lists (active clients and services, limited for debt, waiting to be
  connected, overdue invoices), what waits for whom, payment charts, the monthly income to
  expect, the oldest unpaid invoices and the latest payments.
- Lists where **each person picks their columns** (company code, VAT code, bank, balance… on
  top of the usual ones), values that **copy on a click**, and a click anywhere on a row opens
  the record.
- The client card with tabs: profile, contacts, accounts, contracts with scans, and all
  invoices with paid date, payment method and a coloured status; "create an invoice" by hand.

**Messages, reports, audit**
- Email templates in two languages with variables, a mailing schedule with per-client days,
  invoice PDFs attached.
- Reports: income (lei and euro on separate axes), unpaid amounts by age, service statuses,
  new and lost clients.
- An activity journal of every change, view, export and login, with who did it and from where.

**Migration**
- A universal import through a canonical format, with a dedicated WHMCS adapter, a dry run,
  idempotent re-runs that never overwrite what staff changed since, and a review queue for
  problem records (corrected and imported, or joined to a card added by hand).
- Old duplicate products were merged into one catalogue with every client's price and invoice
  text preserved, proven by comparing all invoices before and after.
- At the cutover the balance of every client was compared with the old system independently
  and evened out with documents of their own; months paid ahead in the old system carried over.

---

## Screenshots

| | |
|---|---|
| ![Clients](screenshots/en/01_clients.png) Clients | ![Legal entity](screenshots/en/03_legal_entity_card.png) Legal entity card |
| ![Service](screenshots/en/04_service.png) A service with speeds and its history | ![Products](screenshots/en/05_products.png) Service catalogue |
| ![Invoices](screenshots/en/06_invoices.png) Invoices | ![Invoice](screenshots/en/07_invoice.png) An invoice |
| ![Invoice PDF](screenshots/pdf/invoice_pdf_ro.png) Invoice PDF, the classic form | ![Invoice PDF](screenshots/pdf/invoice_pdf_ru.png) Invoice PDF of a Russian-speaking client (invoices are in Romanian) |
| ![Monthly billing](screenshots/en/08_monthly_billing.png) A monthly billing run | ![Payments](screenshots/en/09_payments.png) Payments |
| ![Bank statement](screenshots/en/10_bank_statement.png) Bank statement matching | ![Contract inbox](screenshots/en/11_contract_inbox.png) Contract inbox |
| ![Income](screenshots/en/13_report_income.png) Income report | ![Unpaid](screenshots/en/14_report_unpaid.png) Unpaid by age |
| ![Statuses](screenshots/en/15_report_statuses.png) Service statuses | ![Clients report](screenshots/en/16_report_clients.png) New and lost clients |
| ![Template](screenshots/en/17_message_template.png) Message template | ![Schedule](screenshots/en/18_mailing_schedule.png) Mailing schedule |
| ![Company](screenshots/en/19_company_details.png) Company details and settings | ![Journal](screenshots/en/20_activity_journal.png) Activity journal |
| ![Home](screenshots/en/22_home.png) Home page: income, tiles, what waits, charts | ![Data checks](screenshots/en/23_data_checks.png) Data checks |
| ![Schedule](screenshots/en/24_invoicing_schedule.png) Invoicing schedule | ![Dry run](screenshots/en/25_dry_run.png) Dry run of a month, as cards |
| ![Payment files](screenshots/en/26_payment_reports.png) Files for Paynet and the post office | ![Print](screenshots/en/27_print_invoices.png) Print file of a month |
| ![Paid ahead](screenshots/en/28_paid_ahead.png) Paid ahead | ![Columns](screenshots/en/29_columns.png) Columns chosen by each person |
| ![Drafts](screenshots/en/31_invoice_drafts.png) Invoice drafts of legal entities | ![Old invoices](screenshots/en/12_invoice_inbox.png) Old invoices of legal entities |
| ![Account](screenshots/en/21_account.png) An account | |

The interface in Russian and Romanian:

| | |
|---|---|
| ![RU](screenshots/ru/02_client_card.png) Client card (RU) | ![RO](screenshots/ro/02_client_card.png) Client card (RO) |
| ![RU](screenshots/ru/22_home.png) Home page (RU) | ![RO](screenshots/ro/22_home.png) Home page (RO) |
| ![RU](screenshots/ru/24_invoicing_schedule.png) Invoicing schedule (RU) | ![RO](screenshots/ro/26_payment_reports.png) Files for payment (RO) |
| ![RU](screenshots/ru/13_report_income.png) Income (RU) | ![RO](screenshots/ro/10_bank_statement.png) Bank statement (RO) |

---

## Architecture

```mermaid
flowchart LR
  staff([Staff browser]) -->|HTTPS| proxy[Caddy reverse proxy]
  proxy --> app[Django app<br/>admin UI, reports, PDF]
  app --> db[(PostgreSQL)]
  app --> files[(Encrypted file storage)]
  scheduler[Scheduler<br/>nightly and monthly jobs] --> db
  app -->|domain events<br/>outbox| adapters[Adapters]
  adapters -.-> odoo[Odoo]
  adapters -.-> efactura[e-Factura]
  adapters -.-> radius[RADIUS / BRAS]
  bank[Bank statement .dbf] --> app
  scans[Contract and invoice scans] -->|read on the server| app
  app -->|debtors' lists| partners[Paynet email · post office FTP]
  terminals[Partner terminal system] -->|client lookup API| app
```

- **Core**: clients, contracts, catalogue, subscriptions with a change timeline, charging and
  proration, invoicing and the ledger, payments. The core knows nothing about external
  systems: it records domain events in an outbox inside the same transaction, and adapters
  deliver them.
- **Money**: decimal amounts, VAT included and extracted per line, rounding half-up, an
  append-only ledger, balances always computed from entries.
- **Policies** (due dates, invoicing days, numbering, limiting delays, upload limits, file
  name patterns, batch sizes) are settings, not code.
- **Deploys without downtime**: the new image is built and migrated while the old one serves,
  the proxy holds requests during the switch, migrations only add while people work.

## Technology

Python 3.13 · Django 5.2 · PostgreSQL 17 · Docker Compose · Caddy · django-unfold (admin UI) ·
WeasyPrint and pypdf (PDF) · GrapesJS (form editor) · Tesseract OCR and Poppler · Chart.js ·
pytest (730+ tests, run in parallel) · ruff · gitleaks in a pre-commit hook.

## Security and personal data

Designed with the Moldovan personal data law (Law 195/2024) in mind:

- Uploaded and generated files (contract scans, invoice PDFs, print files, payment lists) are
  **encrypted at rest** and served only through a view that checks the permission of the
  record they belong to; every opening is recorded.
- Names of individuals can be masked on printed invoices.
- Uploads are checked by content, not by name; size limits are a setting.
- Documents are read **on the server**; nothing is sent to external services.
- A permission for every page and button, built-in roles (billing manager, chief accountant,
  network admin) kept in sync without undoing what was changed by hand; each person sees only
  what they may open, also on the home page.
- Login lockout after repeated wrong passwords, short sessions, an audit trail of logins.
- Passwords of the mail server, FTP and API keys stored encrypted and never shown again.
- Firewall limited to the internal network, the app reachable only through the reverse proxy,
  secrets only in environment files, a pre-commit hook that blocks secrets, dumps, personal
  codes and internal addresses.
- OS updates visible in the admin, with an "update" button that only leaves a request for a
  fixed server-side job: the web app can never run commands on the server.

## Planned

- e-Factura (the state electronic tax invoice system) for legal entities, through its API.
- Staff single sign-on with two-factor authentication (Authelia) behind HTTPS.
- Odoo integration: clients, services and debt visible next to support tasks.
- RADIUS / BRAS: automatic access limiting for debt and restoring after payment.
- A client self-service cabinet.

---

The screenshots and text in this repository are © the author. The software itself is not
open source.

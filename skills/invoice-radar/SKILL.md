---
name: invoice-radar
description: Collect invoices from connected services and search, inspect, export, or sync documents with the Invoice Radar desktop CLI. Use for Invoice Radar document workflows; not for the internal admin CLI or generic website scraping.
---

# Invoice Radar

Use the `invoice-radar` CLI to collect and work with the user's invoices. Collection runs through integrations in the desktop app; the CLI does not scrape an arbitrary URL.

## Before using the CLI

- The CLI currently runs on macOS. Invoice Radar must be installed, signed in, and have **Settings → General → CLI Access** enabled.
- Check `invoice-radar status`. If the app is closed, use `invoice-radar open --wait` and check again. If the command is missing, see [CLI setup and commands](references/cli.md#setup).
- Commands use the organization selected in the app. For another organization, find its ID with `invoice-radar org list` and pass `--org <org-id>`.
- Prefer `--json` for results an agent will parse. Check command exit status and the returned `success` or `result` field where present; a command completing does not mean invoices were found.

## Collect documents

1. List connected integrations with `invoice-radar integrations list`. If the requested service is not connected, search `invoice-radar integrations available <service>` and add its plugin with `invoice-radar integrations add <plugin-id>`.
2. Run the connected instance with `invoice-radar run <integration-id>`. A plugin ID also works when exactly one instance uses it. The default run handles authentication and document collection; the user may need to complete login or MFA in the desktop app.
3. Read the run summary, including new, existing, out-of-range, and failed counts. If collection failed, inspect the error before retrying. Use `--mode documents` only when the user specifically wants the document step without the full flow.
4. Search for the requested invoice with `invoice-radar --json docs search "<vendor or invoice number>" --limit 25`. Inspect matches and use the `id` of the correct document, not its displayed invoice number. `docs show <document-id>` provides details; `--text` shows extracted PDF text when available.

## Export or sync

- Export one PDF with `invoice-radar docs export <document-id> <output.pdf>`. Choose the user's requested path, or a clear local path and report it. The command refuses to overwrite an existing file unless `--force` is supplied; use `--dry-run` to check whether an export is allowed. Do not treat a metadata record as proof that a PDF exists: check `hasPdf` when available.
- Sync through an export destination already configured in Invoice Radar with `invoice-radar docs sync --destination <destination-id>`. Add `--invoice <document-id>` to target one invoice. Report the actual sync result.
- Trial organizations cannot use CLI export or sync when a watermark would be required. Report that limit instead of trying to bypass it.

For setup, examples, authentication, search options, and troubleshooting, read [CLI setup and commands](references/cli.md). For the full, current command reference, use `invoice-radar <command> --help` or the [official CLI docs](https://invoiceradar.com/docs/cli).

# CLI setup and commands

## Setup

The customer CLI is currently macOS only. [Install Invoice Radar](https://invoiceradar.com/download), open the desktop app, sign in, and enable **Settings → General → CLI Access**. Then run:

```bash
invoice-radar status
```

If the shell cannot find the command, add `$HOME/.local/bin` to `PATH` and reopen the terminal:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

If the app is closed:

```bash
invoice-radar open --wait
```

To select an organization other than the one open in the app:

```bash
invoice-radar org list
invoice-radar --org <org-id> integrations list
```

Place `--json` before a subcommand for machine-readable output, for example `invoice-radar --json docs search "Acme"`.

## Collect from a service

Use an existing connected instance when possible:

```bash
invoice-radar integrations list
invoice-radar --json run <integration-id>
```

To connect a new supported service, use its plugin ID from `available`. Adding creates an instance; it does not log the user in or collect documents by itself:

```bash
invoice-radar integrations available <service>
invoice-radar integrations add <plugin-id>
invoice-radar run <integration-id>
```

The default `run` performs the full integration flow, including authentication and document collection. Login or MFA can require interaction in the app. If one plugin has multiple connected instances, use the instance ID from `integrations list`; the plugin ID is ambiguous. A document-only run is available with `--mode documents` when authentication is already handled.

For integrations with autofill fields, inspect them before setting values. Pipe secrets through stdin rather than putting them in command arguments:

```bash
invoice-radar integrations autofill fields <integration-id>
printf '%s' "$IR_LOGIN_USERNAME" | invoice-radar integrations autofill set <integration-id> username
```

The full [CLI documentation](https://invoiceradar.com/docs/cli) covers config fields, one-run autofill overrides, local plugin development, and debugging.

## Find and inspect documents

```bash
invoice-radar --json docs search "<vendor or invoice number>" --limit 25
invoice-radar docs show <document-id>
invoice-radar docs show <document-id> --text
```

`docs search` defaults to saved documents, a limit of 50, and the current organization. Add `--unsorted` to include documents awaiting review, `--include-deleted` to include deleted records, or `--deleted` to search only deleted records. Do not combine the last two flags. Search results contain the internal `id` needed by `docs show` and `docs export`; invoice numbers and provider document IDs are different fields. If several records match, compare vendor, date, amount, and source before exporting.

`docs show --text` prints extracted PDF text only when available. For a local PDF that is not already in Invoice Radar, use `invoice-radar docs import ./invoice.pdf`.

## Export and sync

```bash
invoice-radar docs export <document-id> ./invoices/invoice.pdf --dry-run
invoice-radar docs export <document-id> ./invoices/invoice.pdf
```

Export writes one PDF to the specified path. For several documents, call it once per document ID with a distinct path. An existing file requires `--force` to overwrite it. `--dry-run` checks eligibility without writing a file and reports whether a document credit would be used. A document without a PDF cannot be exported. CLI export can be unavailable for a trial organization when watermarking is required.

For an export destination already configured in the app:

```bash
invoice-radar docs sync --destination <destination-id>
invoice-radar docs sync --destination <destination-id> --invoice <document-id>
```

The first command syncs unsynced saved invoices; the second targets one. `--force` is available when a sync should run even if Invoice Radar considers the invoice unchanged. Destination setup happens in the desktop app.
Sync can also be unavailable for trial organizations when watermarking is required.

## If a command fails

- **Not connected:** open the app with `invoice-radar open --wait`, then rerun `status`.
- **No organization selected:** select one in the app or pass `--org <org-id>`.
- **No integration found:** check `integrations list` and `integrations available <service>`; use the connected instance ID for `run`.
- **Authentication needed:** complete the service login or MFA in the desktop app, then run the integration again.
- **No search match:** inspect the run result, search by vendor or invoice number, and include unsorted documents when relevant. Do not claim collection succeeded based only on an empty search.
- **Export failed:** inspect `docs show <document-id>` for PDF availability and the CLI error for path, overwrite, or trial limits.

# Invoice Radar skill

An agent skill for collecting invoices with the [Invoice Radar](https://invoiceradar.com) desktop CLI, finding saved documents, and exporting or syncing PDFs.

## Install

```bash
npx skills add invoiceradar/invoice-radar-skill --skill invoice-radar
```

The CLI currently requires macOS. Install the [desktop app](https://invoiceradar.com/download), sign in, and enable **Settings → General → CLI Access**. Verify the connection with `invoice-radar status`.

## Example

```bash
invoice-radar integrations list
invoice-radar run <integration-id>
invoice-radar docs search "<vendor or invoice number>"
invoice-radar docs export <document-id> ./invoice.pdf
```

See the [skill](skills/invoice-radar/SKILL.md) for the agent workflow and [CLI reference](skills/invoice-radar/references/cli.md) for setup, authentication, search, export, and sync examples. The [official CLI documentation](https://invoiceradar.com/docs/cli) has the full command reference.

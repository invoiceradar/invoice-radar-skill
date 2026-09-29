# Invoice Radar skill — automate invoice collection with AI agents

[Invoice Radar](https://invoiceradar.com) collects invoices from web portals and email, so they are easier to find and export. This agent skill uses the desktop CLI to run connected integrations, search saved documents, export invoice PDFs, and sync them to configured destinations.

Use it when you want an AI agent to collect invoices from a supported billing service and deliver the right document without repeating the steps in the app. Learn how [web portal invoice collection](https://invoiceradar.com/docs/invoices-from-web-portals) and [export destinations](https://invoiceradar.com/docs/export) work in Invoice Radar.

## Install

```bash
npx skills add invoiceradar/invoice-radar-skill --skill invoice-radar
```

The CLI currently requires macOS. [Download Invoice Radar](https://invoiceradar.com/download), sign in, and enable **Settings → General → CLI Access**. Verify the connection with `invoice-radar status`.

## Example

```bash
invoice-radar integrations list
invoice-radar run <integration-id>
invoice-radar docs search "<vendor or invoice number>"
invoice-radar docs export <document-id> ./invoice.pdf
```

## Learn more

- [Invoice Radar CLI documentation](https://invoiceradar.com/docs/cli) — full command reference and setup.
- [Plugin documentation](https://invoiceradar.com/docs/plugin-reference) — create an integration for another billing service.
- [Agent workflow](skills/invoice-radar/SKILL.md) and [CLI examples](skills/invoice-radar/references/cli.md) — the files installed with this skill.

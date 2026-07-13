<p align="center">
  <a href="https://omitt.app">
    <img src="assets/png/icon-128.png" alt="omitt" width="96" height="96">
  </a>
</p>

<h1 align="center">omitt</h1>

<p align="center">
  <strong>Hide sensitive data before you share.</strong><br>
  A privacy-first macOS utility for reviewing and redacting sensitive content before it reaches an AI tool.
</p>

<p align="center">
  <a href="https://omitt.app">Website</a> ·
  <a href="https://omitt.app/privacy">Privacy</a> ·
  <a href="https://omitt.app/terms">Beta Terms</a> ·
  <a href="mailto:hello@omitt.app">Contact</a>
</p>

## About

This repository contains the public website and waitlist for omitt.

The macOS app reviews copied text, PDFs, images, and screenshots on-device. It suggests supported sensitive data, secrets, faces, and barcodes for redaction, lets the user approve every mark, and creates a clean copy for sharing. Document content stays on the Mac.

## Repository

The site is intentionally small:

- Static HTML and CSS with a small amount of JavaScript
- Self-hosted Nunito Variable font
- No site analytics, advertising trackers, or document processing
- Security headers and deployment configuration in `vercel.json`
- Automatic deployment from `main` through Vercel

The waitlist sends only the email address and form context the visitor explicitly submits. Details are in the [privacy notice](https://omitt.app/privacy).

## Run locally

No build step or package installation is required.

```sh
python3 -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000).

## Contributing

Before changing copy or visuals, preserve the product rules:

- Write `omitt` in lowercase
- Keep the canonical tagline unchanged
- Use only Ink `#0D0D0C`, Cream `#F9F2ED`, and Nunito Variable
- Prefer plain language and small, verifiable claims
- Use a classic hyphen, never a Unicode em dash
- Never commit credentials, internal research, customer data, or environment files

Please report sensitive issues privately as described in [SECURITY.md](SECURITY.md).

## Status

The landing page is live. The native macOS app is in pre-beta release acceptance and is not yet available as a signed public download.

# Koinkat bank authorization callback

This repository serves https://marcosburlino.github.io/koinkat-callback/ via
GitHub Pages. It is the default OAuth redirect page for
[Koinkat](https://github.com/MarcoSburlino/Koinkat), a local-first personal
finance manager for desktop.

## What this page does

When you link a bank inside Koinkat, the app opens your bank's website so you
can approve access (PSD2, through Enable Banking). After you approve, the bank
redirects your browser to this page with an authorization code in the URL. The
page hands that code, together with the OAuth state parameter, to the Koinkat
desktop app through the app's registered koinkat://auth-callback link. That
handoff is performed by your operating system, not by a network call. The code
also stays visible on the page, so if the automatic handoff does not work you
can copy it and paste it into Koinkat manually. Koinkat then exchanges the
code for access using the Enable Banking credentials that exist only on your
machine. If the authorization fails or is cancelled, the page shows the error
instead.

## Security properties

- The whole site is one static file: [index.html](index.html). No build step,
  no dependencies, no frameworks.
- It makes zero network requests. No analytics, no external fonts or scripts,
  no images. Nothing is loaded from anywhere and nothing is sent anywhere:
  the authorization code never leaves your machine. The koinkat:// handoff is
  a URL scheme launch handled by your operating system, not a network call.
- Koinkat validates the state parameter it receives against the value it
  generated when the flow started, and rejects missing or mismatched states
  (CSRF protection).
- The displayed code is useless on its own. Redeeming it requires the private
  application key stored only on your machine, which this page never sees.
- Anyone can read index.html top to bottom and verify all of the above.

## Self-hosting

You do not have to trust this deployment. Copying and modifying index.html
for your own hosting is expressly permitted (MIT License, see below). Copy it
to any HTTPS host you control (a GitHub Pages site on your own account works),
register that URL as the redirect URL on your Enable Banking application, and
enter the same URL in Koinkat under Settings > Bank credentials.

## License

This repository is licensed under the MIT License - see [LICENSE](LICENSE).
The full notice is also embedded as a comment at the top of index.html, so a
verbatim copy of the file carries its own license with it.

The Koinkat desktop app is a separate work licensed under GPL-3.0-or-later.
This page is deliberately more permissive so that self-hosting stays
frictionless.

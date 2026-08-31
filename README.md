# Maskindiagnose.no

Statisk nettside med kontaktskjema køyrt på Cloudflare Pages.

## Kontaktskjema

Skjemaet sender ikkje personopplysningar til Formspree eller andre skjematenester. Flyten er:

1. Nettlesaren sender til `/api/contact`.
2. Pages Function verifiserer Cloudflare Turnstile, tillaten hostname, felt og storleik.
3. Ei privat Cloudflare service binding sender den validerte innsendinga til `vevsmia-mail-worker`.
4. Mail-workeren slår opp `maskindiagnose-no` i allow-lista og sender e-post til den godkjende mottakaren.

Den besøkande si adresse blir berre brukt som `Reply-To`, aldri som avsendar.

## Cloudflare Pages

Produksjonsvariablar:

- `SITE_ID=maskindiagnose-no`
- `ALLOWED_HOSTNAMES=maskindiagnose.no,www.maskindiagnose.no,maskindiagnose-no.pages.dev`
- hemmeleg variabel: `TURNSTILE_SECRET_KEY`
- service binding: `MAIL_WORKER` → `vevsmia-mail-worker`

Turnstile-widgeten skal tillate `maskindiagnose.no` og `maskindiagnose-no.pages.dev`. Byt `0x4AAAAAAEi2VkjnCz5RW0Ep` i `index.html` med den offentlege site key-en før ende-til-ende-test.

## Test

```bash
node --test test/contact-form.test.mjs
```

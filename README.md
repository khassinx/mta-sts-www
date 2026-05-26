# MTA-STS for khassinx.com

[MTA-STS (Mail Transfer Agent Strict Transport Security, RFC 8461)](https://datatracker.ietf.org/doc/html/rfc8461) policy for the `khassinx.com` domain.

## Policy

Serves at `https://mta-sts.khassinx.com/.well-known/mta-sts.txt`:

```
version: STSv1
mode: testing
mx: mail.protonmail.ch
mx: mailsec.protonmail.ch
max_age: 604800
```

- **Mode `testing`**: receivers report TLS failures but still deliver. Initial warm-up phase.
- **Future upgrade to `enforce`**: after 2-4 weeks of clean reports via TLS-RPT.
- **MX**: matches the `khassinx.com` MX records (Proton Mail).
- **max_age**: 7 days (604,800 seconds).

## DNS records (in Cloudflare zone `khassinx.com`)

```
mta-sts.khassinx.com  CNAME  khassinx.github.io     (proxied)
_mta-sts.khassinx.com TXT    "v=STSv1; id=<timestamp>"
```

Updating the policy requires:
1. Edit `.well-known/mta-sts.txt` here
2. Update the `id=` in the `_mta-sts` TXT DNS record to invalidate cached policies

## Companion records on `khassinx.com`

- TLS-RPT: `_smtp._tls.khassinx.com TXT "v=TLSRPTv1; rua=mailto:abraham@khassinx.com"` — receives aggregate reports of TLS failures.

## Related

Part of the [KhassinX](https://khassinx.com) email security stack:
- SPF: `v=spf1 include:_spf.protonmail.ch ~all`
- DKIM: 3 Proton selectors
- DMARC: `v=DMARC1; p=quarantine; sp=quarantine; rua=mailto:...`
- CAA: limited to Let's Encrypt + Google PKI
- DNSSEC: managed by Cloudflare Registrar
- MTA-STS: this repo

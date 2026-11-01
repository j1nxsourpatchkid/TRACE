# TRACE

**TRACE** is a website diagnostics and auditing CLI. It inspects
publicly accessible websites and builds a technical profile covering
infrastructure signals, network performance, security configuration, TLS
certificates, DNS records, page resources, SEO, accessibility, and page
structure.

TRACE can crawl internal pages, identify broken links, save scan
profiles, and compare two profiles to highlight changes over time.

## Features

### Technology fingerprinting

-   **Web servers and CDNs:** Cloudflare, CloudFront, Fastly, Vercel,
    Nginx, Apache, and custom edge headers.
-   **Backend and runtimes:** engines and frameworks exposed through
    `X-Powered-By` or signature headers.
-   **Frontend frameworks and UI libraries:** Next.js, Nuxt.js, React,
    Vue, jQuery, Bootstrap, and Tailwind CSS, using markup signatures
    and hydration scripts.
-   **CMS and e-commerce:** WordPress and Shopify.
-   **Analytics:** Google Analytics and Google Tag Manager.

Detection relies on public headers and markup signatures. A detected
signature is an indicator, not definitive proof of a technology's full
use.

### Performance and network diagnostics

-   Measures Time To First Byte (TTFB) and full payload retrieval
    duration.
-   Reports exact payload size and human-readable KB/MB weight.
-   Checks Brotli, gzip, and deflate compression.
-   Reads `Cache-Control` directives and `ETag` headers.

### Security auditing

-   Checks HTTPS delivery.
-   Audits six headers: `Strict-Transport-Security`,
    `Content-Security-Policy`, `X-Content-Type-Options`,
    `X-Frame-Options`, `Referrer-Policy`, and `Permissions-Policy`.
-   Inspects each `Set-Cookie` header for `Secure`, `HttpOnly`, and
    `SameSite`.
-   Calculates a Security Health score from 0 to 100.

### TLS / SSL inspection

-   Extracts certificate subject Common Name (CN) and issuing
    Certificate Authority (CA).
-   Reports valid-from date, expiration date, and days remaining.
-   Lists Subject Alternative Names (SANs).
-   Reports negotiated TLS protocol version and cipher suite.

### DNS and email security

Using the Cloudflare DNS resolver, TRACE checks: - IPv4 (`A`) and IPv6
(`AAAA`) addresses. - Mail exchanger (`MX`) hosts and
preference/priority. - Nameserver (`NS`) records. - SPF presence
(`v=spf1`) in root TXT records. - DMARC presence by querying
`_dmarc.<domain>`.

### Resource bloat and asset auditing

-   Detects synchronous scripts in `<head>` without `async` or `defer`.
-   Flags linked images, scripts, and stylesheets larger than 500 KB.
-   Identifies JavaScript and CSS assets served without compression.
-   Calculates combined byte size of discovered page assets.

### SEO diagnostics

-   Checks title presence and flags titles shorter than 30 or longer
    than 60 characters.
-   Checks meta descriptions and flags descriptions shorter than 120 or
    longer than 160 characters.
-   Checks for a canonical link and mobile viewport meta tag.
-   Counts `<h1>` elements and flags missing or duplicate primary
    headings.
-   Discovers OpenGraph (`og:`) and Twitter Card metadata.
-   Calculates an SEO Health score from 0 to 100.

### Structural accessibility

-   Checks for an `<html lang="...">` declaration and reports its value.
-   Finds images missing `alt` attributes and provides DOM selector
    hints.
-   Checks heading hierarchy for skipped levels, such as `<h1>` directly
    followed by `<h3>`.
-   Flags empty buttons and anchors without inner text or ARIA
    attributes.
-   Calculates an Accessibility score from 0 to 100.

### Page structure mapping

-   Counts DOM elements.
-   Separates internal routes from external outbound links.
-   Catalogs inline and external scripts, including deferred and
    asynchronous scripts.
-   Catalogs inline `<style>` blocks and linked stylesheets.
-   Catalogs image elements and sources.

### Recursive multi-page crawling

-   Uses breadth-first search (BFS) to discover internal links.
-   Stays bounded to the target domain.
-   Supports configurable recursion depth and maximum page count.
-   Measures load time per page.
-   Reports broken links, including 404 and 500 responses.

### Profile comparison and change tracking

`trace compare` compares two previously saved JSON scan profiles and can
highlight: - TTFB latency changes. - Content-weight changes in bytes. -
Security and SEO score changes. - Added or removed detected
technologies. - Missing or newly configured security headers. - Page
title changes.

### Report formats

  -----------------------------------------------------------------------
  Format                              Description
  ----------------------------------- -----------------------------------
  Text                                Formatted, color-coded terminal
                                      output

  JSON                                Complete machine-readable technical
                                      profile

  HTML                                Minimal monospace dashboard

  Markdown                            Tables suitable for GitHub pull
                                      requests, issues, and wikis
  -----------------------------------------------------------------------

The `scan` command supports text, JSON, HTML, and Markdown. The
`compare` command supports text, JSON, and Markdown.

## Installation

Build from source:

``` bash
cargo build --release
```

Run the resulting release executable from its output directory, or add
it to your system `PATH`.

## Quick start

``` bash
trace scan https://example.com
trace scan https://example.com --format json --output profile.json
trace cert https://example.com
trace dns https://example.com
trace crawl https://example.com --depth 2 --max-pages 15
trace compare before.json after.json
```

## Command overview

  -------------------------------------------------------------------------------
  Command                                     Purpose
  ------------------------------------------- -----------------------------------
  `trace scan <URL>`                          Full website diagnostic audit

  `trace cert <URL>`                          TLS certificate and negotiated
                                              connection inspection

  `trace dns <URL>`                           DNS records and SPF / DMARC
                                              presence checks

  `trace bloat <URL>`                         Linked asset and resource-weight
                                              audit

  `trace crawl <URL>`                         Internal-page crawl, load times,
                                              and broken links

  `trace security <URL>`                      HTTPS, security headers, and cookie
                                              audit

  `trace performance <URL>`                   Latency, payload, compression, and
                                              caching

  `trace accessibility <URL>`                 Structural accessibility checks

  `trace pages <URL>`                         Page structure and linked-resource
                                              map

  `trace compare <FILE1.json> <FILE2.json>`   Compare saved JSON scan profiles
  -------------------------------------------------------------------------------

See [commands.md](commands.md) for full syntax and
[examples.md](examples.md) for workflows.

## Understanding scores and findings

Security Health, SEO Health, and Accessibility scores use a 0--100
scale. They are automated diagnostic indicators, not formal
certifications or guarantees of security, search ranking, or
accessibility compliance.

Technology fingerprints and heuristic findings are based on publicly
observable signals. Review individual findings in context rather than
treating them as definitive proof.

## Responsible use

Only scan websites you are authorized to assess. Keep crawl depth and
page limits appropriate for the target, and avoid actions that disrupt
services or violate site policies.

------------------------------------------------------------------------

**TRACE** --- Website Intelligence & Diagnostics.

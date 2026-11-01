# TRACE --- Command Reference

This document describes every currently provided TRACE command, its
syntax, and the information it reports.

## `trace scan`

Runs the full website diagnostic audit across technology, performance,
security, TLS, DNS, resource bloat, SEO, accessibility, and
page-structure checks.

**Syntax**

``` bash
trace scan <URL> [--format <text|json|html|md>] [--output <FILE>]
```

  Option                Description
  --------------------- -----------------------------------------
  `<URL>`               Website URL to inspect
  `--format <FORMAT>`   `text`, `json`, `html`, or `md`
  `--output <FILE>`     Writes the report to the specified file

The report includes technology fingerprints; TTFB and payload timing;
payload size, compression, and caching; HTTPS, headers, cookies, and
Security Health; TLS certificate details; DNS and SPF / DMARC checks;
asset findings; SEO checks and score; accessibility checks and score;
and page structure.

Examples:

``` bash
trace scan https://example.com
trace scan https://example.com --format json --output example.json
trace scan https://example.com --format html --output report.html
trace scan https://example.com --format md --output report.md
```

## `trace cert`

Inspects the TLS/SSL certificate and negotiated TLS connection.

**Syntax**

``` bash
trace cert <URL>
```

Reports certificate subject Common Name (CN), issuing CA, valid-from
date, expiration date, days remaining, Subject Alternative Names (SANs),
negotiated TLS protocol version, and cipher suite.

``` bash
trace cert https://example.com
```

## `trace dns`

Resolves DNS records using the Cloudflare resolver and checks basic
email-domain protections.

**Syntax**

``` bash
trace dns <URL>
```

  Record / check   Description
  ---------------- ----------------------------------------
  `A`              IPv4 addresses
  `AAAA`           IPv6 addresses
  `MX`             Mail exchanger hosts and priorities
  `NS`             Nameservers
  SPF              Searches root TXT records for `v=spf1`
  DMARC            Queries `_dmarc.<domain>`

``` bash
trace dns https://example.com
```

## `trace bloat`

Inspects page resources for potentially expensive or inefficient assets.

**Syntax**

``` bash
trace bloat <URL>
```

Reports synchronous scripts in `<head>` without `async` or `defer`,
linked images/scripts/stylesheets larger than 500 KB, JavaScript and CSS
served without compression, and combined byte size of discovered page
assets.

``` bash
trace bloat https://example.com
```

## `trace crawl`

Recursively discovers and audits internal pages using breadth-first
search (BFS).

**Syntax**

``` bash
trace crawl <URL> [--depth <N>] [--max-pages <N>]
```

  Option                Default Description
  ------------------- --------- ---------------------------------------
  `--depth <N>`             `2` Maximum internal-link recursion depth
  `--max-pages <N>`        `15` Maximum number of pages to crawl

Reports discovered internal pages, per-page load time, and broken links
including HTTP 404 and 500 responses. The crawl remains bounded to the
target domain.

``` bash
trace crawl https://example.com
trace crawl https://example.com --depth 2
trace crawl https://example.com --depth 3 --max-pages 100
```

## `trace security`

Audits transport security, response headers, and cookie attributes.

**Syntax**

``` bash
trace security <URL>
```

Checks HTTPS delivery; `Strict-Transport-Security` (HSTS);
`Content-Security-Policy` (CSP); `X-Content-Type-Options`;
`X-Frame-Options`; `Referrer-Policy`; `Permissions-Policy`; and each
`Set-Cookie` header for `Secure`, `HttpOnly`, and `SameSite`. Reports a
Security Health score from 0 to 100.

``` bash
trace security https://example.com
```

## `trace performance`

Measures response timing and inspects payload and caching behavior.

**Syntax**

``` bash
trace performance <URL>
```

Reports TTFB, full payload retrieval duration, exact and human-readable
payload size, compression (Brotli, gzip, deflate, or uncompressed),
`Cache-Control` directives, and `ETag`.

``` bash
trace performance https://example.com
```

## `trace accessibility`

Checks structural accessibility fundamentals in page markup.

**Syntax**

``` bash
trace accessibility <URL>
```

Checks the root `<html lang="...">` declaration, images missing `alt`
attributes (with DOM selector hints), skipped heading levels, and empty
buttons or anchors without inner text or ARIA attributes. Reports an
Accessibility score from 0 to 100.

``` bash
trace accessibility https://example.com
```

## `trace pages`

Maps the structure and linked resources of a page.

**Syntax**

``` bash
trace pages <URL>
```

Reports total DOM element count, internal and external links, inline and
external scripts, deferred and asynchronous script attributes, inline
`<style>` blocks, linked stylesheets, and image elements and sources.

``` bash
trace pages https://example.com
```

## `trace compare`

Compares two previously saved JSON scan profiles.

**Syntax**

``` bash
trace compare <FILE1.json> <FILE2.json> [--format <text|json|md>] [--output <FILE>]
```

  -----------------------------------------------------------------------
  Option                              Description
  ----------------------------------- -----------------------------------
  `<FILE1.json>`                      First / earlier scan profile

  `<FILE2.json>`                      Second / later scan profile

  `--format <FORMAT>`                 `text`, `json`, or `md`

  `--output <FILE>`                   Writes the comparison report to the
                                      specified file
  -----------------------------------------------------------------------

Can highlight TTFB changes, content-weight deltas, Security and SEO
score changes, added or removed technologies, security-header changes,
and page-title changes.

``` bash
trace compare before.json after.json
trace compare before.json after.json --format md --output changes.md
trace compare before.json after.json --format json --output changes.json
```

## Output format support

  ----------------------------------------------------------------------------------
  Command                 Text            JSON            HTML          Markdown
  ------------------ --------------- --------------- --------------- ---------------
  `scan`                   Yes             Yes             Yes             Yes

  `compare`                Yes             Yes             No              Yes

  `cert`, `dns`,     Default output   Not specified   Not specified   Not specified
  `bloat`, `crawl`,                                                  
  `security`,                                                        
  `performance`,                                                     
  `accessibility`,                                                   
  `pages`                                                            
  ----------------------------------------------------------------------------------

Explicit `--format` and `--output` options are documented only for
`scan` and `compare`. Do not assume they are accepted by the other
commands.

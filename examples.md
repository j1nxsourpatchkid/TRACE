# TRACE --- Practical Examples

This guide demonstrates common TRACE workflows. Replace
`https://example.com` with a website you are authorized to inspect.

## 1. Run a complete website scan

``` bash
trace scan https://example.com
```

This runs the full diagnostic audit: technology fingerprinting,
performance, security, TLS, DNS, resource bloat, SEO, accessibility, and
page structure.

## 2. Export reports

Save a machine-readable JSON profile:

``` bash
trace scan https://example.com --format json --output example.json
```

Generate an HTML report:

``` bash
trace scan https://example.com --format html --output report.html
```

Generate Markdown suitable for GitHub:

``` bash
trace scan https://example.com --format md --output report.md
```

## 3. Inspect a TLS certificate

``` bash
trace cert https://example.com
```

Review the certificate subject and issuer, validity dates, remaining
days, SANs, and negotiated TLS protocol and cipher suite.

## 4. Audit DNS and email protections

``` bash
trace dns https://example.com
```

This checks A, AAAA, MX, and NS records through the Cloudflare resolver.
It also searches root TXT records for SPF and queries
`_dmarc.example.com` for DMARC.

## 5. Find large or inefficient assets

``` bash
trace bloat https://example.com
```

Review synchronous scripts in `<head>` without `async` or `defer`,
individual assets larger than 500 KB, uncompressed JavaScript or CSS,
and total discovered asset weight.

## 6. Crawl multiple pages

Use the default settings (depth 2, maximum 15 pages):

``` bash
trace crawl https://example.com
```

Set a depth explicitly:

``` bash
trace crawl https://example.com --depth 3
```

Increase the page limit:

``` bash
trace crawl https://example.com --depth 3 --max-pages 100
```

The crawler uses BFS, stays within the target domain, measures page load
time, and reports broken links.

## 7. Run a focused security audit

``` bash
trace security https://example.com
```

Review HTTPS delivery, six security headers, cookie attributes
(`Secure`, `HttpOnly`, and `SameSite`), and the Security Health score.

## 8. Investigate performance

``` bash
trace performance https://example.com
```

Review TTFB, full payload retrieval duration, payload size, compression,
`Cache-Control`, and `ETag`.

## 9. Check structural accessibility

``` bash
trace accessibility https://example.com
```

Look for missing image alt attributes, missing root language
declarations, heading hierarchy jumps, and empty interactive elements.
Review the Accessibility score alongside individual findings.

## 10. Map page structure

``` bash
trace pages https://example.com
```

Inspect DOM element counts, internal and external links, scripts,
stylesheets, and image sources.

## 11. Compare two website profiles

Save an initial scan:

``` bash
trace scan https://example.com --format json --output before.json
```

Save a later scan:

``` bash
trace scan https://example.com --format json --output after.json
```

Compare them:

``` bash
trace compare before.json after.json
```

Export the comparison as Markdown:

``` bash
trace compare before.json after.json --format md --output changes.md
```

Or save a machine-readable comparison:

``` bash
trace compare before.json after.json --format json --output changes.json
```

The comparison can highlight changes in TTFB, content weight, Security
and SEO scores, detected technologies, security headers, and page
titles.

## 12. Example audit workflow

``` bash
# Save a baseline profile
trace scan https://example.com --format json --output baseline.json

# Inspect certificate and DNS
trace cert https://example.com
trace dns https://example.com

# Review security and performance
trace security https://example.com
trace performance https://example.com

# Inspect assets and page structure
trace bloat https://example.com
trace pages https://example.com

# Check accessibility
trace accessibility https://example.com

# Crawl internal pages
trace crawl https://example.com --depth 2 --max-pages 15
```

After the site changes, create another profile and compare it with the
baseline:

``` bash
trace scan https://example.com --format json --output updated.json
trace compare baseline.json updated.json --format md --output audit-changes.md
```

## Notes

-   Only scan websites you are authorized to assess.
-   Keep crawl depth and page limits appropriate for the target.
-   Automated scores and technology fingerprints are diagnostic
    indicators; review individual findings before drawing conclusions.
-   Explicit report-format and output-file options are documented for
    `scan` and `compare`. Other commands use their normal command
    output.

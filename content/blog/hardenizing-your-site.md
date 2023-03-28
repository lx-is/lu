---
title: "Hardenizing your site"
date: "2023-03-27"
description: ""
tags:
- "security"
- "webdev"
draft: true
---

If your domain is having trouble rising to the occasion, it may be time to consider some measures to strengthen its performance. Impairments in your domain's security can lead to issues with downtime and poor user experience. But fear not, there are ways to improve your domain's resilience and give it the boost it needs. For only $9.99 … okay that's enough.

In all seriousness, the point of this post is to document my processing in "maxing out" my domain's security, at least according to Hardenize's report.

What [Hardenize](https://www.hardenize.com) does is generate a report of your domain and site's security, i.e., email configuration, content security policy, etc. They additionally offer more advanced, paid services, but I've never looked into them.\
*[https://www.hardenize.com/report/lx-is.lu](https://www.hardenize.com/report/lx-is.lu)*

I should preface by stating I'm not a security expert. This post is merely aimed towards those just hosting simple, inconsequential sites. I figured documenting my process of hardening my site could be helpful to someone, or even just me in the future. As such, I won't be going through every report item, but just the ones I had to configure in some way to get all the holy green boxes.

### DNS
#### DNSSEC
Domain Name System Security Extensions, or DNSSEC, authenticates responses to domain name lookups by digitally signing data. The signing process occurs at every level in the DNS lookup process, and, as a result, all answers from DNS can be trusted.

I wouldn't say it's super important (not even Google makes use of it?), but it's pretty easy to set up as long as your registrar and DNS provider both support it. I personally recommend Cloudflare as a DNS provider.

To enable DNSSEC: you first need to go to your DNS provider, as they will generate some records to input into your domain registrar. Keep in mind not all registrars will require all the records for DNSSEC to be successfully enabled.

As it's a slightly different process for every provider and registrar, I'm not going to detail it here. If DNSSEC is supported by both your DNS provider and your domain registrar, then they will likely have good documentation on how to enable it.

#### Certification Authority Authorization
Certification Authority Authorization (CAA) is a standard that allows domain name owners to restrict which certificate authorities are allowed to issue certificates for their domains. This can help to reduce the chance of mis-issuance, either accidentally or maliciously. It's essentially an allowlist.

CAA is easy to set up. Visit this [CAA Record Helper](https://sslmate.com/caa/) and input your domain. Then choose to auto-generate a policy, scroll down, and add the suggested records to your domain's DNS.

Keep in mind that CAA can be an administrative burden if you require SSL certificates from numerous, different authorities.

### Email
#### MTA Strict Transport Security
MTA-STS enhances the transmission of emails over SMTP. It's a relatively new standard; most email providers don't even use it yet. Still, it's good to set up.

To set up MTA-STS, you'll need to host `mta-sts.txt` in the `.well-known` directory of a web server accessible through your "mta-sts" subdomain. I worded that poorly; to visualise, here's how the URL would look: `https://mta-sts.lx-is.lu/.well-known/mta-sts.txt`.

The text file contains the following, each on their own line: version, mode, mail server(s), max age.

```
version: STSv1
mode: enforce
mx: mx1.mailserver.net
mx: mx2.mailserver.net
max_age: 604800
```

You can just copy that and replace the mail servers with your own.

Hosting it is also fairly simple given there's countless static site hosters. I use Cloudflare Pages, but some other options include: Vercel, Netlify, and GitHub Pages.

Simply just create a new repository on GitHub, create `mta-sts.txt` in the `.well-known` directory, and publish the repository using one of the aforementioned hosts. From there, just connect it to your "mta-sts" subdomain.

Lastly, you need to create a TXT record under `_mta-sts`. The contents of the record should include a version and an ID. Example: `v=STSv1; id=20230330`.

The ID can be whatever, but for simplicity you can just put the date you created the record. If you ever change your `mta-sts.txt` file (i.e., different mail servers), you'll need to change the ID.

#### SMTP TLS Reporting
TLS-RPT goes hand-in-hand with MTA-STS. It's a way to receive reports on potential failures with MTA-STS. It's a young standard that is not yet widely supported, but if you have MTA-STS it's good to have.

To enable TLS-RPT, simply create a TXT record under `_smtp._tls` with the version and recipient. Example: `v=TLSRPTv1; rua=mailto:postmaster@domain.com`.

#### SPF & DMARC
Sender Policy Framework and Domain-based Message Authentication, Reporting, & Conformance (lol what an acronym) are two very important email email authentication mechanisms used to prevent email spoofing and protect against email phishing and spam.

Unfortunately, if your mail provider does not support them then you're out of luck. I'd suggest switching email providers.

If your email provider does support them, simply read their documentation and add the appropriate DNS records.

### WWW
#### HTTP Strict Transport Security
HSTS is a security enhancement that tells browsers to only communicate with the website over HTTPS. With HSTS enabled, browsers no longer allow bypassing certificate warnings or errors.

You'll need to include Strict Transport Security in your site's headers. I serve my HSTS header Cloudflare's Edge Certificates settings. However, HSTS can also be served through a _headers file located in the root of your web server, through `<meta>` tags, or however else headers can be served.

In `_headers`, my configuration would look as such:\
`Strict-Transport-Security : max-age=63072000; includeSubDomains; preload`

If I were to use the inferior meta tag method, it would look as such:\
`<meta http-equiv="Strict-Transport-Security" content="max-age=63072000; includeSubDomains">`

HSTS can be preloaded; it's an initiative by The Chromium Project. If your HSTS is preloaded, then you must ensure that you always serve HTTPS connections otherwise your site will be inaccessible.

If you don't want to preload, don't include the "preload" in your configuration. If you do want to preload, then do include it and submit your domain to [hstspreload.org](https://hstspreload.org).

#### Content Security Policy
CSP can restrict what types of resources are loaded and from where, thus defending against cross-site scripting and preventing mixed content issues (if configured in such a way).

Because none of the content on my website (e.g., images, fonts, etc) is external, I restrict my site to only itself. I do this through my `_headers` file, but it can also be served other ways, such as a meta tag.

My CSP is as follows:

```
Content-Security-Policy : default-src 'none'; connect-src 'self'; img-src 'self'; script-src 'self'; style-src 'self'; font-src 'self'; form-action 'none'; frame-ancestors 'none'; block-all-mixed-content; base-uri 'none'
```

If your site does require external connections, then you can modify your CSP to explicitly allow them per resource.

#### Subresource Integrity
SRI is a new standard that enables browsers to verify the integrity of embedded page resources (e.g., scripts and stylesheets).

I use SRI for my stylesheet, despite it, for me, not being necessary whatsoever. I just wanted the green checkbox on Hardenize.

To accomplish this, as my site is built with the Hugo framework, I simply used Hugo's built-in [fingerprint pipe](https://gohugo.io/hugo-pipes/fingerprint/). By doing so, SRI is completely automated and done whenever my site is compiled.

SRI can be done manually, even if it is tedious. To get the hash (or fingerprint) of your resource, you can run `openssl dgst -sha512 -binary main.css | openssl base64 -A` on it. This runs it on `main.css`, for example.

Then, when linking the resource, you add the "integrity" attribute. For example:\
`<link rel="stylesheet" href="main.css" integrity="sha512-/1mUX8ugmMyGwzq8CgJUr+DRL8pG/XErGT1MU8wunbxx7MB4iSofvM0+fVtsIzv/cyYkdTQhiml+V38KdfXzSQ==%" crossorigin="anonymous">`

#### Frame Options
The X-Frame-Options header controls page framing, which occurs when a page is incorporated into some other page, possibly on a different site.

For this, just set `X-Frame-Options` to `DENY` and call it a day. HTML frames are obsolete as far as I'm concerned. Again, this can be done however you serve your headers (e.g., _headers, meta tags, etc).

#### XSS Protection
To best protect against cross-site scripting, the `X-XSS-Protection` header should be set to `0`. As such, the browser's built-in protections are not relied upon.

Again, this can be done however you serve your headers (e.g., _headers, meta tags, etc).

#### Content Type Options
Lastly, content type options. Some browsers have a feature called MIME-type sniffing, however this can sometimes lead to security vulnerabilities, as an attacker could trick the browser into executing malicious code by sending it a file that appears to be one type of content (e.g. an image), but is actually a different type of content (e.g. JavaScript).

I have no idea how prevalent such an attack is, but it's easy to prevent. Just set the `X-Content-Type-Options` header to `nosniff`. Do note, due to its nature, this header cannot be served with a meta tag.

### My configuration
My Hardenize report can be found [here](https://www.hardenize.com/report/lx-is.lu), my headers can be found [here](https://raw.githubusercontent.com/lx-is/lu/main/static/_headers), and my MTA-STS configuration can be found [here](https://mta-sts.lx-is.lu/.well-known/mta-sts.txt).

### Further reading

How DNSSEC Works
: https://www.cloudflare.com/dns/dnssec/how-dnssec-works/

Increase email security with MTA-STS and TLS reporting
: https://support.google.com/a/answer/9261504?hl=en

How to Spot Any Spoofed & Fake Email (Ultimate Guide)
: https://www.youtube.com/watch?v=hF1bIT1ym4g

HTTP Strict Transport Security
: https://www.chromium.org/hsts/

Content Security Policy (CSP)
: https://developer.mozilla.org/docs/Web/HTTP/CSP

Subresource Integrity
: https://developer.mozilla.org/docs/Web/Security/Subresource_Integrity

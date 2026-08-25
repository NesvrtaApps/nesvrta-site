# Deploying nesvrta.com — free hosting, domain stays at GoDaddy, email untouched

GoDaddy has no free hosting tier. But the domain does not need to be
hosted where it is registered. This guide deploys the site to a free
host and points DNS records at GoDaddy to it — WITHOUT changing
nameservers, so your Microsoft 365 email is never at risk.

Two options below. Option A (GitHub Pages) is recommended.

---

## Option A — GitHub Pages (recommended)

Free forever. Custom domain. Free auto-renewing SSL. No nameserver
change. You already have a GitHub account (MealPause org).

### A1. Create the repo and upload the site

1. Go to github.com → New repository
   - Owner: your personal account or the org — either works
   - Name: `nesvrta-site` (any name works)
   - Visibility: **Public** (GitHub Pages on private repos requires
     a paid plan; the site content is public anyway)
2. On the new repo page: "uploading an existing file" link
3. Drag ALL files and the assets folder from this package in
   (index.html, privacy.html, terms.html, styles.css, robots.txt,
   sitemap.xml, CNAME, README.md, assets/)
4. Commit.

### A2. Turn on GitHub Pages

1. Repo → Settings → Pages
2. Source: "Deploy from a branch"
3. Branch: `main`, folder `/ (root)` → Save
4. Wait ~1 minute. The site goes live at
   `https://<username>.github.io/nesvrta-site/`
5. Because the repo contains a `CNAME` file with `nesvrta.com`,
   the Custom domain field should auto-fill. If not, type
   `nesvrta.com` into Settings → Pages → Custom domain and Save.

### A3. Point DNS at GoDaddy (no nameserver change)

Log into GoDaddy → My Products → nesvrta.com → **Manage DNS**.
You are only ADDING/EDITING individual records. Do not touch
nameservers, MX records, or anything email-related.

Add these records:

| Type  | Name | Value               | TTL     |
|-------|------|---------------------|---------|
| A     | @    | 185.199.108.153     | default |
| A     | @    | 185.199.109.153     | default |
| A     | @    | 185.199.110.153     | default |
| A     | @    | 185.199.111.153     | default |
| CNAME | www  | <username>.github.io | default |

(Replace `<username>` with your GitHub username, e.g.
`darkostankovic.github.io` — NOT the repo name.)

If GoDaddy already has an A record for `@` (often "Parked" pointing
to GoDaddy's parking page), EDIT it to the first IP above and add
the other three as new A records. If a `www` CNAME already exists
(often pointing to `@` or GoDaddy's forwarder), edit it.

**Do not touch:** MX records, TXT records containing `v=spf1`,
CNAMEs named `autodiscover`, `msoid`, `sip`, `lyncdiscover`,
anything with `_domainkey`, or any SRV records. Those run your
email.

### A4. Enforce HTTPS

1. Back in GitHub → Settings → Pages
2. Wait for the "DNS check" next to the custom domain to pass
   (minutes to a few hours after the GoDaddy records propagate)
3. Check **"Enforce HTTPS"** once the checkbox becomes available.
   GitHub provisions a free Let's Encrypt certificate automatically.

### A5. Verify

- https://nesvrta.com loads the site
- https://www.nesvrta.com loads the site (redirects to apex)
- Send a test email to support@nesvrta.com — confirms email
  untouched (it will be — you never touched MX records)

### Updating the site later

Edit files directly on github.com (pencil icon on any file) or
upload replacements. Every commit redeploys automatically in
about a minute.

---

## Option B — Cloudflare Pages (if you finish the nameserver migration)

You already added nesvrta.com to Cloudflare. If you complete that
migration (nameservers → Cloudflare, following the email-records
safety checklist from our conversation), then:

1. Cloudflare dashboard → Workers & Pages → Create → Pages →
   **Upload assets** (direct upload — no git needed)
2. Drag this whole folder in. Project name: `nesvrta`.
3. Deploy. Site goes live at `nesvrta.pages.dev`.
4. Project → Custom domains → add `nesvrta.com` and
   `www.nesvrta.com`. Since the zone is on Cloudflare, records are
   created automatically and SSL is instant.

This path gives Cloudflare's CDN on the apex domain, but requires
completing the nameserver migration first — with the mandatory
step of verifying every email DNS record (MX, SPF, autodiscover,
DKIM, msoid, SRV) was imported and set to "DNS Only" (grey cloud)
BEFORE flipping nameservers at GoDaddy.

If you go this route, delete the CNAME file from the upload
(GitHub-specific; harmless but unnecessary).

---

## Which to choose

- Want the site live today with zero email risk → **Option A**.
  The pending Cloudflare zone can sit unused indefinitely (or
  remove it: Cloudflare → nesvrta.com → Overview → Advanced
  Actions → Remove from Cloudflare). Nothing at GoDaddy changes
  until nameservers are actually swapped, so leaving it pending
  is also harmless.
- Want Cloudflare's CDN and are ready to do the careful DNS
  migration → **Option B**.

You can also do A now and migrate to B later — the site files are
identical either way.

# Deploying dantepuglisi.com (static site + email on same domain)

Email MX records and website records coexist — nothing in the email setup conflicts with this.

## Option A — S3 + CloudFront (stays in AWS, ~$0.5/mo)

1. ACM cert in **us-east-1** for `dantepuglisi.com` + `www.dantepuglisi.com` (DNS validation → Route 53 auto-creates the records).
2. S3 bucket, upload `index.html`. Keep the bucket private.
3. CloudFront distribution: S3 origin with Origin Access Control, alternate domain names `dantepuglisi.com` + `www`, attach the cert, default root object `index.html`.
4. Route 53: A-record **alias** for root → CloudFront distribution; same for `www` (or CNAME www → root).

## Option B — GitHub Pages (free, 10 min)

1. Repo with `index.html` + file named `CNAME` containing `dantepuglisi.com`.
2. Settings → Pages → deploy from main.
3. Route 53: A records at root → GitHub Pages IPs (185.199.108.153 / .109. / .110. / .111.), CNAME `www` → `<username>.github.io`. Enable "Enforce HTTPS" once the cert issues.

Recommendation: Option A since you live in AWS anyway and it keeps everything in one place.

## Later upgrades

- Swap the mailto CTA for a Cal.com or Calendly link once you set one up (search for `mailto:` in index.html — two buttons + footer).
- Add a testimonials section after the first 3 sessions (highest-converting element the page can have; Claude can add it when quotes exist).
- Add Plausible or simple analytics if you want to see traffic.

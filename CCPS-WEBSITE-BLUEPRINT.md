# Camden Cultural Preservation Society — Website Blueprint

## Project Overview

A minimal, single-page website for Sydney-based ambient drone collective Camden Cultural Preservation Society.

**Domain:** `camdenculturalpreservationsociety.com`

---

## Architecture

| Component | Service | Cost |
|-----------|---------|------|
| Hosting | GitHub Pages | Free |
| DNS | Cloudflare | Free |
| SSL | GitHub Pages (auto) | Free |
| Analytics | Cloudflare Analytics | Free |
| Contact email | Cloudflare Email Routing | Free |
| Mailing list | Buttondown | Free (up to 100 subscribers) |
| Domain | (already registered) | ~$12/year |

**Total annual cost:** ~$12 (domain renewal only)

---

## Repository Structure

```
camdenculturalpreservationsociety/
├── index.html          # Main website
├── CNAME               # Custom domain config
└── README.md           # Repo documentation
```

---

## Setup Sequence

### Phase 1: GitHub Repository

#### 1.1 Create repository

1. Go to https://github.com/new
2. Repository name: `camdenculturalpreservationsociety`
3. Set to **Public** (required for GitHub Pages on free/pro plans)
4. Do not initialize with README (we'll push files)
5. Click **Create repository**

#### 1.2 Add website files

Create these files locally or via GitHub's web interface:

**index.html**
- The main website file (already built)

**CNAME**
- Plain text file containing only:
```
camdenculturalpreservationsociety.com
```

#### 1.3 Enable GitHub Pages

1. Go to repository → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main` / `root`
4. Click **Save**
5. Note the GitHub Pages URL: `https://liamhbray.github.io/camdenculturalpreservationsociety/`

---

### Phase 2: Cloudflare DNS

#### 2.1 Add site to Cloudflare

1. Go to https://dash.cloudflare.com
2. Click **Add a site**
3. Enter: `camdenculturalpreservationsociety.com`
4. Select **Free** plan
5. Cloudflare will scan existing DNS records

#### 2.2 Update nameservers

1. Cloudflare will provide two nameservers, e.g.:
   - `ada.ns.cloudflare.com`
   - `bob.ns.cloudflare.com`
2. Go to your domain registrar
3. Replace existing nameservers with Cloudflare's
4. Wait for propagation (can take up to 24 hours, usually faster)

#### 2.3 Configure DNS records for GitHub Pages

In Cloudflare DNS dashboard, add these records:

| Type | Name | Content | Proxy status |
|------|------|---------|--------------|
| A | @ | 185.199.108.153 | Proxied |
| A | @ | 185.199.109.153 | Proxied |
| A | @ | 185.199.110.153 | Proxied |
| A | @ | 185.199.111.153 | Proxied |
| CNAME | www | liamhbray.github.io | Proxied |

#### 2.4 SSL/TLS settings

1. In Cloudflare, go to **SSL/TLS** → **Overview**
2. Set mode to **Full**
3. Go to **Edge Certificates**
4. Enable **Always Use HTTPS**

#### 2.5 Verify in GitHub

1. Return to GitHub → **Settings** → **Pages**
2. Under **Custom domain**, enter: `camdenculturalpreservationsociety.com`
3. Click **Save**
4. Wait for DNS check to pass
5. Enable **Enforce HTTPS** once available

---

### Phase 3: Cloudflare Email Routing

#### 3.1 Enable email routing

1. In Cloudflare dashboard, go to **Email** → **Email Routing**
2. Click **Get started**
3. Cloudflare will add required MX and TXT records automatically

#### 3.2 Add destination address

1. Click **Destination addresses**
2. Add your personal email (e.g., `liambray@gmail.com`)
3. Verify via confirmation email

#### 3.3 Create routing rules

| Custom address | Forwards to |
|----------------|-------------|
| `hello@camdenculturalpreservationsociety.com` | Your personal email |
| `booking@camdenculturalpreservationsociety.com` | Your personal email |

Optionally enable **Catch-all** to forward all unmatched addresses.

---

### Phase 4: Buttondown Mailing List

#### 4.1 Create account

1. Go to https://buttondown.email
2. Sign up (free tier: 100 subscribers)
3. Set newsletter name: `Camden Cultural Preservation Society`

#### 4.2 Get embed code or link

**Option A — Link to hosted signup page:**
```html
<a href="https://buttondown.email/camdenculturalpreservationsociety">Subscribe</a>
```

**Option B — Embedded form:**
```html
<form
  action="https://buttondown.email/api/emails/embed-subscribe/camdenculturalpreservationsociety"
  method="post"
  target="popupwindow"
>
  <input type="email" name="email" placeholder="Enter your email" required />
  <button type="submit">Subscribe</button>
</form>
```

#### 4.3 Configure custom sending domain (optional)

To send emails *from* `hello@camdenculturalpreservationsociety.com`:

1. In Buttondown → **Settings** → **Sending**
2. Add custom domain
3. Add the required DNS records in Cloudflare:
   - SPF (TXT record)
   - DKIM (TXT record)
   - DMARC (TXT record)

Buttondown will provide the exact values.

---

### Phase 5: Cloudflare Analytics

#### 5.1 Enable Web Analytics

1. In Cloudflare dashboard, go to **Analytics & Logs** → **Web Analytics**
2. Click **Add a site**
3. Select `camdenculturalpreservationsociety.com`
4. Choose **JS Snippet** method
5. Copy the script tag

#### 5.2 Add to website

Add before `</body>` in `index.html`:

```html
<!-- Cloudflare Web Analytics -->
<script defer src='https://static.cloudflareinsights.com/beacon.min.js' 
  data-cf-beacon='{"token": "YOUR_TOKEN_HERE"}'></script>
```

**Note:** If using Cloudflare proxy (orange cloud), you can also enable automatic analytics injection without modifying your HTML.

---

## Website Updates

### Making changes

**Option A — GitHub web interface:**
1. Go to repo → click on `index.html`
2. Click pencil icon (Edit)
3. Make changes
4. Click **Commit changes**
5. Site updates automatically (1-2 minutes)

**Option B — Git locally:**
```bash
git clone https://github.com/Liamhbray/camdenculturalpreservationsociety.git
cd camdenculturalpreservationsociety
# make edits
git add .
git commit -m "Update description"
git push
```

---

## Future Releases

When adding new releases, duplicate the release section in `index.html`:

1. Copy the entire `<section id="store" class="release">` block
2. Update:
   - Title
   - Bandcamp embed code (get new `album=XXXXXX` ID)
   - Track listing
   - Credits
3. Move older releases below newer ones

---

## DNS Records Summary

Once fully configured, your Cloudflare DNS should have:

| Type | Name | Content | Purpose |
|------|------|---------|---------|
| A | @ | 185.199.108.153 | GitHub Pages |
| A | @ | 185.199.109.153 | GitHub Pages |
| A | @ | 185.199.110.153 | GitHub Pages |
| A | @ | 185.199.111.153 | GitHub Pages |
| CNAME | www | liamhbray.github.io | GitHub Pages |
| MX | @ | (Cloudflare values) | Email routing |
| TXT | @ | (SPF record) | Email auth |
| TXT | @ | (DKIM record) | Email auth |
| TXT | @ | (DMARC record) | Email auth |

---

## Checklist

- [ ] Create GitHub repository
- [ ] Add `index.html`
- [ ] Add `CNAME` file
- [ ] Enable GitHub Pages
- [ ] Add site to Cloudflare
- [ ] Update nameservers at registrar
- [ ] Configure DNS A records for GitHub
- [ ] Configure DNS CNAME for www
- [ ] Set SSL to Full in Cloudflare
- [ ] Verify custom domain in GitHub Pages
- [ ] Enable HTTPS enforcement
- [ ] Enable Cloudflare Email Routing
- [ ] Add destination email address
- [ ] Create email aliases (hello@, booking@)
- [ ] Create Buttondown account
- [ ] Update website with mailing list form
- [ ] Enable Cloudflare Web Analytics
- [ ] Add analytics script to website
- [ ] Test everything

---

## Support Links

- GitHub Pages docs: https://docs.github.com/en/pages
- Cloudflare DNS setup: https://developers.cloudflare.com/dns/
- Cloudflare Email Routing: https://developers.cloudflare.com/email-routing/
- Buttondown docs: https://docs.buttondown.email/
- Bandcamp embeds: https://get.bandcamp.help/hc/en-us/articles/360007902213-Embedding-your-music

---

## Files

The `index.html` file has been provided separately.

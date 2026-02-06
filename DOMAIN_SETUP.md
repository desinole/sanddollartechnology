# Domain Setup Guide: sanddollartechnology.com with GoDaddy

## Overview
This guide walks you through connecting your GoDaddy domain (sanddollartechnology.com) to your GitHub Pages site.

---

## Option 1: Full Custom Domain (Recommended)

This approach uses your custom domain directly with GitHub Pages for better SEO and professional appearance.

### Step 1: Configure DNS in GoDaddy

1. **Login to GoDaddy**
   - Go to https://godaddy.com and sign in
   - Navigate to "My Products"
   - Find your domain `sanddollartechnology.com` and click "DNS"

2. **Add DNS Records**

   Delete any existing A records, then add these **A Records** pointing to GitHub Pages servers:
   
   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | A | @ | 185.199.108.153 | 600 seconds |
   | A | @ | 185.199.109.153 | 600 seconds |
   | A | @ | 185.199.110.153 | 600 seconds |
   | A | @ | 185.199.111.153 | 600 seconds |

3. **Add CNAME for www subdomain**
   
   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | CNAME | www | sanddollartechnology.github.io | 1 Hour |

4. **Save Changes**
   - Click "Save" or "Save All Changes"
   - DNS propagation can take 24-48 hours (usually much faster)

### Step 2: Configure GitHub Pages

1. **Add CNAME file to repository**
   - Create a file named `CNAME` (no extension) in your repository root
   - Add one line: `sanddollartechnology.com`

2. **Enable HTTPS on GitHub**
   - Go to https://github.com/sanddollartechnology/sanddollartechnology/settings/pages
   - Under "Custom domain", enter: `sanddollartechnology.com`
   - Click "Save"
   - Wait a few minutes, then check "Enforce HTTPS"

### Step 3: Update Jekyll Configuration

Update `_config.yml`:

```yaml
url: "https://sanddollartechnology.com"
baseurl: ""
```

Commit and push:
```bash
git add CNAME _config.yml
git commit -m "Configure custom domain"
git push
```

### Step 4: Verify Setup

After DNS propagates (15 minutes to 48 hours):
- Visit https://sanddollartechnology.com
- Visit https://www.sanddollartechnology.com (should redirect to main)
- Verify HTTPS lock icon appears in browser

---

## Option 2: Simple Domain Forwarding

This is quicker but less ideal for SEO. Your domain forwards to the GitHub Pages URL.

### In GoDaddy:

1. **Login to GoDaddy**
   - Go to https://godaddy.com
   - Navigate to "My Products" → "Domains"

2. **Set Up Forwarding**
   - Click on your domain `sanddollartechnology.com`
   - Click "Manage DNS" or "Domain Settings"
   - Find "Forwarding" section
   - Click "Add Forwarding"

3. **Configure Forward Settings**
   - **Forward to**: `https://sanddollartechnology.github.io/sanddollartechnology`
   - **Forward type**: Permanent (301)
   - **Settings**: Forward only *(or "with masking" if you want URL to stay as sanddollartechnology.com)*
   - Click "Save"

4. **Set Up www Subdomain**
   - Add another forward for `www.sanddollartechnology.com`
   - **Forward to**: `https://sanddollartechnology.com`
   - **Forward type**: Permanent (301)
   - Click "Save"

### Propagation Time
- Forwarding usually takes effect within 1 hour
- May take up to 24-48 hours for global DNS propagation

---

## Comparison: Custom Domain vs Forwarding

### Custom Domain (Option 1) ✅ Recommended
**Pros:**
- Better SEO - search engines see your custom domain
- Professional URL in browser address bar
- Full HTTPS support
- Better user experience
- No redirect delay

**Cons:**
- Slightly more complex setup
- Requires DNS configuration
- Longer propagation time

### Domain Forwarding (Option 2)
**Pros:**
- Quick and easy setup
- No DNS configuration needed
- Works immediately

**Cons:**
- Poorer SEO - seen as duplicate content
- URL shows GitHub Pages address (unless masked)
- Less professional appearance
- Redirect adds slight delay

---

## Troubleshooting

### DNS Not Resolving
- **Wait**: DNS changes take time (up to 48 hours)
- **Check**: Use https://dnschecker.org to verify propagation
- **Clear cache**: Clear your browser cache and DNS cache
  ```bash
  # Windows
  ipconfig /flushdns
  ```

### Certificate Errors / HTTPS Not Working
- Wait 24 hours after adding custom domain
- Ensure CNAME file contains only the domain (no https://)
- Try unchecking and rechecking "Enforce HTTPS" in GitHub settings
- Verify DNS A records are correct

### Site Not Loading / 404 Error
- Check that `baseurl: ""` in _config.yml (empty for custom domain)
- Verify CNAME file exists in repository root
- Confirm GitHub Pages is enabled and building successfully
- Check Actions tab for build errors

### www Not Working
- Ensure CNAME record for www subdomain is set correctly
- Value should be: `sanddollartechnology.github.io` (not the full URL)
- Wait for DNS propagation

### Mixed Content Warnings
- Update all asset URLs to use `{{ '/assets/...' | relative_url }}`
- Ensure `url` in _config.yml uses `https://`

---

## Verification Commands

Test DNS configuration:
```bash
# Check A records
nslookup sanddollartechnology.com

# Check CNAME
nslookup www.sanddollartechnology.com

# Test with dig (Mac/Linux)
dig sanddollartechnology.com
dig www.sanddollartechnology.com
```

Expected results:
- `sanddollartechnology.com` should show GitHub's A record IPs
- `www.sanddollartechnology.com` should show CNAME to sanddollartechnology.github.io

---

## Quick Reference Card

### GoDaddy DNS Settings
```
A Record:
  Name: @
  Value: 185.199.108.153 (and 3 others)

CNAME Record:
  Name: www
  Value: sanddollartechnology.github.io
```

### GitHub Repository Files
```
CNAME file content:
  sanddollartechnology.com

_config.yml:
  url: "https://sanddollartechnology.com"
  baseurl: ""
```

---

## Support Resources

- **GitHub Pages Custom Domains**: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
- **GoDaddy DNS Help**: https://www.godaddy.com/help/manage-dns-680
- **DNS Checker**: https://dnschecker.org
- **SSL Certificate Check**: https://www.sslshopper.com/ssl-checker.html

---

## Timeline

| Step | Time |
|------|------|
| GoDaddy DNS configuration | 5 minutes |
| Create CNAME file & update config | 2 minutes |
| GitHub Pages rebuild | 2-5 minutes |
| DNS propagation | 1-48 hours (typically 15-60 mins) |
| HTTPS certificate provisioning | 1-24 hours |

**Total estimated time**: 1-48 hours for full propagation, but often works within the first hour.

---

**Need help?** If you encounter issues after 48 hours, check GitHub Actions for build errors or contact GitHub Support.

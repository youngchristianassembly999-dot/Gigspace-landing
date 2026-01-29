# Deployment Guide for GigSpace Landing Page

This guide provides step-by-step instructions for deploying your GigSpace landing page to various platforms.

## Table of Contents
- [Netlify Deployment (Recommended)](#netlify-deployment)
- [GitHub Pages Deployment](#github-pages-deployment)
- [Vercel Deployment](#vercel-deployment)
- [Custom Server Deployment](#custom-server-deployment)

---

## Netlify Deployment (Recommended)

Netlify is recommended because it provides the easiest integration with Decap CMS.

### Prerequisites
- GitHub account
- Netlify account (free)

### Steps

#### 1. Prepare Your Repository

```bash
# Initialize git repository (if not already done)
git init

# Add all files
git add .

# Commit changes
git commit -m "Initial commit: GigSpace landing page"

# Create repository on GitHub (via GitHub website)
# Then connect your local repo to GitHub:
git remote add origin https://github.com/YOUR_USERNAME/gigspace-landing.git

# Push to GitHub
git push -u origin main
```

#### 2. Deploy to Netlify

1. Go to [netlify.com](https://www.netlify.com/) and sign in
2. Click "Add new site" → "Import an existing project"
3. Choose "GitHub" as your Git provider
4. Authorize Netlify to access your repositories
5. Select your `gigspace-landing` repository
6. Configure build settings:
   - **Branch to deploy**: `main`
   - **Build command**: (leave empty)
   - **Publish directory**: `/`
7. Click "Deploy site"

Your site will be deployed at a URL like `https://random-name-12345.netlify.app`

#### 3. Set Up Custom Domain (Optional)

1. In Netlify dashboard, go to "Domain settings"
2. Click "Add custom domain"
3. Enter your domain (e.g., `gigspace.com`)
4. Follow DNS configuration instructions
5. Netlify will automatically provision SSL certificate

#### 4. Enable Netlify Identity (For CMS Access)

1. In your site dashboard, go to "Identity"
2. Click "Enable Identity"
3. Under "Registration preferences":
   - Select "Invite only" (recommended for security)
4. Under "Services":
   - Enable "Git Gateway"
5. Under "Emails":
   - Customize email templates if desired

#### 5. Invite Users to CMS

1. Go to "Identity" tab
2. Click "Invite users"
3. Enter email addresses of people who should have CMS access
4. They'll receive an email to set up their account

#### 6. Access Your CMS

- Visit `https://your-site-name.netlify.app/admin`
- Log in with Netlify Identity credentials
- Start managing content!

### Environment Variables (if needed)

1. Go to "Site settings" → "Environment variables"
2. Add any needed variables
3. Redeploy for changes to take effect

---

## GitHub Pages Deployment

GitHub Pages is free and great for simple static sites.

### Prerequisites
- GitHub account

### Steps

#### 1. Prepare Repository

```bash
# Initialize git repository
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit"

# Create repository on GitHub
# Then add remote and push
git remote add origin https://github.com/YOUR_USERNAME/gigspace-landing.git
git push -u origin main
```

#### 2. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click "Settings" → "Pages"
3. Under "Source":
   - Select branch: `main`
   - Select folder: `/ (root)`
4. Click "Save"

Your site will be available at:
- `https://YOUR_USERNAME.github.io/gigspace-landing/`

#### 3. Custom Domain (Optional)

1. In "Pages" settings, add your custom domain
2. Create a `CNAME` file in your repository root:
   ```
   gigspace.com
   ```
3. Configure DNS at your domain registrar:
   - Add CNAME record pointing to `YOUR_USERNAME.github.io`
4. GitHub will automatically provision SSL

### CMS Setup with GitHub Pages

For CMS functionality with GitHub Pages, you need to configure OAuth:

1. Edit `admin/config.yml`:
   ```yaml
   backend:
     name: github
     repo: YOUR_USERNAME/gigspace-landing
     branch: main
   ```

2. Create GitHub OAuth App:
   - Go to GitHub Settings → Developer settings → OAuth Apps
   - Click "New OAuth App"
   - Fill in details:
     - Application name: GigSpace CMS
     - Homepage URL: `https://YOUR_USERNAME.github.io/gigspace-landing/`
     - Callback URL: `https://api.netlify.com/auth/done`
   - Save Client ID and Client Secret

3. Use Netlify's authentication service (even for GitHub Pages):
   - Deploy a simple authentication service on Netlify
   - Or use third-party OAuth providers

**Note**: CMS setup is more complex with GitHub Pages. Netlify is recommended for easier CMS integration.

---

## Vercel Deployment

Vercel is another excellent option for static site deployment.

### Prerequisites
- GitHub account
- Vercel account (free)

### Steps

1. **Push to GitHub** (same as above)

2. **Deploy to Vercel**:
   - Go to [vercel.com](https://vercel.com/)
   - Click "Import Project"
   - Select your GitHub repository
   - Configure:
     - Framework Preset: Other
     - Build Command: (leave empty)
     - Output Directory: `./`
   - Click "Deploy"

3. **Custom Domain**:
   - In project settings, add custom domain
   - Configure DNS as instructed

4. **CMS Setup**:
   - Similar to GitHub Pages (requires OAuth setup)
   - Or deploy with Netlify for CMS and use Vercel for main site

---

## Custom Server Deployment

For deploying to your own server or shared hosting.

### Via FTP/SFTP

1. **Connect to your server** using FTP client (FileZilla, Cyberduck, etc.)
2. **Upload all files** to your web root directory (usually `public_html`, `www`, or `htdocs`)
3. **Set permissions**:
   - Directories: 755
   - Files: 644

### Via SSH/Command Line

```bash
# Connect to your server
ssh user@your-server.com

# Navigate to web directory
cd /var/www/html

# Clone repository (or upload files)
git clone https://github.com/YOUR_USERNAME/gigspace-landing.git .

# Set up web server (if needed)
# For Apache, ensure .htaccess is configured
# For Nginx, configure server block
```

### Apache Configuration

Create `.htaccess` file:

```apache
# Enable rewrite engine
RewriteEngine On

# Redirect to HTTPS
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Remove .html extension
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}.html -f
RewriteRule ^(.*)$ $1.html [L]

# Enable compression
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
</IfModule>

# Browser caching
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType image/jpg "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType image/gif "access plus 1 year"
    ExpiresByType image/png "access plus 1 year"
    ExpiresByType text/css "access plus 1 month"
    ExpiresByType application/javascript "access plus 1 month"
</IfModule>
```

### Nginx Configuration

Add to your server block:

```nginx
server {
    listen 80;
    server_name gigspace.com www.gigspace.com;
    
    # Redirect to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name gigspace.com www.gigspace.com;
    
    root /var/www/gigspace-landing;
    index index.html;
    
    # SSL configuration
    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # Gzip compression
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml;
}
```

---

## Post-Deployment Checklist

After deploying to any platform:

- [ ] Test all pages and links
- [ ] Verify contact form works
- [ ] Check mobile responsiveness
- [ ] Test CMS login (if applicable)
- [ ] Configure custom domain and SSL
- [ ] Set up Google Analytics
- [ ] Submit sitemap to Google Search Console
- [ ] Test page load speed
- [ ] Check browser compatibility
- [ ] Verify all images load correctly
- [ ] Test accessibility with screen reader
- [ ] Set up error monitoring (optional)
- [ ] Configure backup strategy

## Continuous Deployment

### With Netlify/Vercel
- Automatically deploys when you push to GitHub
- No manual steps needed after initial setup

### With GitHub Pages
- Automatically deploys on push to main branch

### With Custom Server
Set up automated deployment:

```bash
# Create deploy script
#!/bin/bash
cd /var/www/gigspace-landing
git pull origin main
# Add any build steps if needed
```

Add to cron or use Git hooks for automatic deployment.

---

## Troubleshooting

### Site Not Loading
- Check DNS settings
- Verify files are in correct directory
- Check web server error logs

### CMS Not Working
- Verify Netlify Identity is enabled
- Check Git Gateway configuration
- Ensure OAuth credentials are correct

### Form Submissions Failing
- Verify Formspree endpoint is correct
- Check CORS settings
- Test with browser console open for errors

### Slow Loading
- Optimize images
- Enable compression (gzip/brotli)
- Use CDN for static assets
- Enable browser caching

---

## Support

For deployment issues:
- Check platform documentation (Netlify, Vercel, etc.)
- Review server logs
- Test in browser developer tools
- Contact platform support if needed

For CMS issues:
- Review [Decap CMS documentation](https://decapcms.org/docs/)
- Check Netlify Identity settings
- Verify configuration in `admin/config.yml`

---

**Good luck with your deployment! 🚀**

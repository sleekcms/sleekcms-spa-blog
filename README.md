# One-Page Blog (SleekCMS SPA)

This repository contains a **single-file blog website** powered by **SleekCMS**.  
It loads content using the official `@sleekcms/client`, renders a home page and blog posts, and navigates using **SPA routing** (hash-based or history state) — all without a build step.

The entire site lives in **one HTML file**.

---

## Features

- ✅ Single HTML file (no build, no bundler)
- ✅ Uses `@sleekcms/client` from unpkg
- ✅ Configurable routing: hash-based (`#/`) or history state (`/`)
- ✅ Home page with list of blog posts
- ✅ Blog post page with title, image, and HTML content
- ✅ Works on any static host (Netlify, Vercel, GitHub Pages, S3, etc.)
- ✅ “View source” link for easy inspection

---

## How It Works

### Content Source

Content is fetched directly from **SleekCMS public delivery API** using:

```js
SleekCMS.createSyncClient({
  siteToken: 'your-site-token',
  env: 'latest'
})
```

The client provides:
- `getPage('/')` → home page metadata
- `getPages('/blog')` → all blog posts

No REST calls, no manual JSON parsing.

---

## Content Structure in SleekCMS

### Home Page

| Field | Type |
|------|------|
| title | Text |

Path:
```
/
```

### Blog Posts

| Field | Type |
|------|------|
| title | Text |
| image | Image |
| content | HTML / Rich Text |

Path pattern:
```
/blog/{slug}
```

---

## Setup

### 1. Create a SleekCMS Site
1. Sign up at https://sleekcms.com
2. Create a site
3. Add page models for Home and Blog

### 2. Get Your Site Token
From the SleekCMS dashboard, copy the **Public Site Token**.

### 3. Update the HTML File

Edit the configuration at the top of `index.html`:

```js
const SITE_TOKEN = "your-site-token";
const ENV = "latest";
const SPA = true; // Set to true for history state routing, false for hash routing
```

**Routing Options:**
- `SPA = true`: Uses clean URLs with `history.pushState` (`/`, `/blog/post-slug`)
  - Requires server rewrites or `_redirects` file for production deployment
  - Best for modern hosting platforms (Netlify, Vercel)
- `SPA = false`: Uses hash-based routing (`#/`, `#/blog/post-slug`)
  - Works without server configuration
  - Ideal for simple hosting (GitHub Pages, S3)

### 4. Open or Deploy

Open locally or deploy to any static host.

---

## Routing

This site supports two routing modes controlled by the `SPA` flag:

### History State Routing (`SPA = true`)
- Uses `history.pushState` for clean URLs without hash symbols
- URLs look like: `/`, `/blog/my-post`
- Requires server-side configuration to redirect all requests to `index.html`
- **Deployment requirements:**
  - **Netlify/Vercel:** Include `_redirects` file (already provided)
  - **Apache:** Use `.htaccess` with `RewriteRule`
  - **Nginx:** Configure `try_files $uri /index.html`

### Hash-Based Routing (`SPA = false`)
- Uses URL hash for navigation
- URLs look like: `#/`, `#/blog/my-post`
- No server configuration needed — works anywhere
- **Best for:** GitHub Pages, S3 static hosting, or simple CDN deployments

The included `_redirects` file enables history state routing on Netlify:
```
/*    /index.html   200
```

---

## License

MIT

---

## Learn More

- https://sleekcms.com
- https://www.npmjs.com/package/@sleekcms/client

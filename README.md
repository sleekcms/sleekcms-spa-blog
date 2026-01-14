# SleekCMS SPA Blog

A minimal single-page application (SPA) blog that fetches and renders content directly from [SleekCMS](https://sleekcms.com/) in the browser. No build step, no server required—just open the HTML file or serve it with any static file server.

## Features

- **Client-Side Rendering**: Fetches content directly from SleekCMS using the JavaScript client
- **Zero Build Step**: Single HTML file with no dependencies to install
- **Two Routing Modes**: Hash-based routing (`#/blog/post`) or HTML5 History API (`/blog/post`)
- **Lightweight Styling**: Uses [Pico CSS](https://picocss.com/) for classless, semantic styling
- **Blog-Ready**: Lists posts from `/blog` and renders individual post pages with title, image, and rich content

## Quick Start

1. **Get a SleekCMS Site Token**
   
   Register at [app.sleekcms.com](https://app.sleekcms.com/) and create a site to get your public site token.

2. **Configure the Token**
   
   Open `index.html` and replace the `SITE_TOKEN` value with your own:
   
   ```javascript
   const SITE_TOKEN = "pub-xxxxx-your-token-here";
   ```

3. **Serve the File**
   
   Use any static file server. For example:
   
   ```bash
   npx http-server .
   ```
   
   Then open [http://localhost:8080](http://localhost:8080) in your browser.

## Configuration

Edit the configuration section at the top of the `<script>` block in `index.html`:

| Variable     | Description                                                                 |
|--------------|-----------------------------------------------------------------------------|
| `SITE_TOKEN` | Your SleekCMS public site token (starts with `pub-`)                        |
| `ENV`        | Content environment to fetch (`"latest"`, `"published"`, or a specific env) |
| `SPA`        | Routing mode: `false` = hash routing, `true` = history API routing          |

### Routing Modes

- **Hash Routing (`SPA = false`)**: URLs look like `/#/blog/my-post`. Works with any static file server without additional configuration.

- **History Routing (`SPA = true`)**: URLs look like `/blog/my-post`. Requires server-side configuration to redirect all paths to `index.html`.

## Content Structure

The app expects the following page structure in your SleekCMS site:

```
/                  # Home page (displays site title and blog post list)
/blog/
  /blog/post-1     # Blog post with title, image, and content fields
  /blog/post-2
  /blog/...
```

### Expected Page Fields

| Field     | Type   | Description                        |
|-----------|--------|------------------------------------|
| `title`   | String | Page/post title                    |
| `image`   | Asset  | Featured image (optional)          |
| `content` | HTML   | Rich text content for the post     |

## How It Works

1. **Load SleekCMS Client**: The app loads the SleekCMS client from unpkg CDN
2. **Sync Content**: Uses `createSyncClient()` to fetch and cache all site content
3. **Build Routes**: Creates a path-to-page mapping for client-side routing
4. **Render Views**: Displays home (post list) or individual post based on current URL

### Key Code Example

```javascript
// Initialize the SleekCMS client
const client = await window.SleekCMS.createSyncClient({
  siteToken: SITE_TOKEN,
  env: "latest",
  resolveEnv: true,
});

// Fetch pages
const homePage = client.getPage("/");           // Single page
const blogPages = client.getPages("/blog");     // All pages under /blog
```

## Development

No build tools required! Just edit `index.html` and refresh your browser.

To run locally:

```bash
# Using http-server
npx http-server .

# Using Python
python3 -m http.server 8080

# Using PHP
php -S localhost:8080
```

## License

MIT

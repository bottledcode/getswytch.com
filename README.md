# Swytch Landing Page

Marketing website for [Swytch](https://getswytch.com) - Redis-compatible cache with full durability.

## Prerequisites

Install Hugo (extended version required):

```bash
# macOS
brew install hugo

# Windows
choco install hugo-extended

# Linux (snap)
snap install hugo --channel=extended
```

Verify installation:

```bash
hugo version
# Should show "extended" in the output
```

## Local Development

Start the development server:

```bash
hugo server
```

Visit http://localhost:1313 to see the site. Changes auto-reload.

## Build for Production

Generate the static site:

```bash
hugo --minify
```

Output goes to the `public/` directory.

## Project Structure

```
.
├── hugo.yaml              # Site configuration
├── content/               # Markdown content (if any)
├── layouts/
│   └── partials/          # Template overrides
├── static/
│   ├── css/custom.css     # Custom styles
│   ├── images/            # Images and icons
│   └── favicon.ico
└── public/                # Generated site (gitignored)
```

## Customization

- **Content**: Edit `hugo.yaml` to change headlines, features, and links
- **Styles**: Modify `static/css/custom.css` for styling tweaks
- **Images**: Replace files in `static/images/`

## Deployment

The site deploys to GitHub Pages automatically on push to `main`.

To deploy manually:

```bash
hugo --minify
# Upload contents of public/ to your hosting provider
```

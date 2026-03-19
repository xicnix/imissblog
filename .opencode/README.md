# ImissBlog Repository Summary

## Overview

ImissBlog is a feature-rich Next.js blog template (version 2.4.0) built with modern web technologies. It serves as a comprehensive starting point for creating blogs, combining the power of Next.js with Tailwind CSS and Contentlayer2 for content management. The template is designed to be easily configurable and customizable, making it suitable as a replacement for existing Jekyll and Hugo blogs.

This is a fork/variation of the popular Tailwind Next.js Starter Blog, customized for the author's personal needs.

## Technology Stack

- **Framework**: Next.js 15.2.4 with App Router (React Server Components)
- **UI Library**: React 19.0.0
- **Styling**: Tailwind CSS 4.0.5 with postcss
- **Content Management**: Contentlayer2 0.5.5 with MDX support
- **Blog Toolkit**: Pliny 0.4.1 (analytics, comments, newsletter, search)
- **Typography**: Space Grotesk font from next/font
- **Code Highlighting**: rehype-prism-plus
- **Math Rendering**: KaTeX via rehype-katex
- **Build Tools**: ESBuild 0.25.2

### Key Dependencies

- `@headlessui/react` 2.2.0 (UI components)
- `next-contentlayer2` 0.5.5 (MDX integration)
- `next-themes` 0.4.6 (dark/light mode)
- `remark-gfm`, `remark-math`, `remark-github-blockquote-alert` (MDX plugins)
- `rehype-slug`, `rehype-autolink-headings`, `rehype-citation` (HTML processing)
- `aws-amplify` 6.16.2 (backend integration)

## Repository Structure

```
imissblog/
├── app/                         # Next.js App Router pages
│   ├── blog/                    # Blog posts
│   │   ├── [...slug]/          # Dynamic blog post routes
│   │   ├── page/               # Pagination
│   │   └── page.tsx            # Blog listing page
│   ├── tags/                    # Tag pages
│   │   ├── [tag]/              # Individual tag routes
│   │   └── page.tsx            # All tags listing
│   ├── about/                   # About page
│   ├── projects/                # Projects page
│   ├── layout.tsx               # Root layout with providers
│   ├── page.tsx                 # Homepage
│   ├── seo.tsx                  # SEO components
│   └── theme-providers.tsx      # Theme context provider
├── components/                  # React components
│   ├── Header.tsx               # Navigation header
│   ├── Footer.tsx               # Site footer
│   ├── MobileNav.tsx            # Mobile navigation
│   ├── ThemeSwitch.tsx          # Dark/light mode toggle
│   ├── SearchButton.tsx         # Search functionality
│   ├── Comments.tsx             # Comment system
│   ├── MDXComponents.tsx        # MDX custom components
│   ├── Image.tsx                # Optimized image component
│   ├── Link.tsx                 # Custom link component
│   ├── SectionContainer.tsx     # Layout wrapper
│   ├── PageTitle.tsx            # Page title component
│   ├── TableWrapper.tsx         # Table styling wrapper
│   ├── Tag.tsx                  # Tag display component
│   ├── ScrollTopAndComment.tsx  # Scroll-to-top & comment button
│   └── social-icons/            # Social media icons
├── layouts/                     # Page layouts
│   ├── PostLayout.tsx           # Standard post layout (2 columns)
│   ├── PostSimple.tsx           # Simplified post layout
│   ├── PostBanner.tsx           # Post layout with banner image
│   ├── ListLayout.tsx           # Blog listing with search
│   ├── ListLayoutWithTags.tsx   # Blog listing with tag sidebar
│   └── AuthorLayout.tsx         # Author listing layout
├── data/                        # Content data
│   ├── blog/                    # MDX blog posts
│   │   ├── opencode/           # Custom blog posts
│   │   ├── nestedroute/        # Nested route examples
│   │   └── *.mdx               # Blog files
│   ├── authors/                 # Author information
│   │   ├── default.mdx
│   │   ├── sparrowhawk.mdx
│   │   └── furina.mdx
│   ├── siteMetadata.js          # Site configuration
│   ├── headerNavLinks.ts        # Navigation links
│   ├── projectsData.ts          # Projects display data
│   ├── logo.svg                 # Site logo
│   ├── logo-tailwind.svg        # Tailwind logo variant
│   └── references-data.bib      # Citation database
├── css/                         # Stylesheets
│   ├── tailwind.css             # Tailwind configuration
│   └── prism.css                # Code block styles
├── public/                      # Static assets
│   └── static/                  # Images, favicons, etc.
├── scripts/                     # Build scripts
│   ├── postbuild.mjs            # Post-build processing
│   └── rss.mjs                  # RSS feed generation
├── .opencode/                   # This repository's documentation
├── amplify/                     # AWS Amplify backend configuration
├── faq/                         # Frequently asked questions
├── next.config.js               # Next.js configuration
├── contentlayer.config.ts       # Contentlayer configuration
├── postcss.config.js            # PostCSS configuration
├── tailwind.config.js (in css/) # Tailwind configuration
├── tsconfig.json                # TypeScript configuration
├── eslint.config.mjs            # ESLint configuration
├── prettier.config.js           # Prettier configuration
└── package.json                 # Dependencies and scripts
```

## Key Features

### Content & Markdown Support

- **MDX Support**: Write JSX in markdown documents
- **Frontmatter Support**: YAML frontmatter for metadata (title, date, tags, authors, etc.)
- **Github Alerts**: Support for GitHub-style alert blocks
- **Math Typesetting**: KaTeX support for mathematical equations
- **Citation Support**: Bibliography and citation management via rehype-citation
- **Code Highlighting**: Line numbers and line highlighting via rehype-prism-plus

### User Experience

- **Light/Dark Mode**: Built-in theme switching with next-themes
- **Responsive Design**: Mobile-friendly layout with Tailwind CSS 4
- **Search**: Command palette search (kbar) orAlgolia integration
- **Newsletter**: Multiple providers supported (Mailchimp, Buttondown, Convertkit, Klaviyo, etc.)
- **Comments**: Giscus, Utterances, or Disqus integration

### Performance & Optimization

- **Server Components**: Next.js App Router with React Server Components
- **Image Optimization**: next/image for automatic image optimization
- **Font Optimization**: next/font for self-hosted fonts
- **Code Splitting**: Automatic code splitting and lazy loading
- **Lighthouse Score**: Near-perfect performance scores

### SEO &-meta

- **Automatic Sitemaps**:动生成 sitemap.xml
- **RSS Feed**: Generated feed.xml
- **Open Graph Tags**: Automatic meta tags for social sharing
- **Twitter Cards**: Twitter card support
- **Robots.txt**: Automatic generation
- **Canonical URLs**: Support for canonical URLs

### Layout Flexibility

- **3 Post Layouts**: PostLayout (2-column), PostSimple (simplified), PostBanner (with banner image)
- **2 Listing Layouts**: ListLayout (with search), ListLayoutWithTags (with tag sidebar)
- **Nested Routing**: Support for multi-part posts via nested routes
- **Customizable Navigation**: Configurable header links

### Analytics & Engagement

- **Analytics**: Umami, Plausible, Simple Analytics, Posthog, Google Analytics
- **Comments**: Discussion integration via Giscus, Utterances, or Disqus
- **Newsletter**: Email subscription forms

## Configuration Files

### `next.config.js`

Main Next.js configuration with:

- Contentlayer integration
- Bundle analyzer enabled via ANALYZE env variable
- Security headers (CSP, HSTS, X-Frame-Options, etc.)
- Image optimization configuration
- ESLint configuration
- Webpack extensions for SVG handling

### `contentlayer.config.ts`

Contentlayer configuration with:

- Content sources (blog, authors)
- MDX plugins (remarkGfm, remarkMath, remarkAlert, etc.)
- Rehype plugins (slug, autolink-headings, Katex, Prism, citation, minify)
- Computed fields (readingTime, slug, path, toc)
- Post-build hooks (tag counting, search index generation)

### `data/siteMetadata.js`

Site-wide metadata including:

- Site URL, title, description
- Author information
- Social media links
- Analytics configuration (Umami, Plausible, etc.)
- Newsletter provider settings
- Comment system configuration (Giscus, Utterances, Disqus)
- Search configuration (kbar, Algolia)

### `data/headerNavLinks.ts`

Navigation link configuration for the header.

### `components/MDXComponents.tsx`

Custom MDX components including:

- TOCInline (table of contents)
- Pre (code blocks)
- BlogNewsletterForm (newsletter form)
- Custom Image and Link components

### `tailwind.config.js` / `css/tailwind.css`

Tailwind CSS 4 configuration with:

- Custom primary color theme
- Typography plugin
- Custom utilities and component classes
- Dark mode support

## Available Scripts

```json
{
  "start": "next dev", // Alias for dev
  "dev": "INIT_CWD=$PWD next dev", // Start development server
  "build": "next build && node ./scripts/postbuild.mjs", // Production build
  "serve": "next start", // Start production server
  "analyze": "ANALYZE=true next build", // Build with bundle analysis
  "lint": "next lint --fix --dir ...", // Run ESLint
  "prepare": "husky" // Setup husky hooks
}
```

### Build Variations

**Static export** (for GitHub Pages, S3, etc.):

```bash
EXPORT=1 UNOPTIMIZED=1 yarn build
```

**With base path** (for subdirectory deployments):

```bash
EXPORT=1 UNOPTIMIZED=1 BASE_PATH=/myblog yarn build
```

## Deployment Options

### Vercel (Recommended)

- One-click deploy via the Deploy button
- Automatic caching and optimization
- Support for environment variables
- [Documentation](https://vercel.com/docs)

### GitHub Pages

- Workflow file: `.github/workflows/pages.yml`
- Configure in `Settings > Pages > Build and deployment > Source`
- Use static export for GitHub Pages compatibility

### Netlify

- Next.js runtime automaticallyconfigures serverless functions
- Supports SSR, ISR, and image optimization
- [Next.js on Netlify documentation](https://docs.netlify.com/integrations/frameworks/next-js/)

### Static Hosting (GitHub Pages, S3, Firebase, etc.)

1. Run: `EXPORT=1 UNOPTIMIZED=1 yarn build`
2. Deploy the generated `out/` directory
3. For subdirectory deployments: `BASE_PATH=/myblog EXPORT=1 yarn build`

### Docker

- See `faq/deploy-with-docker.md` for Docker deployment instructions

## Environment Variables

Required environment variables (copy `.env.example` to `.env`):

```bash
# Analytics
NEXT_UMAMI_ID=                                  # Umami analytics website ID

# Comments (Giscus)
NEXT_PUBLIC_GISCUS_REPO=
NEXT_PUBLIC_GISCUS_REPOSITORY_ID=
NEXT_PUBLIC_GISCUS_CATEGORY=
NEXT_PUBLIC_GISCUS_CATEGORY_ID=

# Search (Optional)
NEXT_PUBLIC_ALGOLIA_APP_ID=                     # Algolia app ID
NEXT_PUBLIC_ALGOLIA_API_KEY=                    # Algolia public API key
```

## License

[MIT License](https://github.com/xicnix/imissblog/blob/main/LICENSE) © 2021-2025 Timothy Lin

The original template is created by Timothy Lin. This version has been customized by Xichen Ni.

## Attribution

This repository is a modified version of [tailwind-nextjs-starter-blog](https://github.com/timlrx/tailwind-nextjs-starter-blog), a popular Next.js blog template. The original project provides the foundation for this customized implementation.

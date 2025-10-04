# Fuwari  
![Node.js >= 20](https://img.shields.io/badge/node.js-%3E%3D20-brightgreen) 
![pnpm >= 9](https://img.shields.io/badge/pnpm-%3E%3D9-blue) 
![Astro](https://img.shields.io/badge/Astro-5.12.8-orange)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9.2-blue)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A modern, feature-rich static blog template built with [Astro](https://astro.build), featuring advanced functionality and beautiful design.

🌏 README Languages
[**中文**](README.md) /
[**English**](README.en.md) /

## 🚀 Quick Start

### 📦 Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/mikuas/fuwari.git
   cd fuwari
   ```

2. **Install dependencies:**
   ```bash
   # Install pnpm if not already installed
   npm install -g pnpm
   
   # Install project dependencies
   pnpm install
   ```

3. **Configure your blog:**
   - Edit `src/config.ts` to customize blog settings
   - Update site information, theme colors, banner images, and social links
   - Configure translation settings and feature page functionality

4. **Start the development server:**
   ```bash
   pnpm dev
   ```
   Your blog will be available at `http://localhost:4321`

### 📝 Content Management

- **Create new posts:** `pnpm new-post <filename>`
- **Edit posts:** Modify files in `src/content/posts/`
- **Customize special pages:** Edit files in `src/content/spec/`
- **Add images:** Place images in `src/assets/` or `public/`

### 🚀 Deployment

Deploy your blog to any static hosting platform:

- **Vercel:** Connect your GitHub repository to Vercel
- **Netlify:** Deploy directly from GitHub
- **GitHub Pages:** Use the included GitHub Actions workflow
- **Cloudflare Pages:** Connect your repository

Before deployment, update the `site` URL in `astro.config.mjs`.

## 📝 Post Frontmatter Format

```yaml
---
title: My First Blog Post
published: 2023-09-09
description: This is the first post of my new blog.
image: ./cover.jpg
tags: [tag1, tag2]
category: Frontend
draft: false
pinned: false
lang: en      # Only set when article language differs from site language in config.ts
---
```

### Frontmatter Field Descriptions

- **title**: Article title (required)
- **published**: Publication date (required)
- **description**: Article description for SEO and previews
- **image**: Cover image path (relative to article file)
- **tags**: Array of tags for categorization
- **category**: Article category
- **draft**: Set to `true` to hide article in production
- **pinned**: Set to `true` to pin article to top
- **lang**: Article language (only set when different from site default)

### Pinned Articles Feature

The `pinned` field allows you to pin important articles to the top of your blog list. Pinned articles will always appear before regular articles regardless of their publication date.

**Usage:**
```yaml
pinned: true  # Pin this article to the top
pinned: false # Regular article (default)
```

**Sorting Rules:**
1. Pinned articles appear first, sorted by publication date (newest first)
2. Regular articles follow, sorted by publication date (newest first)

## 🧩 Markdown Extensions

Mizuki supports enhanced features beyond standard GitHub Flavored Markdown:

### 📝 Enhanced Writing
- **Callouts:** Create beautiful annotation boxes using `> [!NOTE]`, `> [!TIP]`, `> [!WARNING]`, etc.
- **Math Formulas:** Write LaTeX math formulas using `$inline$` and `$$block$$` syntax
- **Code Highlighting:** Advanced syntax highlighting with line numbers and copy buttons
- **GitHub Cards:** Embed repository cards using `::github{repo="user/repo"}`

### 🎨 Visual Elements
- **Image Gallery:** Automatic PhotoSwipe integration for image viewing
- **Collapsible Sections:** Create expandable content blocks
- **Custom Components:** Enhance content with special directives

### 📊 Content Organization
- **Table of Contents:** Automatically generated from headings with smooth scrolling
- **Reading Time:** Automatically calculated and displayed
- **Article Metadata:** Rich frontmatter support with categories and tags

## ⚡ Commands

All commands are run from the project root:

| Command                    | Action                                   |
|:---------------------------|:-----------------------------------------|
| `pnpm install`             | Install dependencies                     |
| `pnpm dev`                 | Start local dev server at `localhost:4321` |
| `pnpm build`               | Build production site to `./dist/`       |
| `pnpm preview`             | Preview build locally before deployment  |
| `pnpm check`               | Run Astro error checking                 |
| `pnpm format`              | Format code with Biome                   |
| `pnpm lint`                | Check and fix code issues                |
| `pnpm new-post <filename>` | Create a new blog post                   |
| `pnpm astro ...`           | Run Astro CLI commands                   |

## 🎯 Configuration Guide

### 🔧 Basic Configuration

Edit `src/config.ts` to customize your blog:

```typescript
export const siteConfig: SiteConfig = {
  title: "Your Blog Name",
  subtitle: "Your Blog Description",
  lang: "en", // or "zh-CN", "ja", etc.
  themeColor: {
    hue: 210, // 0-360, theme hue
    fixed: false, // Hide theme color picker
  },
  translate: {
    enable: true, // Enable translation
    service: "client.edge", // Translation service
    defaultLanguage: "english",
  },
  banner: {
    enable: true,
    src: ["assets/banner/1.webp"], // Banner images
    carousel: {
      enable: true,
      interval: 0.8, // seconds
    },
  },
};
```

### 📱 Feature Page Configuration

- **Anime Page:** Edit anime list in `src/pages/anime.astro`
- **Friends Page:** Edit friend data in `src/content/spec/friends.md`
- **Diary Page:** Edit moments in `src/pages/diary.astro`
- **About Page:** Edit content in `src/content/spec/about.md`

## ✏️ Contributing

Contributions are welcome! Feel free to submit issues and pull requests.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
[Mizuki](https://github.com/matsuzaka-yuki/mizuki)

---

⭐ If you find this project helpful, please consider giving it a star!
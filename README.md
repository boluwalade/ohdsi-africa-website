# OHDSI Africa Website - SvelteKit Version

A modern, responsive website for the OHDSI Africa Chapter built with SvelteKit and TypeScript.

## Features

- 🎨 **Modern Design** - Clean, professional design
- 📱 **Responsive** - Works perfectly on all devices
- ⚡ **Fast** - Built with SvelteKit for optimal performance
- 🎯 **TypeScript** - Type-safe development
- 🧩 **Component-based** - Reusable Svelte components
- 🔧 **Easy to maintain** - Clean code structure and documentation

## Project Structure

```
src/
├── lib/
│   └── components/          # Reusable Svelte components
│       ├── Header.svelte
│       ├── Footer.svelte
│       ├── PageHeader.svelte
│       ├── AnnouncementBanner.svelte
│       ├── HeroSection.svelte
│       └── ContactForm.svelte
├── routes/                  # Page routes
│   ├── +layout.svelte       # Main layout
│   ├── +page.svelte         # Home page
│   ├── about/
│   │   └── +page.svelte     # About page
│   ├── community/
│   │   └── +page.svelte     # Community page
│   └── contact/
│       └── +page.svelte     # Contact page
├── app.css                  # Global styles
└── app.html                 # HTML template
```

## Development

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Start development server:**
   ```bash
   npm run dev
   ```

3. **Open in browser:**
   Navigate to `http://localhost:5173`

## Building for Production

```bash
npm run build
```

The built files will be in the `build/` directory.

## Pages Included

- ✅ **Home** - Hero section with announcement banner
- ✅ **About** - Mission, leadership, and global context
- ✅ **Community** - National nodes and community engagement
- ✅ **Contact** - Contact forms and information
- 🚧 **Blog** - Past events and news (to be added)
- 🚧 **Events** - Africa Symposium and other events (to be added)

## Deployment

This SvelteKit app can be deployed to:

- **Vercel** (recommended)
- **Netlify** 
- **Cloudflare Pages**
- **Static hosting** (with adapter-static)

## Technologies Used

- **SvelteKit** - Full-stack framework
- **TypeScript** - Type safety
- **CSS Custom Properties** - Consistent styling
- **Font Awesome** - Icons
- **Inter Font** - Typography

## Original HTML Version

The original HTML version is available in the parent directory and has been fully converted to this modern SvelteKit version with component-based architecture.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is part of the OHDSI collaborative network and follows OHDSI's open-source principles.

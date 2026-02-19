# Agent Development Guidelines

This file provides essential information for agentic coding agents working in this Astro project.

## Build Commands

```bash
# Development
npm run dev              # Start dev server on localhost:4321
npm start               # Alias for dev

# Production
npm run build           # Build for production
npm run preview         # Preview production build locally

# Linting & Formatting
npm run lint             # Run ESLint
npm run lint:fix         # Auto-fix ESLint errors
npm run format           # Format code with Prettier
npm run typecheck        # Run Astro type checking

# Utility Scripts
npm run remove-decap     # Remove Decap CMS integration
npm run remove-demo      # Remove demo content (keeps blog/CMS)
npm run remove-dark-mode # Remove dark mode functionality
```

**Note:** No test framework is currently configured. Add testing setup before running tests.

## Project Structure

```
src/
├── components/          # Component-per-folder pattern
├── layouts/           # BaseLayout, BlogPostLayout, etc.
├── pages/             # Astro pages (file-based routing)
├── styles/            # LESS stylesheets
├── js/                # Utility functions and client scripts
├── data/              # Site configuration (client.ts, navData.json)
├── content/           # Content Collections (blog posts)
├── assets/            # Optimized images (use with <Picture />)
└── icons/             # SVG icons for astro-icon
```

## Code Style Guidelines

### Imports

Use path aliases defined in tsconfig.json:

```typescript
import ComponentName from '@components/ComponentName/ComponentName.astro';
import { SITE } from '@data/client';
import '@styles/root.less';
import { helperFn } from '@js/utils.js';
```

**Import Order:** Utils → Components → Data/Types

### TypeScript

- **strictNullChecks: true** enabled
- Define Props interfaces explicitly:

```typescript
interface Props {
  title: string;
  description: string;
  heroImage?: HeroImage;
}
```

- Use union types for images: `type HeroImage = GetImageResult | ImageMetadata;`

### Naming Conventions

- **Components:** PascalCase (`DynamicHeader`, `BaseLayout`)
- **Functions:** camelCase (`formatDate`, `getDropdownId`)
- **CSS Classes:** kebab-case, `cs-` prefix (`cs-button-solid`, `cs-title`)
- **Constants:** UPPER_SNAKE_CASE (`SITE`, `BUSINESS`, `SEO`)
- **Files:** PascalCase for components, camelCase for utilities

### Component Structure

**Component-per-folder pattern:**

```
src/components/
└── ComponentName/
    └── ComponentName.astro
```

**Astro component template:**

```astro
---
// Imports at top
// Props interface
// Destructure Astro.props
---

<!-- HTML content -->
<style lang="less">
  // Scoped styles
</style>
```

### Styling (LESS)

- Use **CSS variables** for design tokens (defined in `src/styles/root.less`)
- Mobile-first responsive design
- Nested selectors with `&`:

```less
.button {
  &:hover {
    color: #fff;
  }
}
```

- Utility classes prefixed with `cs-`

### JavaScript/TypeScript Utilities

- ES6+ syntax with arrow functions
- JSDoc comments for exported functions:

```javascript
/**
 * Formats date to readable string
 * @param {Date|string} date - Date to format
 * @returns {string} Formatted date string
 */
export function formatDate(date) {
  // implementation
}
```

### Accessibility

- Use `aria-label`, `aria-current`, `aria-expanded` attributes
- Include skip-to-main link in BaseLayout
- Ensure keyboard navigation works for all interactive elements

### View Transitions

Wrap scripts with `astro:page-load` event:

```javascript
document.addEventListener('astro:page-load', () => {
  // Runs on initial load and after navigation
  setupEventListeners();
});
```

### Images

**Images must be in `src/assets/` for optimization:**

```astro
---
import { Picture } from 'astro:assets';
import image from '@assets/images/photo.jpg';
---

<Picture src={image} alt="Description" formats={['avif', 'webp']} width={400} />
```

- Public images in `public/assets/` are NOT optimized
- Use `getImage()` from `astro:assets` for preloading

### Data Configuration

- **Site config:** `src/data/client.ts` (business info, SEO defaults)
- **Navigation:** `src/data/navData.json` (data-driven nav)
- **CMS config:** `public/admin/config.yml`

### Error Handling

- Minimal explicit error handling in this codebase
- Rely on Astro's built-in error handling
- TypeScript prevents many runtime errors via type checking

### Dark Mode

- Toggle via `dark-mode` class on `<body>`
- Persist with `localStorage.setItem('theme', 'dark'|'light')`
- Styles in `src/styles/dark.less`

## Important Notes

- **No testing framework** configured - add before writing tests
- **Linting configured:** ESLint + Prettier for Astro + TypeScript
  - Run `npm run lint` to check for errors
  - Run `npm run typecheck` for TypeScript type errors
- Use `await` for image optimization with `getImage()`
- Import paths use aliases, not relative paths
- All blog content validated via Content Collections in `src/content.config.ts`
- Decap CMS integration for client-managed content

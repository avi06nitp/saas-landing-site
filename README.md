# Atomic Labs - SaaS Landing Page

<div align="center">

![Next.js](https://img.shields.io/badge/Next.js-14.2.4-black?style=for-the-badge&logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue?style=for-the-badge&logo=typescript)
![TailwindCSS](https://img.shields.io/badge/Tailwind-3.4.1-38bdf8?style=for-the-badge&logo=tailwind-css)
![Framer Motion](https://img.shields.io/badge/Framer_Motion-11.2.14-ff0055?style=for-the-badge&logo=framer)

A modern, high-performance SaaS landing page built with Next.js 14, featuring advanced animations, responsive design, and optimized asset handling.

[View Demo](#) • [Report Bug](#) • [Request Feature](#)

</div>

---

## Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Technical Architecture](#-technical-architecture)
- [Project Structure](#-project-structure)
- [Critical Implementations](#-critical-implementations)
- [Component Breakdown](#-component-breakdown)
- [Styling Architecture](#-styling-architecture)
- [Animation System](#-animation-system)
- [Performance Optimizations](#-performance-optimizations)
- [Development Workflow](#-development-workflow)
- [Deployment](#-deployment)
- [Environment & Configuration](#-environment--configuration)
- [Browser Support](#-browser-support)
- [Contributing](#-contributing)
- [License](#-license)

---

## Overview

**Atomic Labs Landing Page** is a production-ready SaaS marketing website designed to showcase product features, pricing tiers, customer testimonials, and brand partnerships. Built with modern web technologies, it emphasizes performance, accessibility, and developer experience.

### Project Metadata

| Property | Value |
|----------|-------|
| **Framework** | Next.js 14.2.4 (App Router) |
| **Language** | TypeScript 5.x |
| **Styling** | TailwindCSS 3.4.1 + Custom CSS Modules |
| **Animations** | Framer Motion 11.2.14 |
| **Deployment** | Vercel (optimized configuration) |
| **Font System** | Next.js Font Optimization (DM Sans) |
| **Asset Management** | Next.js Image Optimization + Custom SVG Loader |

---

## Key Features

### Design & UX
- **Responsive Design**: Mobile-first approach with breakpoints at 375px, 768px, and 1200px
- **Modern UI/UX**: Gradient backgrounds, glassmorphism effects, and smooth transitions
- **Interactive Elements**: Hover states, animated CTAs, and scroll-based effects
- **Accessibility**: Semantic HTML, proper ARIA labels, and keyboard navigation support

### Performance
- **Server-Side Rendering (SSR)**: Next.js App Router for optimal SEO and performance
- **Image Optimization**: Automatic WebP conversion, lazy loading, and responsive images
- **Code Splitting**: Automatic route-based code splitting for faster page loads
- **Custom SVG Loading**: SVGR integration for component-based SVG usage

### Animations
- **Scroll-Driven Animations**: Parallax effects using Framer Motion's `useScroll` and `useTransform`
- **Infinite Loops**: Logo ticker and testimonial carousels with seamless looping
- **Micro-Interactions**: Button hover effects, animated gradients, and entrance animations

### Developer Experience
- **TypeScript**: Full type safety across the entire codebase
- **Component Architecture**: Modular, reusable section-based components
- **Custom Utilities**: TailwindCSS utility classes and component patterns
- **ESLint Configuration**: Code quality enforcement and consistent formatting

---

## Technical Architecture

### Frontend Stack

```
┌─────────────────────────────────────────────┐
│           Next.js 14 App Router             │
│         (React 18 + Server Components)      │
├─────────────────────────────────────────────┤
│  TypeScript Layer                           │
│  - Type-safe components                     │
│  - Interface definitions                    │
│  - Enhanced IDE support                     │
├─────────────────────────────────────────────┤
│  Styling Layer                              │
│  - TailwindCSS (utility-first)              │
│  - Custom CSS components                    │
│  - clsx + tailwind-merge                    │
├─────────────────────────────────────────────┤
│  Animation Layer                            │
│  - Framer Motion                            │
│  - Scroll-based animations                  │
│  - Spring physics                           │
├─────────────────────────────────────────────┤
│  Asset Management                           │
│  - Next.js Image (optimization)             │
│  - SVGR (SVG components)                    │
│  - Static asset pipeline                    │
└─────────────────────────────────────────────┘
```

### Rendering Strategy

- **Static Generation**: Default for all pages (optimal performance)
- **Client-Side Interactivity**: Selective hydration for animated components
- **Font Optimization**: Google Fonts loaded via Next.js font system
- **CSS-in-JS**: Zero-runtime CSS with TailwindCSS

---

##  Project Structure

```
saas-landing-site/
├── src/
│   ├── app/
│   │   ├── layout.tsx          # Root layout with metadata & font config
│   │   ├── page.tsx            # Home page composition
│   │   └── globals.css         # Global styles & Tailwind directives
│   │
│   ├── sections/               # Page section components
│   │   ├── Header.tsx          # Sticky navigation header
│   │   ├── Hero.tsx            # Hero section with parallax
│   │   ├── LogoTicker.tsx      # Infinite logo carousel
│   │   ├── ProductShowcase.tsx # Product feature showcase
│   │   ├── Pricing.tsx         # Pricing tiers with animations
│   │   ├── Testimonials.tsx    # Customer testimonial columns
│   │   ├── CallToAction.tsx    # Final conversion section
│   │   └── Footer.tsx          # Site footer with links
│   │
│   └── assets/                 # Static assets
│       ├── *.png               # Raster images (avatars, logos, products)
│       └── *.svg               # Vector graphics (icons, social)
│
├── public/                     # Public static files
│
├── next.config.mjs             # Next.js configuration (SVGR setup)
├── tailwind.config.ts          # Tailwind theme & utilities
├── tsconfig.json               # TypeScript compiler options
├── vercel.json                 # Vercel deployment config
├── package.json                # Dependencies & scripts
└── .eslintrc.json              # ESLint rules
```

---

## Critical Implementations

### 1. Custom SVG Component Loader

**File**: `next.config.mjs`

```javascript
// Custom webpack configuration for SVG handling
webpack(config) {
  const fileLoaderRule = config.module.rules.find((rule) =>
    rule.test?.test?.(".svg")
  );

  config.module.rules.push(
    // SVG imports ending in ?url remain as files
    {
      ...fileLoaderRule,
      test: /\.svg$/i,
      resourceQuery: /url/,
    },
    // All other SVGs become React components
    {
      test: /\.svg$/i,
      issuer: fileLoaderRule.issuer,
      resourceQuery: { not: [...fileLoaderRule.resourceQuery.not, /url/] },
      use: ["@svgr/webpack"],
    }
  );

  fileLoaderRule.exclude = /\.svg$/i;
  return config;
}
```

**Benefits**:
- SVG files imported as React components (tree-shakable)
- Inline SVG manipulation via props (className, fill, etc.)
- Reduced bundle size through code splitting
- Improved performance over base64 encoding

**Usage Example**:
```tsx
import ArrowIcon from "@/assets/arrow-right.svg";
<ArrowIcon className="h-5 w-5 text-white" />
```

---

### 2. Font Optimization with Next.js

**File**: `src/app/layout.tsx:2`

```typescript
import { DM_Sans } from "next/font/google";

const dmSans = DM_Sans({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Atomic Labs Landing Page",
  description: "This is my first Frontend Project",
};
```

**Implementation Details**:
- **Automatic Subsetting**: Only Latin characters loaded (reduced font size)
- **Self-Hosting**: Fonts served from same origin (privacy + performance)
- **Preloading**: Critical font files preloaded in `<head>`
- **FOIT Prevention**: Font display strategy prevents invisible text

**Performance Impact**:
- Eliminates external font requests (Google Fonts CDN)
- Reduces layout shift (CLS)
- Improves First Contentful Paint (FCP)

---

### 3. Scroll-Based Parallax System

**File**: `src/sections/Hero.tsx:16-21`

```typescript
const heroRef = useRef(null);
const { scrollYProgress } = useScroll({
  target: heroRef,
  offset: ["start end", "end start"],
});
const translateY = useTransform(scrollYProgress, [0, 1], [150, -150]);
```

**Technical Breakdown**:
- **useScroll**: Tracks scroll position relative to target element
- **offset**: Defines animation start ("start end") and end ("end start") points
- **useTransform**: Maps scroll progress (0-1) to Y-axis translation (150px to -150px)
- **Hardware Acceleration**: Uses CSS transforms for 60fps animations

**Applied To**:
- `Hero.tsx:70`: Cylinder image parallax
- `Hero.tsx:80`: Noodle image parallax
- `ProductShowcase.tsx:43`: Pyramid image parallax
- `CallToAction.tsx:35`: Star/spring image parallax

---

### 4. Infinite Loop Animation Pattern

**File**: `src/sections/LogoTicker.tsx:16-26`

```typescript
<motion.div
  className="flex gap-14 flex-none pr-14"
  animate={{ translateX: "-50%" }}
  transition={{
    duration: 20,
    repeat: Infinity,
    ease: "linear",
    repeatType: "loop",
  }}
>
  {/* Logos rendered twice for seamless loop */}
</motion.div>
```

**Pattern Explanation**:
1. **Duplication**: Logos array rendered twice (lines 28-89)
2. **Translation**: Container translates by exactly 50% of its width
3. **Seamless Transition**: When first set completes, second set starts at same position
4. **Linear Easing**: Constant speed for smooth scrolling effect

**Performance Considerations**:
- CSS transforms (GPU-accelerated)
- No layout recalculation during animation
- `will-change: transform` implicitly applied by Framer Motion

---

### 5. Responsive Pricing Card System

**File**: `src/sections/Pricing.tsx:6-56`

```typescript
const pricingTiers = [
  { title: "Free", monthlyPrice: 0, inverse: false, popular: false, ... },
  { title: "Pro", monthlyPrice: 9, inverse: true, popular: true, ... },
  { title: "Business", monthlyPrice: 19, inverse: false, popular: false, ... }
];
```

**Dynamic Styling Logic**:
```typescript
<div className={twMerge(
  "card",
  inverse === true && "border-black bg-black text-white"
)}>
```

**Features**:
- **Conditional Styling**: `inverse` prop inverts colors for emphasis
- **Popular Badge**: Animated gradient text using CSS `background-clip`
- **Responsive Grid**: Flexbox layout adapts to screen size (column → row)
- **Feature Lists**: Dynamic rendering of feature arrays

**Animation**: Popular badge uses continuous background position animation (lines 82-90)

---

### 6. Testimonial Carousel Architecture

**File**: `src/sections/Testimonials.tsx:77-121`

```typescript
const TestimonialsColumn = (props: {
  className?: string;
  testimonials: typeof testimonials;
  duration?: number;
}) => (
  <div className={props.className}>
    <motion.div
      animate={{ translateY: "-50%" }}
      transition={{
        duration: props.duration || 10,
        repeat: Infinity,
        ease: "linear",
        repeatType: "loop",
      }}
    >
      {[...new Array(2)].fill(0).map((_, index) => (
        <React.Fragment key={index}>
          {props.testimonials.map(({ text, imageSrc, name, username }) => (
            <div className="card">...</div>
          ))}
        </React.Fragment>
      ))}
    </motion.div>
  </div>
);
```

**Component Composition**:
- **Three Columns**: `firstColumn`, `secondColumn`, `thirdColumn` (lines 73-75)
- **Staggered Durations**: 15s, 19s, 17s for visual variety
- **Responsive Visibility**: Hidden on mobile/tablet via `className`
- **Mask Effect**: Gradient mask fades top/bottom edges (line 137)

**Data Structure**:
- 9 testimonials split into 3 columns (3 cards each)
- Each card contains text, avatar, name, username

---

### 7. Sticky Header with Responsive Navigation

**File**: `src/sections/Header.tsx:5-34`

```typescript
<header className="sticky top-0 z-20">
  <div className="flex justify-center items-center py-3 bg-black text-white">
    {/* Announcement banner */}
  </div>
  <div className="py-5">
    <div className="container">
      <div className="flex items-center justify-between rounded-full bg-white">
        <Image src={Logo} alt="Saas Logo" height={40} width={40} />
        <MenuIcon className="h-5 w-5 md:hidden" />
        <nav className="hidden md:flex gap-6">
          {/* Navigation links */}
        </nav>
      </div>
    </div>
  </div>
</header>
```

**Layout Strategy**:
- **Sticky Positioning**: Header remains visible during scroll
- **Z-Index Management**: `z-20` ensures header stays above content
- **Responsive Menu**: Hamburger icon on mobile, full nav on desktop
- **Glassmorphism**: Gradient background with shadow and border radius

---

### 8. TailwindCSS Component Layer

**File**: `src/app/globals.css:5-41`

```css
@layer components {
  .btn {
    @apply px-4 py-2 rounded-lg font-medium inline-flex items-center justify-center tracking-tight;
  }

  .btn-primary {
    @apply bg-black text-white;
  }

  .section-title {
    @apply text-center text-3xl md:text-[54px] md:leading-[60px] font-bold tracking-tighter bg-gradient-to-b from-black to-[#001E80] text-transparent bg-clip-text;
  }

  .card {
    @apply p-10 border border-[#222222]/10 rounded-3xl shadow-[0_7px_14px_#EAEAEA] max-w-xs w-full;
  }
}
```

**Component Classes**:
| Class | Purpose | Usage |
|-------|---------|-------|
| `.btn` | Base button styles | All buttons |
| `.btn-primary` | Primary action button | CTAs |
| `.btn-text` | Text-only button | Secondary actions |
| `.tag` | Pill-shaped label | Section badges |
| `.section-title` | Gradient heading | Section headers |
| `.section-description` | Body text for sections | Descriptions |
| `.card` | Card container | Pricing, testimonials |

---

### 9. Vercel Deployment Configuration

**File**: `vercel.json`

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/" }
  ]
}
```

**Purpose**:
- **SPA Fallback**: All routes redirect to index (single-page app behavior)
- **Client-Side Routing**: Ensures Next.js router handles navigation
- **404 Prevention**: Prevents 404 errors on direct URL access

---

### 10. Custom Container System

**File**: `tailwind.config.ts:17-23`

```typescript
container: {
  center: true,
  padding: {
    DEFAULT: "20px",
    lg: "80px",
  },
}
```

**Responsive Padding**:
- **Mobile/Tablet**: 20px horizontal padding
- **Desktop (≥1200px)**: 80px horizontal padding
- **Auto-Centering**: Horizontal centering across all breakpoints

**Usage**: Applied to all section containers for consistent spacing

---

##  Component Breakdown

### Section Components

| Component | Lines | Description | Key Features |
|-----------|-------|-------------|--------------|
| **Header** | 35 | Sticky navigation header | Responsive menu, announcement banner, glassmorphism |
| **Hero** | 89 | Main hero section | Parallax animations, gradient text, floating elements |
| **LogoTicker** | 96 | Partner logo carousel | Infinite scroll, mask gradient, duplicated content |
| **ProductShowcase** | 61 | Product feature display | Parallax decorations, centered image, gradient background |
| **Pricing** | 119 | Pricing tiers | Dynamic styling, animated badge, feature lists |
| **Testimonials** | 154 | Customer testimonials | Three-column layout, infinite scroll, staggered timing |
| **CallToAction** | 59 | Final conversion section | Parallax decorations, dual CTA buttons |
| **Footer** | 38 | Site footer | Social links, navigation, gradient logo effect |

### Component Dependencies

```
page.tsx (Home)
├── Header
├── Hero
│   └── Assets: cog.png, cylinder.png, noodle.png
├── LogoTicker
│   └── Assets: 6 logo PNGs
├── ProductShowcase
│   └── Assets: product-image.png, pyramid.png, tube.png
├── Pricing
│   └── Assets: check.svg
├── Testimonials
│   └── Assets: avatar-1 through avatar-9.png
├── CallToAction
│   └── Assets: star.png, spring.png
└── Footer
    └── Assets: logosaas.png, 5 social SVGs
```

---

##  Styling Architecture

### Color Palette

```typescript
// Primary Colors
const colors = {
  primary: "#183EC2",      // Blue gradient start
  secondary: "#001E80",    // Blue gradient end
  background: "#EAEEFE",   // Light blue background
  card: "#FFFFFF",         // White cards
  text: "#010D3E",         // Dark text
  border: "#222222/10",    // Subtle borders
};
```

### Typography System

```typescript
// Font Stack
font-family: 'DM Sans', sans-serif;

// Heading Scales
h1: 5xl (48px) → 7xl (72px)    // Hero title
h2: 3xl (30px) → 54px          // Section titles
h3: lg (18px)                   // Card titles
body: base (16px) → xl (20px)  // Body text
small: sm (14px)                // Labels, captions
```

### Gradient System

1. **Text Gradients**: `from-black to-[#001E80]` (section titles)
2. **Background Gradients**: `from-white to-[#D2DCFF]` (sections)
3. **Radial Gradients**: `radial-gradient(ellipse_200%_100%_at_bottom_left,#183EC2,#EAEEFE_100%)` (hero)
4. **Animated Gradients**: Multi-color horizontal gradients (popular badge)

### Responsive Breakpoints

```typescript
// tailwind.config.ts
screens: {
  sm: "375px",   // Mobile
  md: "768px",   // Tablet
  lg: "1200px",  // Desktop
}
```

---

##  Animation System

### Animation Types

1. **Scroll-Triggered Parallax**
   - Implementation: `useScroll` + `useTransform`
   - Elements: Hero decorations, showcase decorations, CTA decorations
   - Performance: Hardware-accelerated CSS transforms

2. **Infinite Loops**
   - Implementation: `animate` + `repeat: Infinity`
   - Elements: Logo ticker, testimonial columns
   - Technique: Duplication + 50% translation

3. **Hover Interactions**
   - Implementation: Native CSS transitions
   - Elements: Buttons, navigation links, cards
   - Duration: 150ms-300ms

4. **Entrance Animations**
   - Implementation: Framer Motion `animate` prop
   - Elements: Hero images (floating effect)
   - Pattern: Continuous Y-axis oscillation

### Performance Optimization

- **GPU Acceleration**: All animations use `transform` and `opacity`
- **No Layout Thrashing**: Zero width/height animations
- **RequestAnimationFrame**: Framer Motion uses RAF internally
- **Lazy Loading**: Images below fold defer loading

---

## Performance Optimizations

### Image Optimization

**Next.js Image Component** (`next/image`):
- Automatic WebP/AVIF conversion
- Responsive srcset generation
- Lazy loading with Intersection Observer
- Blur-up placeholders (optional)

**Asset Sizing**:
- Avatars: 40x40px
- Logos: Auto height, 8rem width
- Product images: Full-width responsive

### Code Splitting

- **Route-Based**: Each page loads only required code
- **Component-Based**: Dynamic imports for heavy components (if needed)
- **SVG Tree-Shaking**: Only used SVG paths included in bundle

### Build Optimizations

```json
// next.config.mjs optimizations
{
  "swcMinify": true,           // Rust-based minifier
  "reactStrictMode": true,     // Development checks
  "webpack": "custom",         // SVG loader integration
}
```

### Bundle Analysis

```bash
# Analyze bundle size
npm run build
# Check .next/build-manifest.json for chunk sizes
```

**Estimated Bundle Sizes**:
- Main bundle: ~150KB (gzipped)
- Framer Motion: ~45KB (gzipped)
- Total First Load JS: ~200KB

---

##  Development Workflow

### Installation

```bash
# Clone repository
git clone https://github.com/yourusername/saas-landing-site.git
cd saas-landing-site

# Install dependencies
npm install

# Start development server
npm run dev
```

### Available Scripts

```json
{
  "dev": "next dev",           // Start dev server (http://localhost:3000)
  "build": "next build",       // Production build
  "start": "next start",       // Start production server
  "lint": "next lint"          // Run ESLint
}
```

### Development Server

```bash
npm run dev
```

**Features**:
- Hot Module Replacement (HMR)
- Fast Refresh for React components
- Error overlay with stack traces
- TypeScript type checking

### Building for Production

```bash
npm run build
```

**Output**:
- Static HTML files (`.next/static`)
- Optimized JavaScript bundles
- Image optimization cache
- Build manifest

---

##  Deployment

### Vercel Deployment (Recommended)

**One-Click Deploy**:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/yourusername/saas-landing-site)

**Manual Deployment**:

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy to Vercel
vercel

# Deploy to production
vercel --prod
```

**Automatic Deployments**:
- Pushes to `main` branch → Production
- Pull requests → Preview deployments

### Custom Deployment

**Static Export**:
```bash
# Update next.config.mjs
module.exports = {
  output: 'export',
}

# Build
npm run build

# Output in /out directory
```

**Server Deployment**:
```bash
npm run build
npm run start
```

---

## ⚙Environment & Configuration

### Environment Variables

Create `.env.local` for environment-specific variables:

```bash
# Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX

# API Endpoints (if needed)
NEXT_PUBLIC_API_URL=https://api.example.com
```

### TypeScript Configuration

**File**: `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

**Key Features**:
- Path aliases (`@/` → `src/`)
- Strict type checking
- JSX preservation for Next.js

---

##  Browser Support

| Browser | Version |
|---------|---------|
| Chrome | ≥90 |
| Firefox | ≥88 |
| Safari | ≥14 |
| Edge | ≥90 |

**Polyfills**: Not required (modern browsers only)

**Progressive Enhancement**:
- Animations degrade gracefully on older browsers
- Core functionality works without JavaScript
- Images have proper alt text

---

##  Contributing

### Development Guidelines

1. **Code Style**: Follow existing patterns
2. **TypeScript**: Maintain strict typing
3. **Components**: Keep sections under 200 lines
4. **Commits**: Use conventional commits (`feat:`, `fix:`, `docs:`)

### Pull Request Process

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'feat: add amazing feature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📄 License

This project is licensed under the MIT License.

---

## 📞 Contact & Support

- **Author**: Avinash Singh
- **Email**: avinash06nitp@gmail.com
- **GitHub**: [@avinash06nitp](https://github.com/avinash06nitp)

---

## Acknowledgments

- Design inspiration: Modern SaaS landing pages
- Animations: Framer Motion documentation
- Icons: Custom SVG assets
- Fonts: Google Fonts (DM Sans)

---

<div align="center">

[⬆ Back to Top](#atomic-labs---saas-landing-page)

</div>

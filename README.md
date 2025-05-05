# Portfolio Website

A modern portfolio website built with Nuxt.js, featuring smooth animations and interactive sections.

## Project Structure

```
portfolio/
├── components/
│   ├── HeroSection.vue
│   ├── ApproachSection.vue
│   ├── ExpertiseSection.vue
│   └── AnimatedText.vue
├── pages/
│   ├── index.vue
│   └── expertise/
│       ├── frontend.vue
│       ├── ecommerce.vue
│       ├── design.vue
│       └── performance.vue
├── public/
│   ├── images/
│   │   ├── frontend.jpg
│   │   ├── ecommerce.jpg
│   │   ├── design.jpg
│   │   └── performance.jpg
│   ├── iconshop.png
│   └── laptopicon.png
└── plugins/
    └── motion.js
```

## Expertise Section

The expertise section (`ExpertiseSection.vue`) features an interactive FAQ-style layout with animated modals. Each expertise item links to a dedicated page under the `/expertise` route.

### Expertise Pages Structure

1. **Frontend Development** (`/expertise/frontend`)
   - Modern web development practices
   - Vue.js and React expertise
   - Responsive design implementation

2. **E-commerce Solutions** (`/expertise/ecommerce`)
   - Shopify store development
   - E-commerce optimization
   - Custom shopping experiences

3. **UI/UX Design** (`/expertise/design`)
   - User interface design
   - User experience optimization
   - Interactive prototypes

4. **Performance Optimization** (`/expertise/performance`)
   - Website speed optimization
   - Technical SEO implementation
   - Performance monitoring

### Interactive Features

- Hover animations on expertise items
- Modal with sliding images
- Custom cursor with "View" label
- Smooth transitions between sections

## Dependencies

- Nuxt.js
- GSAP for animations
- @vueuse/motion for Vue.js animations
- Tailwind CSS for styling

## Development

1. Install dependencies:
```bash
npm install
```

2. Run development server:
```bash
npm run dev
```

3. Build for production:
```bash
npm run build
```

## Setup

Make sure to install dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install

# bun
bun install
```

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm dev

# yarn
yarn dev

# bun
bun run dev
```

## Production

Build the application for production:

```bash
# npm
npm run build

# pnpm
pnpm build

# yarn
yarn build

# bun
bun run build
```

Locally preview production build:

```bash
# npm
npm run preview

# pnpm
pnpm preview

# yarn
yarn preview

# bun
bun run preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.

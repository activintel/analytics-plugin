Yep—Codex is perfect for this. Paste the prompt below into Codex and let it scaffold the whole site. It’s self-contained (no downloads needed), includes Tailwind + “shadcn/ui-style” tokens, pages for all four products, a platform blueprint slot, and Docker/CI.

# Single prompt for Codex

**Goal:** Create a brand-new public website for **Third Ray** using **Vite + React + TypeScript + TailwindCSS**, with shadcn/ui-style tokens and a small set of reusable UI components. Pages: Home, Platform, Products (with subpages for Activi, Zephyr, Nexus, Aurora), Solutions, Pricing, Contact, and 404. Include a dark-mode toggle, simple forms, and a drop-in image slot for a “Platform Blueprint” PNG. Provide Dockerfile and GitHub Action.

**Tech & tooling**

* React 18, TypeScript, Vite 5, React Router 6.
* TailwindCSS 3 with shadcn/ui-style CSS variables (tokens) for theming.
* lucide-react for icons.
* No Next.js, no node server—we’ll build and host as static assets.
* Create minimal UI primitives (Button, Card, Input, Badge, Tabs, Accordion) in `src/components/ui/*` using Tailwind classes.

**Project structure**

```
thirdray-site/
  index.html
  package.json
  tsconfig.json
  vite.config.ts
  postcss.config.js
  tailwind.config.js
  public/
    favicon.ico
    ThirdRay-OnePager.pdf   # placeholder file
    images/platform-blueprint.png  # placeholder image
  src/
    main.tsx
    styles/globals.css
    lib/utils.ts
    components/ui/{button.tsx,card.tsx,input.tsx,badge.tsx,tabs.tsx,accordion.tsx}
    routes/App.tsx
    pages/
      Home.tsx
      Platform.tsx
      Products.tsx
      Activi.tsx
      Zephyr.tsx
      Nexus.tsx
      Aurora.tsx
      Solutions.tsx
      Pricing.tsx
      Contact.tsx
      NotFound.tsx
  Dockerfile
  .github/workflows/build.yml
  README.md
```

**Brand & tokens (put into `src/styles/globals.css`)**

* Define CSS variables (HSL) for light/dark like shadcn: `--background`, `--foreground`, `--primary`, `--accent`, `--border`, etc.
* Primary color: **electric violet** vibe (approx HSL 259, 84%, 56%); set sensible dark-mode variants.
* Add Tailwind base that applies `bg-background text-foreground` to `body`.
* Add `.dark` class tokens (manual toggle).

**Tailwind config (`tailwind.config.js`)**

* `darkMode: ["class"]`
* `content: ["./index.html","./src/**/*.{ts,tsx,js,jsx}"]`
* Container width 1400px, padding 2rem.
* Extend colors from CSS variables (`hsl(var(--primary))` etc.).
* Add small borderRadius tokens and simple accordion keyframes.

**Routing**

* Use `react-router-dom` with a root layout `src/routes/App.tsx` that renders:

  * Sticky top Nav with links: Platform, Products, Solutions, Pricing, Contact.
  * Brand text “Third Ray” on the left (we’ll swap logo later).
  * Right side: Theme toggle button and a CTA button linking `/ThirdRay-OnePager.pdf`.
  * Footer with product links and contact email `hello@thirdray.ai`.
* `Outlet` renders child pages.

**UI components**

* `Button`: variants `default | secondary | outline | ghost`; sizes `sm | md | lg`. Tailwind classes only.
* `Card`: `{Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter}`.
* `Input`, `Badge`, `Tabs` (very simple), `Accordion` (basic wrapper).
* Utilities in `src/lib/utils.ts`: `cn()` (clsx) and `toggleTheme()` toggling `.dark` on `document.documentElement`.

**Pages (content drafts)**

* **Home.tsx**

  * Hero: “Intelligent Automation Platform”. Subtitle:
    “Activi orchestrates intelligence. Zephyr is the data backbone. Nexus is the agent control layer. Aurora is domain execution.”
  * Two CTAs: “See Platform” (link `/platform`) and “Talk to Sales” (link `/contact`).
  * Show `/public/images/platform-blueprint.png` centered in a rounded card.
  * Four product cards linking to their pages with one-line descriptions.

* **Platform.tsx**

  * Title: “Third Ray Intelligent Automation Platform”.
  * Paragraph describing land-expand-scale and interlocks:

    * Activi↔Zephyr (data-triggered workflows), Activi↔Nexus (embedded agents), Zephyr↔Nexus (agent data-access), Aurora↔All.
  * Display the same blueprint image.
  * Two boxes: **Entry Path** (Activi, Aurora, Zephyr, Nexus depending on ICP) and **Expand & Scale** (Zephyr as backbone, Nexus standardizes agents, Aurora operationalizes, unified contract).

* **Products.tsx**

  * Grid with four tiles linking to **/products/activi**, **/products/zephyr**, **/products/nexus**, **/products/aurora**.

* **Activi.tsx**

  * Explain no-code orchestration with embedded AI agents, unstructured data handling, dynamic processes, decision logic; human-in-the-loop, Slack/Email, observability hooks; SOC2/ISO alignment.

* **Zephyr.tsx**

  * Iceberg workspace, real Iceberg REST catalog, S3/S3-compatible, Python exec engine, CDC ingestion, secure sharing; snapshots, schema evolution, triggers, access control.

* **Nexus.tsx**

  * Agent trigger/manager/orchestration; evaluation, guardrails, cost/latency controls; multi-agent patterns, provider-agnostic.

* **Aurora.tsx**

  * Manufacturing/vendor automation; uses Activi+Zephyr+Nexus; use cases: vendor onboarding & compliance, predictive maintenance, plant-floor digitization.

* **Solutions.tsx**

  * Two columns (Telco/OSS-BSS and Manufacturing) each with 3 bullet use cases and the product combos used.

* **Pricing.tsx**

  * Three simple tiers (Starter, Growth, Enterprise) with bullets; all “Custom” price copy.

* **Contact.tsx**

  * Simple form (name, email, message); submit shows a thank-you (no backend).

* **NotFound.tsx**

  * Minimal 404 with link home.

**Placeholders**

* Create a tiny placeholder `public/images/platform-blueprint.png` (e.g., simple SVG converted to PNG) and `public/ThirdRay-OnePager.pdf` with a short text “placeholder” so links work.

**Packages & scripts (`package.json`)**

* deps: `react`, `react-dom`, `react-router-dom`, `lucide-react`, `clsx`
* devDeps: `vite`, `@vitejs/plugin-react`, `typescript`, `tailwindcss`, `postcss`, `autoprefixer`, `@types/react`, `@types/react-dom`, `@types/node`, `eslint`
* scripts:

  * `dev`: `vite`
  * `build`: `tsc -b && vite build`
  * `preview`: `vite preview`
  * `lint`: `eslint .`

**Dockerfile (multi-stage)**

```Dockerfile
# Build
FROM node:22-slim AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Serve
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
```

**GitHub Action `.github/workflows/build.yml`**

* Trigger on push to main.
* Use Node 22, `npm ci`, `npm run build`.
* Upload `dist` as artifact named `site-dist`.

**README.md**

* Quickstart, dev/build/preview commands, how to change brand colors (edit CSS variables), where to replace the one-pager and blueprint image, and how to run Docker:

  * `docker build -t thirdray-site .`
  * `docker run -p 8080:80 thirdray-site`

**Acceptance criteria**

* `npm run dev` starts site at `/`.
* Navbar links work; dark-mode toggle flips tokens (no FOUC).
* All pages render with copy above.
* Mobile responsive (stacked grids, readable text).
* Lighthouse pass: no blocking console errors.
* Docker image serves built site on port 80.
* Artifact produced in GH Actions on push.

**Deliverables**

* Entire repo created exactly as structure above.
* All files committed.
* Print final tree and the exact commands to run:

  1. `npm ci`
  2. `npm run dev`
  3. `npm run build && npm run preview`
  4. Docker build/run commands.

---

If you want, I can also give you a **follow-up Codex prompt** to add:

* Your **actual logo** and swap typography,
* A **blog** section with MDX,
* A **CMS hook** (e.g., Contentlayer or a simple JSON content loader),
* Or a **Netlify/Vercel** deploy workflow.

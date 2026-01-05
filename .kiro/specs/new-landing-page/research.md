# Research & Design Decisions

---
**Purpose**: Capture discovery findings, architectural investigations, and rationale that inform the technical design.

**Usage**:
- Log research activities and outcomes during the discovery phase.
- Document design decision trade-offs that are too detailed for `design.md`.
- Provide references and evidence for future audits or reuse.
---

## Summary
- **Feature**: `new-landing-page`
- **Discovery Scope**: New Feature (Greenfield)
- **Key Findings**:
  - Next.js 14+ with App Router is the modern standard for landing pages in 2026, providing server components, built-in image optimization, and performance benefits
  - WCAG 2.1 Level AA compliance is mandatory by April 2026 under ADA Title II requirements
  - Lighthouse performance score of 85+ requires strategic focus on Total Blocking Time (30% weight) and Largest Contentful Paint (25% weight)
  - Headless CMS integration enables content management without developer involvement while maintaining performance
  - Google Analytics 4 with event-driven tracking is the standard for modern analytics integration

## Research Log

### Modern Landing Page Architecture (Next.js 2026)

- **Context**: Determine the most appropriate frontend framework and architecture pattern for a high-performance, modern landing page
- **Sources Consulted**:
  - [Build a Landing Page with AI and Next.js - Strapi](https://strapi.io/blog/build-a-landing-page-with-ai-and-nextjs)
  - [Next.js Folder Structure Best Practices 2026](https://www.codebydeep.com/blog/next-js-folder-structure-best-practices-for-scalable-applications-2026-guide)
  - [How to Master Next.js in 2026 - Medium](https://medium.com/@hashbyt/nextjs-advanced-techniques-2026-93e09f1c728d)
- **Findings**:
  - Next.js App Router (default since 2025-2026) fully integrates React Server Components, layouts, nested routes, and improved data fetching
  - React Server Components (RSC) render on server by default, improving performance by reducing client-side JavaScript
  - Image optimization using Next.js Image component with automatic WebP/AVIF format selection and lazy loading
  - Incremental Static Regeneration (ISR) allows updating static pages without full rebuild, balancing performance and freshness
  - React cache and memoization reduce backend calls while speeding up route transitions
- **Implications**:
  - Architecture should leverage Next.js 14+ App Router with Server Components for hero and static sections
  - Image optimization handled natively through Next.js Image component
  - Static generation with ISR for content updates without redeployment

### Performance Optimization & Lighthouse Score

- **Context**: Achieve requirement 6 target of Lighthouse score 85+ and 3-second load time
- **Sources Consulted**:
  - [Lighthouse Performance Scoring - Chrome Developers](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)
  - [How we improved the Lighthouse score to 96 - Checkly](https://www.checklyhq.com/blog/how-we-improved-the-lighthouse-score-of-our-landing-page-to-96/)
  - [Understanding Lighthouse Performance Scores](https://pagespeed.deployhq.com/guides/lighthouse-score)
- **Findings**:
  - Performance score is weighted average of 6 metrics: Total Blocking Time (30%), Largest Contentful Paint (25%), FCP, CLS, Speed Index
  - 90-100 score range indicates excellent performance; 85+ is achievable with optimization
  - Key optimization strategies: compressed next-gen formats (WebP/AVIF), lazy loading, removing unused JavaScript/CSS
  - Real-world case study achieved 96+ score through WebP conversion, lazy loading, and removing third-party tools
  - Lighthouse score doesn't directly impact SEO, but slow performance affects real-user Core Web Vitals which are ranking signals
- **Implications**:
  - Design must prioritize LCP optimization (hero section images, above-the-fold content)
  - Lazy loading mandatory for below-the-fold images
  - JavaScript bundle splitting and code elimination critical
  - Third-party analytics integration must be async and non-blocking

### WCAG 2.1 Level AA Accessibility Compliance

- **Context**: Meet requirement 7 for WCAG 2.1 Level AA compliance
- **Sources Consulted**:
  - [What WCAG 2.1 AA Means for ADA Title II - Medium](https://adabook.medium.com/what-wcag-2-1-aa-means-for-ada-title-ii-web-compliance-in-2026-904d60fff912)
  - [Mastering Accessibility in ReactJS - Medium](https://medium.com/@sajjadjavadi/mastering-accessibility-in-reactjs-deep-dives-into-aria-semantic-components-and-best-practices-25ae6f30daf3)
  - [WCAG 2.1 AA compliance guidelines](https://innowise.com/blog/wcag-21-aa/)
- **Findings**:
  - WCAG 2.1 AA contains 50 success criteria, mandatory for U.S. ADA Title II compliance starting April 2026
  - POUR principles: Perceivable, Operable, Understandable, Robust
  - Automated tools (Axe DevTools, Lighthouse) help identify violations
  - React implementation requires semantic HTML, proper ARIA labels, keyboard navigation, focus management
  - Color contrast ratios: 4.5:1 for normal text, 3:1 for large text
  - All interactive elements must support full keyboard navigation with visible focus indicators
- **Implications**:
  - Component design must include semantic HTML (header, main, section, nav, footer)
  - All images require alt text; decorative images use empty alt=""
  - Form components need proper labels and error messages
  - Focus management for modal interactions and dynamic content
  - Automated testing integration in CI/CD pipeline

### Google Analytics 4 Integration Best Practices

- **Context**: Implement requirement 9 for analytics tracking with modern privacy-compliant approach
- **Sources Consulted**:
  - [Google Analytics 4 Everything You Need to Know - KrokenTech](https://www.krokentech.com/google-analytics-4-ga4/)
  - [Top 10 Google Analytics Best Practices 2026 - Medium](https://medium.com/@admin_28353/top-10-google-analytics-best-practices-2026-you-must-know-0e397342f426)
  - [Top 8 Google Analytics 4 Tips for 2026 - RW Digital](https://www.rwdigital.ca/blog/top-8-google-analytics-4-tips-for-2026/)
- **Findings**:
  - GA4 emphasizes event-driven architecture vs. session-based tracking
  - Privacy-first design complies with GDPR, CCPA, DPDP Act
  - Integration with Google Ads, BigQuery, Looker Studio for advanced analysis
  - Naming conventions: lowercase, underscores, under 40 characters
  - Automatic cost data tracking from Meta, TikTok, Pinterest, Snap
  - Monthly light audit and quarterly full audit recommended for accuracy
  - Multi-touch attribution and predictive analytics built-in
- **Implications**:
  - Event schema design required for CTA clicks, form submissions, scroll depth, time on page
  - gtag.js or @next/third-parties for Next.js integration
  - Consent management required for privacy compliance
  - Non-blocking script loading to avoid performance impact

### Headless CMS for Content Management

- **Context**: Enable requirement 11 for content management without code deployment
- **Sources Consulted**:
  - [Best headless CMS for Next.js in 2026 - Naturaily](https://naturaily.com/blog/next-js-cms)
  - [Headless CMS Showdown: Strapi vs. Contentful vs. Sanity - Meerako](https://www.meerako.com/blogs/headless-cms-showdown-strapi-vs-contentful-vs-sanity-nextjs)
  - [Strapi vs Storyblok vs Contentful - Netguru](https://www.netguru.com/blog/strapi-vs-storyblok-vs-contentful)
- **Findings**:
  - Headless CMS separates backend from frontend, delivering content via APIs
  - **Strapi v5**: Open-source (MIT), free, self-hosted, REST/GraphQL APIs, full customization, requires infrastructure management
  - **Contentful**: Enterprise SaaS, $300+/month, excellent localization, clean UI for non-technical users, scales with API calls
  - **Sanity**: Real-time collaboration, structured content, portable text, developer-friendly
  - Direct integration with Next.js ISR, Preview Mode, and webhooks
- **Implications**:
  - CMS selection impacts hosting requirements and operational cost
  - Content structure should be defined as JSON schema or GraphQL types
  - Webhook integration enables automatic revalidation on content updates
  - Preview mode allows content editors to view changes before publishing

### Responsive Design Breakpoints Strategy

- **Context**: Implement requirement 5 for responsive design across device sizes
- **Sources Consulted**:
  - [Responsive Design Breakpoints in 2025 - BrowserStack](https://www.browserstack.com/guide/responsive-design-breakpoints)
  - [Common Screen Resolutions in 2026 - BrowserStack](https://www.browserstack.com/guide/common-screen-resolutions)
  - [Responsive Design Breakpoints 2025 Playbook - DEV Community](https://dev.to/gerryleonugroho/responsive-design-breakpoints-2025-playbook-53ih)
- **Findings**:
  - Recommended breakpoints: 360px (mobile), 768px (tablet), 1366px (desktop) based on market share
  - Mobile-first approach with min-width breakpoints is best practice in 2026
  - Content-based breakpoints should supplement device-based breakpoints
  - Limit to 2-3 major breakpoints for maintainability
  - Over 70% of global internet traffic from mobile devices (Statista 2025)
  - Continuous viewport resizing during testing catches layout issues between breakpoints
- **Implications**:
  - Base styles designed for 320px minimum width
  - Major breakpoints at 640px (sm), 768px (md), 1024px (lg), 1280px (xl) following Tailwind CSS convention
  - Component-level breakpoints for granular control (e.g., navigation menu, feature cards)
  - Grid systems adapt: 1 column (mobile), 2-3 columns (tablet), 3-4 columns (desktop)

### Next.js Image Optimization (WebP/AVIF)

- **Context**: Implement requirement 6 image optimization for performance
- **Sources Consulted**:
  - [Next.js Image Optimization - DebugBear](https://www.debugbear.com/blog/nextjs-image-optimization)
  - [Optimizing Images - Next.js Official Docs](https://nextjs.org/docs/14/app/building-your-application/optimizing/images)
  - [Next.js Image Component Guide - Strapi](https://strapi.io/blog/nextjs-image-optimization-developers-guide)
- **Findings**:
  - Next.js serves WebP/AVIF automatically based on browser Accept headers
  - AVIF 25-70% smaller than JPEG/PNG; WebP fallback for broader support
  - Built-in lazy loading for off-screen images (loading="lazy" default)
  - LCP images must use loading="eager" and fetchPriority="high"
  - Responsive srcset generated automatically for device sizes
  - Each format cached separately (increased storage requirement)
  - next.config.js allows format configuration
- **Implications**:
  - Use Next.js Image component exclusively for all images
  - Hero images: loading="eager", fetchPriority="high", priority prop
  - Below-fold images: default lazy loading
  - Specify width/height to prevent layout shift (CLS)
  - Configure image domains in next.config.js for external sources

## Architecture Pattern Evaluation

| Option | Description | Strengths | Risks / Limitations | Notes |
|--------|-------------|-----------|---------------------|-------|
| **Next.js App Router with Server Components (Selected)** | Modern Next.js architecture with React Server Components for static sections and Client Components for interactive elements | Native performance optimization, automatic code splitting, built-in image optimization, ISR support, SEO-friendly | Learning curve for RSC paradigm, requires careful client/server boundary management | Aligns with 2026 best practices, direct integration with headless CMS |
| Component-Based Architecture | Modular React components with clear separation of concerns | Reusability, maintainability, parallel development, testability | Component proliferation if not carefully organized | Foundation for the implementation |
| Atomic Design Pattern | Hierarchical component structure (atoms → molecules → organisms → templates → pages) | Clear component hierarchy, design system alignment, scalability | May be over-engineered for single landing page | Useful for future expansion |

## Design Decisions

### Decision: Next.js 14+ App Router with Hybrid Rendering

- **Context**: Need to balance performance, SEO, and dynamic content capabilities for landing page
- **Alternatives Considered**:
  1. **Pure Static Site Generation (SSG)** — Pre-render all content at build time
  2. **Client-Side Rendering (CSR)** — Render entirely in browser with React
  3. **Hybrid: App Router with Server Components + ISR** — Server-render static content, use ISR for updates, Client Components for interactivity
- **Selected Approach**: Hybrid rendering with Next.js App Router
  - Static sections (hero, features, social proof) as Server Components
  - Interactive elements (forms, analytics, modals) as Client Components
  - ISR with revalidation for content updates via CMS webhooks
- **Rationale**:
  - Maximizes Lighthouse score through reduced JavaScript payload
  - Maintains SEO benefits of server-side rendering
  - Enables content updates without redeployment
  - Aligns with 2026 best practices and Next.js ecosystem
- **Trade-offs**:
  - Benefits: Best performance, optimal SEO, reduced client-side JavaScript, built-in optimizations
  - Compromises: Requires understanding of server/client component boundaries, increased initial setup complexity
- **Follow-up**: Verify ISR revalidation timing based on content update frequency during implementation

### Decision: Headless CMS Integration (CMS-Agnostic Design)

- **Context**: Requirement 11 mandates content management without code deployment
- **Alternatives Considered**:
  1. **No CMS (hardcoded content)** — Simple but requires developer for updates
  2. **Strapi v5 (open-source)** — Free, full control, self-hosted
  3. **Contentful (enterprise SaaS)** — Managed service, excellent DX, $300+/month
  4. **CMS-agnostic abstraction layer** — Define content schema, allow any CMS backend
- **Selected Approach**: CMS-agnostic design with content schema definition
  - Define TypeScript interfaces for landing page content structure
  - Create adapter pattern for CMS integration
  - Implement content validation layer
  - Support both file-based (JSON) and API-based content sources
- **Rationale**:
  - Allows project to start with simple JSON files or free tier CMS
  - Enables future migration to enterprise CMS without architecture changes
  - Reduces vendor lock-in risk
  - Maintains flexibility for different deployment contexts
- **Trade-offs**:
  - Benefits: Maximum flexibility, no upfront cost commitment, easy testing
  - Compromises: Additional abstraction layer, requires adapter implementation
- **Follow-up**: Document recommended CMS choices (Strapi for self-hosted, Contentful for managed) with integration guides

### Decision: Google Analytics 4 with Event-Driven Tracking

- **Context**: Requirement 9 requires comprehensive analytics with privacy compliance
- **Alternatives Considered**:
  1. **GA4 with gtag.js** — Official Google library
  2. **GA4 via Google Tag Manager** — Centralized tag management
  3. **Self-hosted analytics (Plausible, Umami)** — Privacy-first, no cookies
  4. **GA4 with @next/third-parties** — Next.js optimized integration
- **Selected Approach**: GA4 with @next/third-parties package
  - Use Next.js Script component with afterInteractive strategy
  - Implement custom event tracking via gtag function
  - Create analytics service abstraction for event consistency
  - Include consent management for privacy compliance
- **Rationale**:
  - @next/third-parties provides optimized loading for Next.js
  - afterInteractive strategy prevents blocking page load
  - Event abstraction allows analytics provider swapping
  - GA4 provides industry-standard features and integrations
- **Trade-offs**:
  - Benefits: Non-blocking performance, privacy compliance, standard analytics features
  - Compromises: Dependency on Google service, requires consent management implementation
- **Follow-up**: Define complete event taxonomy (page_view, cta_click, form_submit, scroll_depth) during implementation

### Decision: Tailwind CSS for Styling

- **Context**: Need efficient, responsive, maintainable styling system
- **Alternatives Considered**:
  1. **CSS Modules** — Scoped styles, more verbose
  2. **Styled Components** — CSS-in-JS, runtime overhead
  3. **Tailwind CSS** — Utility-first, build-time processing
  4. **Vanilla CSS with design tokens** — Full control, more manual work
- **Selected Approach**: Tailwind CSS with custom configuration
  - Utility-first classes for rapid development
  - Custom theme configuration for brand colors, spacing, typography
  - JIT (Just-In-Time) compiler for minimal CSS bundle
  - Responsive modifiers for breakpoints
- **Rationale**:
  - Zero runtime overhead (unlike CSS-in-JS solutions)
  - Built-in responsive design utilities
  - Excellent integration with Next.js
  - Strong community and tooling support
  - Smaller CSS bundle with purging
- **Trade-offs**:
  - Benefits: Fast development, small bundle size, consistent design system
  - Compromises: HTML can become verbose with many classes, learning curve for utility-first approach
- **Follow-up**: Define custom Tailwind config with brand colors and breakpoints

### Decision: Form Handling Strategy

- **Context**: Requirement 12 for optional contact form integration
- **Alternatives Considered**:
  1. **React Hook Form** — Lightweight, excellent performance
  2. **Formik** — Popular but heavier
  3. **Native HTML5 validation** — Simple but limited
  4. **Next.js Server Actions** — Server-side form handling
- **Selected Approach**: React Hook Form with Zod validation + Server Actions
  - Client-side validation with React Hook Form and Zod schema
  - Server-side submission via Next.js Server Actions
  - Integration points for CRM/email marketing (abstract interface)
  - Accessibility-compliant error messaging
- **Rationale**:
  - React Hook Form minimizes re-renders, improving performance
  - Zod provides type-safe validation shared between client and server
  - Server Actions eliminate need for separate API routes
  - Adapter pattern supports multiple CRM integrations
- **Trade-offs**:
  - Benefits: Best performance, type safety, simple architecture
  - Compromises: Additional dependencies (react-hook-form, zod)
- **Follow-up**: Define CRM adapter interface for flexibility

## Risks & Mitigations

- **Risk 1: Third-party script impact on Lighthouse score** — Mitigation: Use async/defer loading, @next/third-parties optimization, lazy load analytics after initial render
- **Risk 2: CMS performance impact on ISR** — Mitigation: Implement aggressive caching strategy, use stale-while-revalidate pattern, monitor revalidation times
- **Risk 3: Accessibility violations** — Mitigation: Automated testing with axe-core in CI/CD, manual testing with screen readers, accessibility checklist review
- **Risk 4: Image optimization complexity with CMS** — Mitigation: Use Next.js Image component with remote patterns, implement image proxy if needed, document image dimension requirements
- **Risk 5: Form submission reliability** — Mitigation: Implement retry logic, show user-friendly error messages, provide alternative contact methods, log failures for monitoring
- **Risk 6: Browser compatibility issues** — Mitigation: Polyfills for older browsers, progressive enhancement strategy, automated cross-browser testing

## References

- [Build a Landing Page with AI and Next.js - Strapi](https://strapi.io/blog/build-a-landing-page-with-ai-and-nextjs)
- [Next.js Folder Structure Best Practices 2026](https://www.codebydeep.com/blog/next-js-folder-structure-best-practices-for-scalable-applications-2026-guide)
- [How to Master Next.js in 2026 - Medium](https://medium.com/@hashbyt/nextjs-advanced-techniques-2026-93e09f1c728d)
- [Lighthouse Performance Scoring - Chrome Developers](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)
- [How we improved the Lighthouse score to 96 - Checkly](https://www.checklyhq.com/blog/how-we-improved-the-lighthouse-score-of-our-landing-page-to-96/)
- [What WCAG 2.1 AA Means for ADA Title II - Medium](https://adabook.medium.com/what-wcag-2-1-aa-means-for-ada-title-ii-web-compliance-in-2026-904d60fff912)
- [Mastering Accessibility in ReactJS - Medium](https://medium.com/@sajjadjavadi/mastering-accessibility-in-reactjs-deep-dives-into-aria-semantic-components-and-best-practices-25ae6f30daf3)
- [Google Analytics 4 Everything You Need to Know - KrokenTech](https://www.krokentech.com/google-analytics-4-ga4/)
- [Top 10 Google Analytics Best Practices 2026 - Medium](https://medium.com/@admin_28353/top-10-google-analytics-best-practices-2026-you-must-know-0e397342f426)
- [Best headless CMS for Next.js in 2026 - Naturaily](https://naturaily.com/blog/next-js-cms)
- [Headless CMS Showdown: Strapi vs. Contentful vs. Sanity - Meerako](https://www.meerako.com/blogs/headless-cms-showdown-strapi-vs-contentful-vs-sanity-nextjs)
- [Responsive Design Breakpoints in 2025 - BrowserStack](https://www.browserstack.com/guide/responsive-design-breakpoints)
- [Next.js Image Optimization - DebugBear](https://www.debugbear.com/blog/nextjs-image-optimization)
- [Optimizing Images - Next.js Official Docs](https://nextjs.org/docs/14/app/building-your-application/optimizing/images)

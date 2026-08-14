# Purelane Homepage - Shopify Build

This repository contains my submission for the Troopod AI Product Engineer assignment. I have converted the provided `purelane-homepage.html` prototype into production-ready, modular Shopify Liquid sections built on top of the Dawn theme.

## 01. Dev Store URL and Password
* **URL:** [https://troopod-shubham-dev.myshopify.com/]
* **Password:** [edraiy]

## 02. GitHub Repo
The commit history is preserved within this repository, demonstrating a logical, step-by-step feature implementation from base setup to individual modular sections.

## 03. Metafield or Metaobject Definitions
**N/A.** I intentionally opted out of using Metafields/Metaobjects for this specific build. 
* **Reasoning:** To ensure maximum flexibility for a marketing team without requiring complex backend setup. I heavily utilized Liquid `schema` blocks, dynamic settings, and block iterators within the sections themselves. This keeps the sections self-contained, "plug-and-play," and fully customizable directly from the Shopify Theme Editor.

## 04. Short Notes on the Build
### What I'd flag about the original file:
* **The Monolith Anti-Pattern:** The original file was a single HTML document with massive base64 encoded SVG images injected directly into a unified `<style>` block. This approach is detrimental to Shopify architecture as it bloats the DOM, hinders caching, and makes asset management impossible for a non-technical merchant.
* **Lack of Dynamic Structure:** It was completely static. Implementing it required mapping repeating UI elements (like cards and combos) into repeatable Liquid blocks.

### What I changed in the code and why:
* **Modularization:** I refactored the monolith into 14 independent Liquid sections (e.g., `purelane-hero`, `purelane-shop`, `purelane-combos`). 
* **Scoped CSS:** To prevent the original CSS from overriding or breaking Dawn's native styles, I strictly scoped all custom styling to `#shopify-section-{{ section.id }}`.
* **Bulletproof Fallbacks & Real Shopify Data:** Sections like the "Shop Grid" are wired to standard Shopify collection objects. 
    * If a collection is selected, it pulls real data, handles edge cases gracefully (e.g., dynamic "Sold Out" badges, compare-at pricing logic). 
    * If no collection is selected, I implemented logic to render the exact static base64 SVG layout as a fallback, ensuring the merchant's theme editor preview never breaks.
* **Native JavaScript Animations:** I decoupled animations from the original code and implemented lightweight `IntersectionObserver` logic natively within the sections to ensure performant scroll-triggered fade-ins without relying on external libraries.

### What I'd do with more time:
* **Asset Optimization:** Extract all the inline base64 SVGs/images, upload them to the Shopify CDN (Assets folder), and render them dynamically using the `image_url` filter with `loading="lazy"` to significantly improve Core Web Vitals (LCP/FCP).
* **AJAX Cart Integration:** Implement AJAX cart functionality so the "Add to cart" buttons update the slide-out cart dynamically without triggering a full page reload.
* **Accessibility Enhancements:** Deepen the accessibility focus by ensuring comprehensive `aria-labels` on all interactive elements, fully managing focus states, and implementing `prefers-reduced-motion` CSS queries for the animations.

## 05. Short Notes on My AI Workflow
### What I delegated:
* I utilized AI to parse the 800+ line HTML/CSS monolith, split it into logical chunks, and rapidly scaffold the initial Liquid `{% schema %}` structures for customizable text, links, and blocks.

### Where it failed me:
* **Over-engineering:** Initially, the AI attempted to aggressively clean up the original CSS and merge it into Dawn's global stylesheet, which broke the layout.
* **Data Stripping:** The AI erroneously assumed the large base64 strings were "junk data" or placeholders and stripped them out, resulting in broken image rendering in the initial sections.

### What I'd systematise if I had to do twenty more of these:
* I would deploy a strict, predefined prompt framework (or a specialized custom AI Agent) with absolute, non-negotiable constraints: 
    * *"Never strip base64 assets without explicit permission."*
    * *"Strictly wrap and isolate all CSS inside `#shopify-section-{{ section.id }}` to guarantee collision-free rendering within Dawn."*
* By providing the AI with clear boundaries on what NOT to optimize, the process becomes a highly predictable and rapid copy-paste pipeline from prototype to production Liquid.

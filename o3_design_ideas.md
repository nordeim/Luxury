### ✨ Design Ideas & Inspiration  
| Area | Key Decisions | Luxury Inspiration Sources |
|------|---------------|----------------------------|
| Typography | • “Cinzel” (elegant Roman-style serif) for headlines  <br>• “Lato” (clean humanist sans) for body copy | Fashion magazines (e.g., Vogue), Cartier & Dior web typography |
| Palette / Theme | • Day mode: airy neutrals, champagne-gold accent (#d4af37)  <br>• Night mode: near-black canvas, emerald-green accent (#50d890)  | Upscale packaging + hotel-bar ambience (day = luminous, night = speakeasy) |
| Hero | Full-bleed botanical macro shot overlaid with a soft dark veil → ensures white text passes WCAG contrast. | Aesop, La Mer hero treatments |
| 3-D Buttons | Layered linear-gradient + drop & inner shadows → tactile “push” feel. | Apple “hover-lift” pattern and classic Skeuomorphism cues |
| Content Blocks | 1️⃣ Bestseller grid (CSS Grid, card hover-lift)  <br>2️⃣ “Our Holistic Philosophy” two-column story  <br>3️⃣ Pure-CSS testimonial carousel  <br>4️⃣ Champagne-accent newsletter banner | Glossier story section, Jo Malone storytelling, Chanel carousel |
| Motion & Delight | • `@keyframes` carousel  <br>• Fade-in on scroll via IntersectionObserver  | Subtle Lottie-style reveals, Burberry “live” pages |
| Mobile-First | Single-column flow → auto-fit grid on ≥600 px → wider hero type on ≥800 px | Net-à-Porter responsive pattern |
| Accessibility | Alt text, `aria-label`s, focus outlines, 4.5:1 contrast, reduced-motion friendly | W3C + Shopify ADA guidelines |

---

### 🔧 Why the **Day ↔ Night** toggle may feel broken
1. The script flips the `data-theme` attribute on `<html>` but the CSS rules target `html[data-theme="night"]`.  
2. If your browser caches the old CSS, you’ll see no change—hard-refresh or disable cache.  
3. Some elements (e.g. inline SVGs or pseudo-elements) might still own static colors.

```js
// Quick reliability patch
const html = document.documentElement;
const saved = localStorage.getItem('theme') || 'day';
html.dataset.theme = saved;
toggle.textContent = saved === 'night' ? '☀️' : '☾';

toggle.onclick = () => {
  const newTheme = html.dataset.theme === 'night' ? 'day' : 'night';
  html.dataset.theme = newTheme;
  localStorage.setItem('theme', newTheme);
  toggle.textContent = newTheme === 'night' ? '☀️' : '☾';
};
```

---

### 🚀 Future Refinements & Upgrades
1. Performance  
   • `loading="lazy"` on images + `srcset` / `sizes` for optimal resolutions.  
   • Combine critical CSS inline, defer the rest.

2. Micro-interactions  
   • Button ripple or fragrance-mist particle effect on “Add to Cart”.  
   • Subtle parallax on hero foliage.

3. Commerce Features  
   • Mini-cart drawer with live item count.  
   • Back-in-stock modal tied to newsletter.

4. Accessibility & UX  
   • `prefers-reduced-motion` media query to halt carousel / fades.  
   • ARIA “roledescription=carousel” + keyboard controls for testimonials.

5. Visual Polish  
   • Replace JPG heroes with compressed AVIF/WebP.  
   • Add glass-morphism header (blur + translucency) that darkens in night mode.

6. Content Depth  
   • “Ingredient provenance” timeline—interactive map showing farm origins.  
   • Editorial blog snippets for SEO and storytelling.

7. Code Maintainability  
   • Switch to CSS custom-property theming (`--bg`, `--text`, etc.) so one rule-set drives both themes.  
   • Extract components into a small Vue/React layer if dynamic inventory or personalization is needed.

With these tweaks the landing page will move from a solid luxury MVP to a flagship-level digital experience on par with top beauty maisons.

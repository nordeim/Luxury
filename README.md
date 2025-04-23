## Summary

Crafting an effective prompt for an LLM to generate a **stunning, modern e-commerce landing page HTML** involves clearly specifying the **role**, **tone**, and **output format**, while providing detailed **design requirements** and **brand context**. You should include sections on **visual hierarchy**, **typography**, **color palette**, **animations**, **accessibility**, and **performance**, and explicitly instruct the model to wrap its output within ```html tags. Incorporating best practices from prompt engineering ensures clarity and relevance, and referencing luxury e-commerce design principles guarantees a “WOW” effect in the final page. citeturn0search0turn0news57

## Prompt Structure

1. **System/Role Definition**  
   Define the LLM’s persona and capabilities. citeturn0search0  
2. **Brand Context**  
   Describe the luxury brand’s values (natural ingredients, top quality, beauty & aromatherapy focus). citeturn0search1  
3. **Design Brief**  
   Detail the desired page sections (hero, features, testimonials, product showcase, CTA), visual hierarchy, and emotional tone. citeturn0search7  
4. **Technical Requirements**  
   Specify **semantic HTML**, **responsive design**, **CSS animations**, and **accessibility** guidelines. citeturn0search3turn0search10  
5. **Style Guidelines**  
   Indicate typography (2–3 fonts max), color palette (limited to premium hues), whitespace usage, and image quality. citeturn0search17turn0search5  
6. **Output Formatting**  
   Instruct the model to wrap **only** the HTML code within \`\`\`html\`\`\` fences, without additional commentary. citeturn0search0

## Key Components to Include

### Visual Hierarchy & Layout  
- Clear hero section with full-width background image/video. citeturn0search7  
- Distinct sections for features, product highlights, and customer testimonials. citeturn0search1  

### Semantic HTML & Accessibility  
- Use `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`. citeturn0search3  
- Include `alt` attributes on images and `aria-*` labels for interactive elements. citeturn0search9  

### Typography & Color  
- Limit fonts to two (e.g., serif for headings, sans-serif for body). citeturn0search17  
- Employ a cohesive palette of neutral and accent tones to evoke luxury. citeturn0search7  

### Animations & Interactivity  
- Subtle hover effects on buttons and images. citeturn0search10  
- Entrance animations for key elements using CSS keyframes. citeturn0search4  

### Performance & Responsiveness  
- Mobile-first CSS with flexible grids/`flexbox` or `grid`. citeturn0search15  
- Optimize images with `srcset` and lazy loading. citeturn0search9  

## Example Prompt

```plaintext
You are a **High-End Front-End Developer** specialized in luxury e-commerce experiences. Generate a **modern, responsive**, and **visually stunning** landing page for a **top luxury brand** selling natural-ingredient beauty and aromatherapy products. The page must:

1. Reflect the brand’s values: **premium**, **natural**, **holistic**.
2. Include semantic HTML elements: <header>, <nav>, <main>, <section>, <footer>.
3. Feature a **hero** section with full-width background image and bold headline.
4. Present a **product showcase** grid with images, names, and “Add to Cart” buttons.
5. Add a **testimonials** carousel animated with CSS.
6. Use **two fonts maximum**: one elegant serif for headings, one neutral sans-serif for body.
7. Employ a **neutral color palette** with **one accent color** for CTAs.
8. Implement **subtle hover** and **scroll-triggered** animations.
9. Ensure **mobile-first** design with flexible layout.
10. Follow **accessibility** best practices (alt text, ARIA labels).
11. Output ONLY the **complete HTML and inline CSS** within triple backticks labeled `html`, with no extra commentary.
```

Use this prompt verbatim to instruct any LLM for optimal results.

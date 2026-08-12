# Navy & Gold Premium Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current "Cloud Engineer SaaS template" look of `rafawilliams.github.io` with the approved "Navy & Gold Premium" corporate-fintech design, and add the Experience, Skills, and Certifications sections using Rafael's real LinkedIn content — while leaving the hidden Projects section's data/logic and `script.js` functionally untouched.

**Architecture:** Static two-file rewrite. `index.html` is restructured (new sections added, hero simplified to a centered layout, dead markup removed) and `style.css` is rewritten from scratch with a new navy/gold/ivory design system (colors, serif+sans typography, no particle/gradient animation). `script.js` and `projects-config.js` are not touched — every CSS class/id they depend on (`.project-card`, `.filter-btn`, `#projectModal`, etc.) is preserved by name in the new stylesheet, just restyled. There is no build step and no automated test suite (plain HTML/CSS/JS site for GitHub Pages), so verification is done by serving the file locally and checking it in a real browser (visual screenshots + console error check) instead of unit tests.

**Tech Stack:** Plain HTML5, CSS3 (custom properties, Grid/Flexbox, media queries), vanilla JS (unchanged), Font Awesome 6 (CDN, already linked), Google Fonts Inter (CDN, already linked), Georgia as the serif system font (no new font load needed).

**Reference spec:** `docs/superpowers/specs/2026-08-11-navy-gold-redesign-design.md`

---

## Before you start

Read `C:\apps\Portfolio\index.html`, `C:\apps\Portfolio\style.css`, and `C:\apps\Portfolio\script.js` once before making changes, so you can see exactly what's being replaced. Key facts you need going in:

- `script.js` does `document.getElementById('particles')` and `document.querySelectorAll('.tech-item')`. Both are guarded (`if (!particlesContainer) return;` and a `forEach` over a possibly-empty list), so removing the particle container and tech-stack markup from the hero is safe — no JS errors.
- `script.js` builds project cards with this exact markup (do not rename any of these classes/ids when restyling): `.project-card`, `.project-image`, `.project-info`, `.project-title`, `.project-description`, `.project-tags`, `.project-tag[data-tag="..."]`, `.project-links`, `.project-link.primary`/`.secondary`, `#projectsGrid`, `.filter-btn[data-filter="..."]`, `#projectModal`, `.modal-close`, `#modalTitle`, `#modalImage`, `#modalDescription`, `#modalTechnologies`, `#modalGithub`, `#modalDemo`.
- The Projects section (`<section id="projects">...</section>`) is currently commented out in `index.html` between `<!-- Projects Section (oculta temporalmente...` and `-->`. It stays commented out — you're only updating the CSS rules that would style it once it's uncommented later.
- `.gitignore` already excludes `.superpowers/`. The spec doc is already committed. Do not push to GitHub — every push in this project has required an explicit "yes" from the user in chat; the final task in this plan stops at a local commit.

---

### Task 1: Restructure `index.html`

**Files:**
- Modify: `C:\apps\Portfolio\index.html` (full replacement of the `<body>` content; `<head>` stays as-is)

This task: simplifies the hero to the approved "centered classic" layout (drops the particle background, floating shapes, and the code-snippet/tech-stack card entirely — none of that survived the visual review), adds the Experience/Skills/Certifications sections with Rafael's real LinkedIn content, updates the footer nav to link to the new sections, and refreshes the Contact section headline (it still said "transformar tu infraestructura" / "escalar en la nube", leftover Cloud-only phrasing from before this session's earlier cleanup pass).

- [ ] **Step 1: Replace the entire `<body>` of `index.html`**

Keep the existing `<head>` (lines 1–11) untouched. Replace everything from `<body>` to `</html>` with:

```html
<body>
    <!-- Hero Section -->
    <section id="hero" class="hero">
        <div class="hero-content">
            <div class="hero-badge">
                <i class="fas fa-credit-card"></i>
                <span>Payment Systems Specialist</span>
            </div>
            <h1 class="hero-title">Rafael Williams Puerto</h1>
            <p class="hero-description">
                Ingeniero de Software con 10+ años de experiencia, especializado en sistemas de pago (ISO 8583, Mastercard) e infraestructura cloud en AWS.
            </p>
            <div class="hero-buttons">
                <button class="btn btn-primary btn-lg" onclick="scrollToSection('contact')">
                    <i class="fas fa-envelope"></i>
                    Contactar
                </button>
                <button class="btn btn-outline btn-lg" onclick="scrollToSection('experience')">
                    <i class="fas fa-briefcase"></i>
                    Ver Experiencia
                </button>
            </div>
        </div>
        <div class="scroll-indicator" onclick="scrollToSection('experience')">
            <div class="scroll-text">Desliza para explorar</div>
            <div class="scroll-arrow">
                <i class="fas fa-chevron-down"></i>
            </div>
        </div>
    </section>

    <!-- Experience Section -->
    <section id="experience" class="experience">
        <div class="container">
            <div class="section-header">
                <div class="section-badge">
                    <i class="fas fa-briefcase"></i>
                    <span>Experiencia</span>
                </div>
                <h2 class="section-title">Trayectoria profesional</h2>
                <p class="section-subtitle">Más de una década construyendo software, con foco en sistemas de pago desde 2020</p>
            </div>
            <div class="experience-timeline">
                <div class="timeline-item timeline-current">
                    <div class="timeline-dot"></div>
                    <div class="timeline-content">
                        <h3>Ingeniero de Software</h3>
                        <span class="timeline-meta">Banistmo · jun. 2020 - actualidad</span>
                        <p>Diseño e implementación de soluciones tecnológicas, coordinación con proveedores y generación de valor incremental, garantizando disponibilidad y usabilidad óptimas.</p>
                    </div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-content">
                        <h3>Senior Software Engineer</h3>
                        <span class="timeline-meta">Pensanomica · ago. 2015 - jun. 2020</span>
                        <p>Desarrollo e implementación de herramientas de TI basadas en las últimas tendencias de programación web para la gestión empresarial.</p>
                    </div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-content">
                        <h3>Senior Developer</h3>
                        <span class="timeline-meta">Admios · ago. 2012 - jun. 2015</span>
                        <p>Desarrollo de aplicaciones web para clientes exclusivos en el área de la Bahía de San Francisco, usando tecnologías líderes del mercado.</p>
                    </div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-content">
                        <h3>Analista de Sistema</h3>
                        <span class="timeline-meta">Pensanomica · sept. 2011 - ago. 2012</span>
                        <p>Desarrollo e implementación de aplicaciones web para gestión empresarial basadas en CMS, ERP y CRM.</p>
                    </div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-content">
                        <h3>Analista de Sistemas y Programador</h3>
                        <span class="timeline-meta">Q360 · abr. 2009 - nov. 2011</span>
                        <p>Desarrollo de aplicaciones web basadas en CMS y CRM.</p>
                    </div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-dot"></div>
                    <div class="timeline-content">
                        <h3>Analista de Sistemas</h3>
                        <span class="timeline-meta">Net Think Media · feb. 2007 - feb. 2009</span>
                        <p>Desarrollo de aplicaciones web basadas en CMS y CRM.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Skills Section -->
    <section id="skills" class="skills">
        <div class="container">
            <div class="section-header">
                <div class="section-badge">
                    <i class="fas fa-layer-group"></i>
                    <span>Habilidades</span>
                </div>
                <h2 class="section-title">Dominio técnico</h2>
                <p class="section-subtitle">De la mensajería de pagos a la infraestructura que la soporta</p>
            </div>
            <div class="skills-grid">
                <div class="skills-category">
                    <h3>Medios de Pago</h3>
                    <div class="skills-tags">
                        <span class="skill-tag">ISO 8583</span>
                        <span class="skill-tag">Mastercard</span>
                        <span class="skill-tag">Tokenización</span>
                        <span class="skill-tag">PCI DSS</span>
                        <span class="skill-tag">Compensación y Liquidación</span>
                    </div>
                </div>
                <div class="skills-category">
                    <h3>Cloud</h3>
                    <div class="skills-tags">
                        <span class="skill-tag">AWS</span>
                        <span class="skill-tag">AWS CloudFormation</span>
                        <span class="skill-tag">Amazon EKS</span>
                    </div>
                </div>
                <div class="skills-category">
                    <h3>Desarrollo</h3>
                    <div class="skills-tags">
                        <span class="skill-tag">Node.js</span>
                        <span class="skill-tag">PHP</span>
                        <span class="skill-tag">Python</span>
                        <span class="skill-tag">SQL</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Certifications Section -->
    <section id="certifications" class="certifications">
        <div class="container">
            <div class="section-header">
                <div class="section-badge">
                    <i class="fas fa-certificate"></i>
                    <span>Certificaciones</span>
                </div>
                <h2 class="section-title">Formación certificada</h2>
                <p class="section-subtitle">Validación formal de conocimientos en la nube</p>
            </div>
            <div class="certifications-grid">
                <div class="cert-card">
                    <div class="cert-icon">
                        <i class="fab fa-aws"></i>
                    </div>
                    <div class="cert-info">
                        <h3>AWS Certified AI Practitioner</h3>
                        <p>Amazon Web Services Training and Certification</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Projects Section (oculta temporalmente: se irán agregando proyectos poco a poco)
    <section id="projects" class="projects">
        <div class="container">
            <div class="section-header">
                <div class="section-badge">
                    <i class="fas fa-project-diagram"></i>
                    <span>Proyectos</span>
                </div>
                <h2 class="section-title">Soluciones que transforman negocios</h2>
                <p class="section-subtitle">Proyectos destacados que demuestran mi experiencia en cloud y DevOps</p>
            </div>
            <div class="projects-filter">
                <button class="filter-btn active" data-filter="all">
                    <i class="fas fa-th"></i>
                    Todos
                </button>
                <button class="filter-btn" data-filter="Básico">
                    <i class="fas fa-star"></i>
                    Básico
                </button>
                <button class="filter-btn" data-filter="Avanzado">
                    <i class="fas fa-rocket"></i>
                    Avanzado
                </button>
                <button class="filter-btn" data-filter="Profesional">
                    <i class="fas fa-crown"></i>
                    Profesional
                </button>
            </div>
            <div class="projects-grid" id="projectsGrid">
                <!-- Projects will be loaded by JavaScript -->
            </div>
        </div>
    </section>
    -->

    <!-- Contact Section -->
    <section id="contact" class="contact">
        <div class="container">
            <div class="section-header">
                <div class="section-badge">
                    <i class="fas fa-envelope"></i>
                    <span>Contacto</span>
                </div>
                <h2 class="section-title">Hablemos</h2>
                <p class="section-subtitle">Disponible para conversar sobre roles en sistemas de pago, fintech o cloud</p>
            </div>
            <div class="contact-content">
                <div class="contact-info">
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fas fa-envelope"></i>
                        </div>
                        <div class="contact-details">
                            <h3>Email</h3>
                            <p>rafael.williams@gmail.com</p>
                            <a href="mailto:rafael.williams@gmail.com" class="contact-link">Enviar Email</a>
                        </div>
                    </div>
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fab fa-linkedin"></i>
                        </div>
                        <div class="contact-details">
                            <h3>LinkedIn</h3>
                            <p>Conecta profesionalmente</p>
                            <a href="https://www.linkedin.com/in/rafael-williams-puerto-604a3521/" target="_blank" class="contact-link">Ver Perfil</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Project Modal -->
    <div id="projectModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 id="modalTitle"></h2>
            <span class="modal-close">&times;</span>
            </div>
            <div class="modal-body">
                <div class="modal-image-container">
                <img id="modalImage" src="" alt="" class="modal-image">
                </div>
                <div class="modal-info">
                    <p id="modalDescription"></p>
                    <div class="modal-technologies" id="modalTechnologies"></div>
                    <div class="modal-buttons">
                        <a id="modalGithub" href="#" target="_blank" class="btn btn-primary">
                            <i class="fab fa-github"></i>
                            Ver Código
                        </a>
                        <a id="modalDemo" href="#" target="_blank" class="btn btn-outline">
                            <i class="fas fa-external-link-alt"></i>
                            Ver Demo
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
        <div class="container">
            <div class="footer-content">
                <div class="footer-brand">
                    <p>Ingeniero de Software especializado en sistemas de pago y cloud.</p>
                </div>
                <div class="footer-links">
                    <div class="footer-section">
                        <h4>Navegación</h4>
                        <ul>
                            <li><a href="#hero">Inicio</a></li>
                            <li><a href="#experience">Experiencia</a></li>
                            <li><a href="#skills">Habilidades</a></li>
                            <li><a href="#certifications">Certificaciones</a></li>
                            <!-- <li><a href="#projects">Proyectos</a></li> -->
                        </ul>
                    </div>
                    <div class="footer-section">
                        <h4>Contacto</h4>
                        <ul>
                            <li><a href="mailto:rafael.williams@gmail.com">Email</a></li>
                            <li><a href="https://www.linkedin.com/in/rafael-williams-puerto-604a3521/" target="_blank">LinkedIn</a></li>
                        </ul>
                    </div>
                </div>
            </div>
            <div class="footer-bottom">
                <p>&copy; 2025 Rafael Williams | Todos los derechos reservados.</p>
            </div>
        </div>
    </footer>

    <script src="projects-config.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify the HTML is well-formed and complete**

Run:
```bash
node -e "const fs=require('fs');const h=fs.readFileSync('C:/apps/Portfolio/index.html','utf8');const opens=(h.match(/<!--/g)||[]).length;const closes=(h.match(/-->/g)||[]).length;console.log('comment balance (opens, closes):', opens, closes);['hero','experience','skills','certifications','contact'].forEach(id => console.log(id, h.includes('id=\"'+id+'\"')));console.log('projects still commented:', h.includes('<!-- Projects Section') && h.includes('id=\"projects\"'));"
```
Expected output: `comment balance (opens, closes): 10 10` (Step 1's HTML has 10 separate comments: the `<!-- Hero Section -->` / `<!-- Experience Section -->` / `<!-- Skills Section -->` / `<!-- Certifications Section -->` / `<!-- Contact Section -->` / `<!-- Project Modal -->` / `<!-- Footer -->` section labels, the Projects section's open/close pair, the nested `<!-- Projects will be loaded by JavaScript -->` inside it, and the commented-out `<!-- <li><a href="#projects">...</a></li> -->` footer nav item — all balanced is what matters, not the exact number; if opens ≠ closes, an HTML comment is unclosed somewhere and must be fixed before continuing), `true` printed for all five ids (`hero`, `experience`, `skills`, `certifications`, `contact`), and `projects still commented: true`.

If Node isn't available, open the file in a browser instead (see Task 3) and use `read_page` or `get_page_text` to confirm the rendered DOM only contains the five expected sections (the browser parser naturally drops HTML comments, so a commented `<section id="projects">` won't show up there).

- [ ] **Step 3: Commit**

```bash
cd C:\apps\Portfolio
git add index.html
git commit -m "Restructure index.html for Navy & Gold Premium redesign

Simplify hero to centered layout (drop particles/floating shapes/code
card), add Experience/Skills/Certifications sections with real
LinkedIn content, update footer nav and Contact copy. Projects section
stays commented out; script.js-dependent ids/classes are unchanged."
```

---

### Task 2: Rewrite `style.css`

**Files:**
- Modify: `C:\apps\Portfolio\style.css` (full file replacement)

This is the "Navy & Gold Premium" design system: navy `#0A1F44` / ivory `#FAFAF8` / gold `#C9A24B` palette, Georgia serif for headings, Inter sans-serif for body text, no particle/gradient/shimmer animation. All class names that `script.js` depends on (listed in "Before you start") are preserved — only their colors/fonts/spacing change.

- [ ] **Step 1: Replace the entire contents of `style.css`**

```css
/* Reset and Base Styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root {
    /* Colors - Navy & Gold Premium */
    --color-navy: #0A1F44;
    --color-navy-rgb: 10, 31, 68;
    --color-navy-light: #12295A;
    --color-ivory: #FAFAF8;
    --color-white: #FFFFFF;
    --color-gold: #C9A24B;
    --color-gold-dark: #A9843A;
    --color-text-on-navy: #FAFAF8;
    --color-text-muted-on-navy: #9AA5C0;
    --color-text-on-ivory: #0A1F44;
    --color-text-muted-on-ivory: #5A6478;

    /* Shadows (navy-tinted) */
    --shadow-sm: 0 1px 2px 0 rgba(var(--color-navy-rgb), 0.08);
    --shadow-md: 0 4px 6px -1px rgba(var(--color-navy-rgb), 0.12), 0 2px 4px -1px rgba(var(--color-navy-rgb), 0.08);
    --shadow-lg: 0 10px 15px -3px rgba(var(--color-navy-rgb), 0.12), 0 4px 6px -2px rgba(var(--color-navy-rgb), 0.08);
    --shadow-xl: 0 20px 25px -5px rgba(var(--color-navy-rgb), 0.15), 0 10px 10px -5px rgba(var(--color-navy-rgb), 0.08);
    --shadow-2xl: 0 25px 50px -12px rgba(var(--color-navy-rgb), 0.3);

    /* Border Radius */
    --radius-sm: 0.375rem;
    --radius-md: 0.5rem;
    --radius-lg: 0.75rem;
    --radius-xl: 1rem;
    --radius-2xl: 1.5rem;

    /* Spacing */
    --space-xs: 0.5rem;
    --space-sm: 1rem;
    --space-md: 1.5rem;
    --space-lg: 2rem;
    --space-xl: 3rem;
    --space-2xl: 4rem;
    --space-3xl: 6rem;

    /* Typography */
    --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    --font-serif: Georgia, 'Times New Roman', serif;
    --font-size-xs: 0.75rem;
    --font-size-sm: 0.875rem;
    --font-size-base: 1rem;
    --font-size-lg: 1.125rem;
    --font-size-xl: 1.25rem;
    --font-size-2xl: 1.5rem;
    --font-size-3xl: 1.875rem;
    --font-size-4xl: 2.25rem;
    --font-size-5xl: 3rem;
    --font-size-6xl: 3.75rem;

    /* Transitions */
    --transition-fast: 0.15s ease;
    --transition-normal: 0.3s ease;
    --transition-slow: 0.5s ease;
}

/* Base Styles */
html {
    scroll-behavior: smooth;
}

body {
    font-family: var(--font-sans);
    background: var(--color-ivory);
    color: var(--color-text-on-ivory);
    line-height: 1.6;
    overflow-x: hidden;
    font-size: var(--font-size-base);
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 var(--space-lg);
}

/* Typography */
h1, h2, h3, h4, h5, h6 {
    font-family: var(--font-serif);
    font-weight: 700;
    line-height: 1.2;
    margin-bottom: var(--space-sm);
    color: var(--color-text-on-ivory);
}

h1 { font-size: var(--font-size-5xl); }
h2 { font-size: var(--font-size-4xl); }
h3 { font-size: var(--font-size-3xl); }
h4 { font-size: var(--font-size-2xl); }
h5 { font-size: var(--font-size-xl); }
h6 { font-size: var(--font-size-lg); }

p {
    margin-bottom: var(--space-sm);
    color: var(--color-text-muted-on-ivory);
}

/* Buttons */
.btn {
    display: inline-flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-sm) var(--space-lg);
    border: 2px solid transparent;
    border-radius: var(--radius-md);
    font-family: var(--font-sans);
    font-weight: 600;
    font-size: var(--font-size-sm);
    text-decoration: none;
    cursor: pointer;
    transition: all var(--transition-normal);
    white-space: nowrap;
}

.btn-primary {
    background: var(--color-gold);
    color: var(--color-navy);
    border-color: var(--color-gold);
}

.btn-primary:hover {
    background: var(--color-gold-dark);
    border-color: var(--color-gold-dark);
    transform: translateY(-2px);
    box-shadow: var(--shadow-md);
}

.btn-outline {
    background: transparent;
    color: var(--color-navy);
    border-color: var(--color-navy);
}

.btn-outline:hover {
    background: rgba(var(--color-navy-rgb), 0.06);
    transform: translateY(-2px);
}

.hero-buttons .btn-outline {
    color: var(--color-text-on-navy);
    border-color: var(--color-text-on-navy);
}

.hero-buttons .btn-outline:hover {
    background: rgba(250, 250, 248, 0.1);
}

.btn-lg {
    padding: var(--space-md) var(--space-xl);
    font-size: var(--font-size-base);
}

/* Hero Section */
.hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    background: var(--color-navy);
    text-align: center;
    padding: var(--space-3xl) 0;
}

.hero-content {
    max-width: 800px;
    margin: 0 auto;
    padding: 0 var(--space-lg);
    position: relative;
    z-index: 2;
}

.hero-badge {
    display: inline-flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-xs) var(--space-md);
    background: transparent;
    border: 1px solid var(--color-gold);
    border-radius: var(--radius-xl);
    color: var(--color-gold);
    font-family: var(--font-sans);
    font-size: var(--font-size-xs);
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: var(--space-lg);
}

.hero-title {
    font-family: var(--font-serif);
    font-size: var(--font-size-6xl);
    font-weight: 700;
    margin-bottom: var(--space-lg);
    line-height: 1.15;
    color: var(--color-text-on-navy);
}

.hero-description {
    font-family: var(--font-sans);
    font-size: var(--font-size-lg);
    color: var(--color-text-muted-on-navy);
    margin-bottom: var(--space-xl);
    line-height: 1.7;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
}

.hero-buttons {
    display: flex;
    gap: var(--space-md);
    justify-content: center;
    margin-bottom: var(--space-xl);
}

.scroll-indicator {
    position: absolute;
    bottom: var(--space-xl);
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
    color: var(--color-text-muted-on-navy);
    cursor: pointer;
    transition: all var(--transition-normal);
}

.scroll-indicator:hover {
    color: var(--color-text-on-navy);
    transform: translateX(-50%) translateY(-5px);
}

.scroll-text {
    font-family: var(--font-sans);
    font-size: var(--font-size-sm);
    margin-bottom: var(--space-xs);
}

.scroll-arrow {
    animation: bounce 2s infinite;
}

/* Section Headers (shared by Experience / Skills / Certifications / Projects / Contact) */
.section-header {
    text-align: center;
    margin-bottom: var(--space-3xl);
}

.section-badge {
    display: inline-flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-xs) var(--space-md);
    background: var(--color-navy);
    color: var(--color-text-on-navy);
    border-radius: var(--radius-xl);
    font-family: var(--font-sans);
    font-size: var(--font-size-sm);
    font-weight: 600;
    margin-bottom: var(--space-md);
}

.section-title {
    font-family: var(--font-serif);
    font-size: var(--font-size-4xl);
    font-weight: 700;
    color: var(--color-text-on-ivory);
    margin-bottom: var(--space-md);
}

.section-subtitle {
    font-family: var(--font-sans);
    font-size: var(--font-size-lg);
    color: var(--color-text-muted-on-ivory);
    max-width: 600px;
    margin: 0 auto;
}

/* Experience Section */
.experience {
    padding: var(--space-3xl) 0;
    background: var(--color-ivory);
}

.experience-timeline {
    position: relative;
    max-width: 800px;
    margin: 0 auto;
    padding-left: var(--space-xl);
}

.experience-timeline::before {
    content: '';
    position: absolute;
    left: 4px;
    top: 6px;
    bottom: 6px;
    width: 2px;
    background: var(--color-gold);
    opacity: 0.4;
}

.timeline-item {
    position: relative;
    margin-bottom: var(--space-xl);
}

.timeline-item:last-child {
    margin-bottom: 0;
}

.timeline-dot {
    position: absolute;
    left: calc(-1 * var(--space-xl));
    top: 6px;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: var(--color-navy);
    box-shadow: 0 0 0 2px var(--color-navy);
}

.timeline-item.timeline-current .timeline-dot {
    background: var(--color-gold);
    box-shadow: 0 0 0 2px var(--color-gold);
}

.timeline-content h3 {
    font-family: var(--font-serif);
    font-size: var(--font-size-lg);
    font-weight: 700;
    color: var(--color-text-on-ivory);
    margin-bottom: 2px;
}

.timeline-meta {
    display: block;
    font-family: var(--font-sans);
    font-size: var(--font-size-sm);
    font-weight: 600;
    color: var(--color-gold-dark);
    margin-bottom: var(--space-xs);
}

.timeline-content p {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-ivory);
    font-size: var(--font-size-sm);
    margin-bottom: 0;
}

/* Skills Section */
.skills {
    padding: var(--space-3xl) 0;
    background: var(--color-white);
}

.skills-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--space-xl);
    max-width: 1000px;
    margin: 0 auto;
}

.skills-category h3 {
    font-family: var(--font-serif);
    font-size: var(--font-size-xl);
    color: var(--color-navy);
    margin-bottom: var(--space-md);
    text-align: center;
}

.skills-tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-xs);
    justify-content: center;
}

.skill-tag {
    font-family: var(--font-sans);
    font-size: var(--font-size-sm);
    font-weight: 500;
    color: var(--color-navy);
    background: var(--color-ivory);
    border: 1px solid rgba(var(--color-navy-rgb), 0.15);
    border-radius: var(--radius-xl);
    padding: var(--space-xs) var(--space-md);
}

/* Certifications Section */
.certifications {
    padding: var(--space-3xl) 0;
    background: var(--color-ivory);
}

.certifications-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: var(--space-lg);
}

.cert-card {
    display: flex;
    align-items: center;
    gap: var(--space-md);
    background: var(--color-white);
    border-left: 3px solid var(--color-gold);
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-md);
    padding: var(--space-md) var(--space-lg);
    max-width: 360px;
    transition: all var(--transition-normal);
}

.cert-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
}

.cert-icon {
    width: 48px;
    height: 48px;
    border-radius: var(--radius-md);
    background: var(--color-navy);
    color: var(--color-gold);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: var(--font-size-xl);
    flex-shrink: 0;
}

.cert-info h3 {
    font-family: var(--font-serif);
    font-size: var(--font-size-base);
    color: var(--color-text-on-ivory);
    margin-bottom: 2px;
}

.cert-info p {
    font-family: var(--font-sans);
    font-size: var(--font-size-xs);
    color: var(--color-text-muted-on-ivory);
    margin-bottom: 0;
}

/* Projects Section (hidden today; classes kept in sync with script.js) */
.projects {
    padding: var(--space-3xl) 0;
    background: var(--color-white);
}

.projects-filter {
    display: flex;
    justify-content: center;
    gap: var(--space-sm);
    margin-bottom: var(--space-3xl);
    flex-wrap: wrap;
}

.filter-btn {
    display: flex;
    align-items: center;
    gap: var(--space-xs);
    padding: var(--space-sm) var(--space-lg);
    background: transparent;
    border: 1px solid rgba(var(--color-navy-rgb), 0.2);
    border-radius: var(--radius-lg);
    color: var(--color-text-muted-on-ivory);
    font-family: var(--font-sans);
    font-weight: 500;
    cursor: pointer;
    transition: all var(--transition-normal);
}

.filter-btn:hover,
.filter-btn.active {
    background: var(--color-navy);
    color: var(--color-text-on-navy);
    border-color: var(--color-navy);
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: var(--space-xl);
}

.project-card {
    background: var(--color-white);
    border-radius: var(--radius-lg);
    overflow: hidden;
    box-shadow: var(--shadow-md);
    transition: all var(--transition-normal);
    cursor: pointer;
    border: 1px solid rgba(var(--color-navy-rgb), 0.08);
}

.project-card:hover {
    transform: translateY(-6px);
    box-shadow: var(--shadow-lg);
}

.project-image {
    width: 100%;
    height: 220px;
    object-fit: contain;
    background: var(--color-ivory);
}

.project-info {
    padding: var(--space-lg);
}

.project-title {
    font-family: var(--font-serif);
    font-size: var(--font-size-xl);
    color: var(--color-text-on-ivory);
    margin-bottom: var(--space-sm);
}

.project-description {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-ivory);
    margin-bottom: var(--space-md);
}

.project-tags {
    display: flex;
    gap: var(--space-xs);
    margin-bottom: var(--space-lg);
    flex-wrap: wrap;
}

.project-tag {
    padding: var(--space-xs) var(--space-sm);
    color: var(--color-navy);
    background: var(--color-ivory);
    border: 1px solid rgba(var(--color-navy-rgb), 0.15);
    border-radius: var(--radius-sm);
    font-family: var(--font-sans);
    font-size: var(--font-size-xs);
    font-weight: 600;
}

.project-tag[data-tag="Básico"] {
    color: #2E7D5B;
    border-color: #2E7D5B;
}

.project-tag[data-tag="Avanzado"] {
    color: var(--color-gold-dark);
    border-color: var(--color-gold-dark);
}

.project-tag[data-tag="Profesional"] {
    color: var(--color-navy);
    border-color: var(--color-navy);
    background: rgba(var(--color-navy-rgb), 0.06);
}

.project-links {
    display: flex;
    gap: var(--space-sm);
}

.project-link {
    flex: 1;
    text-align: center;
    padding: var(--space-sm);
    border-radius: var(--radius-md);
    text-decoration: none;
    font-family: var(--font-sans);
    font-weight: 600;
    font-size: var(--font-size-sm);
    transition: all var(--transition-normal);
}

.project-link.primary {
    background: var(--color-navy);
    color: var(--color-text-on-navy);
}

.project-link.secondary {
    background: transparent;
    color: var(--color-navy);
    border: 1px solid var(--color-navy);
}

.project-link:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-md);
}

/* Contact Section */
.contact {
    padding: var(--space-3xl) 0;
    background: var(--color-white);
}

.contact-content {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: var(--space-3xl);
}

.contact-info {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    gap: var(--space-lg);
}

.contact-card {
    background: var(--color-ivory);
    padding: var(--space-lg);
    border-radius: var(--radius-lg);
    border: 1px solid rgba(var(--color-navy-rgb), 0.08);
    transition: all var(--transition-normal);
    display: flex;
    align-items: center;
    gap: var(--space-lg);
}

.contact-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
}

.contact-icon {
    width: 50px;
    height: 50px;
    background: var(--color-navy);
    color: var(--color-gold);
    border-radius: var(--radius-md);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: var(--font-size-xl);
    flex-shrink: 0;
}

.contact-details h3 {
    font-family: var(--font-serif);
    font-size: var(--font-size-lg);
    color: var(--color-text-on-ivory);
    margin-bottom: var(--space-xs);
}

.contact-details p {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-ivory);
    margin-bottom: var(--space-sm);
}

.contact-link {
    font-family: var(--font-sans);
    color: var(--color-gold-dark);
    text-decoration: none;
    font-weight: 600;
    transition: color var(--transition-normal);
}

.contact-link:hover {
    color: var(--color-navy);
}

/* Modal */
.modal {
    display: none;
    position: fixed;
    z-index: 2000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(var(--color-navy-rgb), 0.6);
    backdrop-filter: blur(6px);
}

.modal-content {
    position: relative;
    background-color: var(--color-white);
    margin: 5% auto;
    padding: 0;
    border-radius: var(--radius-lg);
    width: 90%;
    max-width: 800px;
    max-height: 90vh;
    overflow-y: auto;
    box-shadow: var(--shadow-2xl);
    animation: modalFadeIn 0.3s ease;
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--space-lg);
    border-bottom: 1px solid rgba(var(--color-navy-rgb), 0.1);
}

.modal-header h2 {
    margin: 0;
    color: var(--color-text-on-ivory);
}

.modal-close {
    color: var(--color-text-muted-on-ivory);
    font-size: var(--font-size-2xl);
    font-weight: bold;
    cursor: pointer;
    transition: color var(--transition-normal);
}

.modal-close:hover {
    color: var(--color-navy);
}

.modal-body {
    padding: var(--space-lg);
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--space-xl);
}

.modal-image-container {
    border-radius: var(--radius-md);
    overflow: hidden;
}

.modal-image {
    width: 100%;
    height: 400px;
    object-fit: contain;
    background: var(--color-ivory);
    border-radius: var(--radius-md);
}

.modal-info p {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-ivory);
    line-height: 1.7;
    margin-bottom: var(--space-lg);
}

.modal-technologies {
    display: flex;
    gap: var(--space-xs);
    margin-bottom: var(--space-lg);
    flex-wrap: wrap;
}

.modal-tech-tag {
    padding: var(--space-xs) var(--space-sm);
    background: var(--color-navy);
    color: var(--color-text-on-navy);
    border-radius: var(--radius-sm);
    font-family: var(--font-sans);
    font-size: var(--font-size-xs);
    font-weight: 600;
}

.modal-buttons {
    display: flex;
    gap: var(--space-sm);
}

/* Footer */
.footer {
    background: var(--color-navy);
    color: var(--color-text-on-navy);
    padding: var(--space-3xl) 0 var(--space-xl);
}

.footer-content {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: var(--space-3xl);
    margin-bottom: var(--space-xl);
}

.footer-brand p {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-navy);
    line-height: 1.6;
}

.footer-links {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: var(--space-xl);
}

.footer-section h4 {
    font-family: var(--font-serif);
    color: var(--color-text-on-navy);
    margin-bottom: var(--space-md);
    font-size: var(--font-size-lg);
}

.footer-section ul {
    list-style: none;
    padding: 0;
}

.footer-section ul li {
    margin-bottom: var(--space-xs);
}

.footer-section ul li a {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-navy);
    text-decoration: none;
    transition: color var(--transition-normal);
}

.footer-section ul li a:hover {
    color: var(--color-gold);
}

.footer-bottom {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-top: var(--space-lg);
    border-top: 1px solid rgba(250, 250, 248, 0.15);
}

.footer-bottom p {
    font-family: var(--font-sans);
    color: var(--color-text-muted-on-navy);
    margin: 0;
}

/* Animations */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
    }
    40% {
        transform: translateY(-10px);
    }
    60% {
        transform: translateY(-5px);
    }
}

@keyframes modalFadeIn {
    from {
        opacity: 0;
        transform: scale(0.9);
    }
    to {
        opacity: 1;
        transform: scale(1);
    }
}

.fade-in {
    animation: fadeInUp 1s ease forwards;
}

.animate-in {
    animation: fadeInUp 0.6s ease forwards;
}

/* Responsive Design */
@media (max-width: 1024px) {
    .skills-grid {
        grid-template-columns: repeat(2, 1fr);
    }

    .contact-content {
        flex-direction: column;
        gap: var(--space-2xl);
    }

    .modal-body {
        grid-template-columns: 1fr;
    }

    .projects-grid {
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    }
}

@media (max-width: 768px) {
    .hero-title {
        font-size: var(--font-size-4xl);
    }

    .hero-description {
        font-size: var(--font-size-base);
    }

    .hero-buttons {
        flex-direction: column;
        align-items: center;
    }

    .section-title {
        font-size: var(--font-size-3xl);
    }

    .skills-grid {
        grid-template-columns: 1fr;
    }

    .certifications-grid {
        flex-direction: column;
        align-items: center;
    }

    .cert-card {
        max-width: 100%;
        width: 100%;
    }

    .experience-timeline {
        padding-left: var(--space-lg);
    }

    .timeline-dot {
        left: calc(-1 * var(--space-lg));
    }

    .footer-content {
        grid-template-columns: 1fr;
        text-align: center;
    }

    .footer-links {
        grid-template-columns: 1fr;
    }

    .footer-bottom {
        flex-direction: column;
        gap: var(--space-md);
        text-align: center;
    }

    .contact-info {
        flex-direction: column;
    }

    .contact-card {
        flex-direction: column;
        text-align: center;
    }

    .modal-content {
        width: 95%;
        margin: 10% auto;
    }

    .modal-image {
        height: 250px;
    }

    .project-image {
        height: 180px;
    }
}

@media (max-width: 480px) {
    .hero-title {
        font-size: var(--font-size-2xl);
    }

    .hero-description {
        font-size: var(--font-size-sm);
    }

    .section-title {
        font-size: var(--font-size-2xl);
    }

    .section-subtitle {
        font-size: var(--font-size-base);
    }

    .btn {
        padding: var(--space-sm) var(--space-lg);
        font-size: var(--font-size-sm);
    }

    .cert-card {
        flex-direction: column;
        text-align: center;
    }

    .timeline-content h3 {
        font-size: var(--font-size-base);
    }

    .project-image {
        height: 160px;
    }
}
```

- [ ] **Step 2: Commit**

```bash
cd C:\apps\Portfolio
git add style.css
git commit -m "Rewrite style.css with Navy & Gold Premium design system

New navy/ivory/gold palette, Georgia serif + Inter sans typography,
no particle/gradient/shimmer animation. All class names script.js
depends on are preserved, only restyled."
```

---

### Task 3: Visual QA pass

**Files:** none modified — this task only verifies Tasks 1–2 by rendering the real page.

There's no automated test suite for this static site, so "testing" here means: serve the file locally, look at it in the browser at multiple viewport widths, and check the JS console for errors. Do every one of these checks — don't skip to the commit.

- [ ] **Step 1: Serve the site locally**

```bash
cd C:\apps\Portfolio
python -m http.server 8793 &
```

(Use a port not already in use; check with `curl -s -o /dev/null -w "%{http_code}" http://localhost:8793/index.html` — expect `200`.)

- [ ] **Step 2: Desktop screenshot pass**

Using the claude-in-chrome tools (`navigate` to `http://localhost:8793/index.html`, then `computer` with `action: screenshot`), capture and visually confirm each section at desktop width (~1568px):
  - Hero: solid navy background (no particles/floating shapes/gradient shimmer), gold outline badge, "Rafael Williams Puerto" in serif, description, two buttons (gold filled "Contactar", outlined "Ver Experiencia")
  - Experience: ivory background, vertical gold timeline line with 6 items, Banistmo item's dot is gold-filled (current role), the other 5 are navy-filled
  - Skills: white background, 3 columns (Medios de Pago / Cloud / Desarrollo) of pill tags
  - Certifications: ivory background, one card with AWS icon, "AWS Certified AI Practitioner"
  - Contact: white background, two cards (Email, LinkedIn)
  - Footer: navy background, nav links Inicio/Experiencia/Habilidades/Certificaciones

Expected: no leftover blue/green/purple/amber colors from the old palette should be visible anywhere (only navy, ivory, white, gold appear).

- [ ] **Step 3: Console error check**

Using `read_console_messages` on the same tab, confirm there are no JS errors. `script.js` should log `Loading projects... 9 projects found` and `Filtered projects: 9` (from `loadProjects()`, which still runs on `DOMContentLoaded` even though the Projects section is hidden — that's expected and not an error, just means `projectsGrid` is null and `loadProjects()` returns early after logging).

- [ ] **Step 4: Mobile viewport pass**

Using `resize_window` (or the browser's device toolbar) to 375px wide, re-screenshot the Hero and Footer. Expected: hero buttons stack vertically, footer columns stack and center, no horizontal scrollbar anywhere on the page.

- [ ] **Step 5: Verify the Projects/Modal CSS without touching the committed HTML**

The Projects section must stay commented out in the committed `index.html`, but you still need to confirm its CSS actually works before it's needed later. Do this via the browser console so no file changes are involved:

Using `javascript_tool` (or the browser devtools console) on the running local page, run:

```js
document.querySelector('.projects-grid') === null
```
Expected: `true` (confirms the section is indeed not in the DOM — i.e., still commented out).

Then temporarily inject a single sample project card to sanity-check the restyled `.project-card`/`.project-tag`/`.project-link` CSS renders correctly:

```js
const grid = document.createElement('div');
grid.className = 'projects-grid';
grid.style.maxWidth = '400px';
grid.style.margin = '40px auto';
grid.innerHTML = `<div class="project-card">
  <img class="project-image" src="imagenes/AWS_Organization.jpg" alt="test">
  <div class="project-info">
    <h3 class="project-title">Organización en el Cloud</h3>
    <p class="project-description">Test description</p>
    <div class="project-tags"><span class="project-tag" data-tag="Profesional">Profesional</span></div>
    <div class="project-links">
      <a href="#" class="project-link primary">Ver Código</a>
      <a href="#" class="project-link secondary">Ver Demo</a>
    </div>
  </div>
</div>`;
document.body.appendChild(grid);
```
Screenshot the result, confirm the card matches the navy/gold/ivory system (navy "Ver Código" button, outlined navy "Ver Demo" button, ivory tag pill), then remove it:
```js
grid.remove();
```
This only ran in the live DOM of your local preview tab — it does not touch `index.html` on disk, so there's nothing to revert in the file.

- [ ] **Step 6: Stop the local server**

```bash
kill %1
```
(or find and stop the `python -m http.server 8793` process another way if job control isn't available)

- [ ] **Step 7: Fix anything wrong, then re-run Steps 2–6**

If any check in Steps 2–5 fails (wrong colors, layout broken, console errors, horizontal scroll on mobile), fix the specific rule in `style.css` (or markup in `index.html` if it's structural) and re-run the checks. Once everything passes, amend the Task 1/2 commits is not necessary — just commit the fix separately:

```bash
cd C:\apps\Portfolio
git add index.html style.css
git commit -m "Fix visual QA issues found in redesign review"
```
(Skip this step entirely if Steps 2–5 passed cleanly the first time.)

---

### Task 4: Update the design spec status (documentation)

**Files:**
- Modify: `docs/superpowers/specs/2026-08-11-navy-gold-redesign-design.md`

- [ ] **Step 1: Add an "Implementation status" line**

At the very top of the file, right after the `# Rediseño del portfolio: "Navy & Gold Premium"` heading, add:

```markdown

**Estado:** Implementado (índice, hoja de estilos, secciones Experiencia/Habilidades/Certificaciones). Pendiente: reactivar y poblar la sección de Proyectos cuando el usuario lo pida.
```

- [ ] **Step 2: Commit**

```bash
cd C:\apps\Portfolio
git add docs/superpowers/specs/2026-08-11-navy-gold-redesign-design.md
git commit -m "Mark Navy & Gold Premium spec as implemented"
```

---

## Stop here — do not push

All four tasks end in local commits only. Do **not** run `git push`. Every push to `rafawilliams.github.io` so far in this project has required the user to explicitly say "yes" in chat first (the site is live and publishing to it is a visible, shared-state action) — ask before pushing once this plan is fully executed and QA'd.

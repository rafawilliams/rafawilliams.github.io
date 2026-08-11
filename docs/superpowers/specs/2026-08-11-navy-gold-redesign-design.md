# Rediseño del portfolio: "Navy & Gold Premium"

## Contexto

El portfolio en `C:\apps\Portfolio` (publicado en `rafawilliams.github.io`) partió de una plantilla genérica de "Cloud Engineer" con hero animado (partículas, gradientes, formas flotantes). Ya se personalizó el contenido (nombre, contacto, proyectos AWS reales) y se corrigió el hero para reflejar el perfil real de Rafael: **Ingeniero de Software / Payment Systems Specialist en Banistmo**, con 10+ años de experiencia y foco en sistemas de pago (ISO 8583, Mastercard, tokenización, PCI DSS) más infraestructura AWS.

El diseño visual, sin embargo, seguía siendo el de una plantilla SaaS genérica. Este documento define un rediseño completo del look & feel para que el sitio se sienta acorde a un perfil de banca/fintech corporativo, y añade las secciones de Experiencia, Habilidades y Certificaciones que hoy no existen.

La sección de Proyectos permanece oculta (comentada en HTML) por decisión explícita del usuario — se irá reactivando/poblando incrementalmente más adelante y no es parte de este rediseño.

## Fuente de contenido real

Todo el contenido nuevo (experiencia, bio, skills) sale directamente del perfil de LinkedIn del usuario (`linkedin.com/in/rafael-williams-puerto-604a3521`), verificado en sesión de navegador:

**Bio (Acerca de):**
> En Banistmo, lidero el diseño e implementación de soluciones tecnológicas optimizadas para garantizar niveles óptimos de disponibilidad y usabilidad. Mi enfoque se centra en la generación de valor incremental mientras colaboro con proveedores para asegurar entregas eficientes y alineadas con las metas de tiempo, calidad y costo. Con más de una década en el desarrollo de software, domino herramientas modernas de programación web e infraestructura tecnológica.

**Experiencia (orden cronológico inverso):**
1. **Ingeniero de Software** — Banistmo · jun. 2020 - actualidad (Panamá, Híbrido)
2. **Senior Software Engineer** — Pensanomica · ago. 2015 - jun. 2020
3. **Senior Developer** — Admios · ago. 2012 - jun. 2015
4. **Analista de Sistema** — Pensanomica · sept. 2011 - ago. 2012
5. **Analista de Sistemas y Programador** — Q360 · abr. 2009 - nov. 2011
6. **Analista de sistemas** — Net Think Media · feb. 2007 - feb. 2009

**Habilidades (agrupadas por categoría):**
- Medios de Pago: ISO 8583, Mastercard, Tokenización, PCI DSS, Compensación y Liquidación
- Cloud: AWS, AWS CloudFormation, Amazon EKS
- Desarrollo: Node.js, PHP, Python, SQL

**Certificaciones:**
- AWS Certified AI Practitioner (AIF-C01) — Amazon Web Services Training and Certification

## Sistema visual

| Token | Valor | Uso |
|---|---|---|
| `--navy` | `#0A1F44` | Fondo del hero, texto sobre fondo claro en headings |
| `--navy-light` | `#12295A` | Tarjetas/paneles sobre fondo navy |
| `--ivory` | `#FAFAF8` | Fondo de secciones de contenido |
| `--gold` | `#C9A24B` | Acentos, CTAs, bordes de énfasis, iconografía de marca |
| `--text-muted-on-navy` | `#9AA5C0` | Texto secundario sobre fondo navy |
| `--text-muted-on-ivory` | `#5A6478` | Texto secundario sobre fondo ivory |

**Tipografía:**
- Headings (nombre, títulos de sección, títulos de experiencia): Georgia u otra serif de sistema — transmite tono editorial/banca privada
- Cuerpo de texto: Inter (ya cargada vía Google Fonts) — se mantiene igual que hoy
- Sin fuente monoespaciada ni efectos de "código" (eso pertenece al diseño anterior)

**Principios de estilo:**
- Sin partículas animadas, gradientes llamativos ni formas flotantes — fondo sólido navy en el hero
- Botones: relleno gold sobre navy para la acción primaria; borde fino (outline) para la secundaria
- Tarjetas con sombra sutil, borde izquierdo en gold para destacar (usado en experiencia y certificaciones)
- Iconografía existente de Font Awesome se conserva, recoloreada a la nueva paleta

## Estructura de secciones (orden final)

1. **Hero** (layout "centrado clásico", ya validado)
   - Badge outline gold: "Payment Systems Specialist"
   - Nombre en serif, grande
   - Tagline/descripción con la bio real resumida
   - Botones: "Contactar" (relleno gold) / "Ver Experiencia" (outline)
2. **Experiencia** — timeline vertical (línea + puntos), un ítem por rol, con empresa, rango de fechas y descripción corta
3. **Habilidades** — grid de tags/pills agrupados en 3 columnas por categoría (Medios de Pago / Cloud / Desarrollo)
4. **Certificaciones** — fila de badges (ícono + nombre + emisor + link), empezando con AWS Certified AI Practitioner
5. **Proyectos** — permanece comentada/oculta, sin cambios de contenido; solo se asegura que las clases CSS que usa (`.projects`, `.project-card`, `.projects-filter`, etc.) se migren al nuevo sistema de diseño para que al reactivarla combine visualmente sin trabajo adicional
6. **Contacto** — mismas dos tarjetas (Email, LinkedIn), re-estilizadas a la nueva paleta
7. **Footer** — mismo contenido y enlaces (con "Proyectos" comentado como ya está), re-estilizado

## Alcance técnico

- Sitio 100% estático (HTML/CSS/JS sin build tools), compatible con GitHub Pages — sin cambios en este aspecto
- `style.css`: reescritura completa. El CSS actual está construido alrededor de la estética de partículas/gradientes y no es una base útil para el nuevo sistema
- `index.html`: se reestructuran las secciones (se añaden Experiencia, Habilidades, Certificaciones; se re-etiquetan clases donde haga falta), pero el `<head>`, meta tags y el bloque de Proyectos/Modal se conservan tal cual (comentados donde corresponde)
- `script.js`: sin cambios funcionales. La lógica de proyectos, modal, filtros y notificaciones sigue intacta; solo se ajustan selectores CSS si el nuevo HTML los requiere
- `projects-config.js`: sin cambios
- No se introducen nuevas dependencias ni frameworks — sigue siendo Font Awesome (CDN) + Google Fonts (CDN) + CSS/JS vanilla

## Fuera de alcance

- No se reactiva ni se pobla la sección de Proyectos
- No se añade sección "Sobre mí / Bio" separada (la bio vive dentro del Hero)
- No se cambia el flujo de publicación (GitHub Pages desde `main`) ni el dominio

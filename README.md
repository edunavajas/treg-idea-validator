# validar-idea

Skill de [Claude Code](https://claude.com/claude-code) que valida una idea de producto con **datos reales** antes de que programes nada: demanda en Reddit y Hacker News, competencia, reseñas negativas, quién hay detrás del competidor fuerte y volumen de búsqueda. Al final te da un veredicto: **CONSTRÚYELO, PIVOTA u OLVÍDALO**, con fuentes en cada afirmación.

Los datos los pone [Treg](https://treg.superdesign.dev) — *"OpenRouter, but for agent tools"*. Una sola conexión y el agente accede a miles de endpoints de datos (SEO, Reddit, YouTube, App Store, enriquecimiento de empresas, keyword volume…) **pagando por llamada**, al precio del proveedor, sin suscripciones y sin que ninguna clave de API toque tu máquina.

## Por qué no le preguntas a un agente "a pelo"

Si le preguntas "¿es buena idea esto?" te dirá que sí. No tiene datos, tiene opiniones — y opiniones para quedar bien contigo. Esta skill obliga al agente a mirar **fuera**: gente real quejándose del problema, productos que ya lo resuelven y lo que esos productos hacen mal.

## Requisitos

- Claude Code (también funciona en Cursor, opencode, Codex… — son Markdown plano)
- Una cuenta de Treg (nueva verificada = **$1.00 gratis** de saldo)

### 1. Instala Treg en Claude Code

```
/plugin marketplace add superdesigndev/treg
/plugin install treg@treg
```

En el primer uso, el propio agente te guía: instala la CLI (`curl -fsSL https://treg.to/install.sh | sh`), inicias sesión (`treg login`) y conecta las herramientas (`treg mcp install`). No pegas ninguna clave de API en ningún sitio: **las credenciales viven en el servidor de Treg**, y tu agente nunca las ve.

### 2. Instala esta skill

```bash
git clone https://github.com/edunavajas/validar-idea.git
mkdir -p ~/.claude/skills
cp -r validar-idea/skills/validar-idea ~/.claude/skills/
```

## Uso

```
/validar-idea Hytale Creators: una plataforma web para que los creadores de Hytale suban y compartan sus mapas, mods y contenido, con perfiles de creador y descargas. Público: jugadores y creadores de Hytale.
```

El agente no suelta una opinión: decide qué herramienta necesita para cada cosa (Reddit aquí, keyword volume allá, reseñas de la competencia aquí) y te enseña el informe completo.

### Qué comprueba

| Paso | Qué mira |
|------|----------|
| 1. La idea en una frase | Para quién es y qué problema resuelve |
| 2. Demanda | Gente con el problema en Reddit, HN y comunidades del nicho (no "qué buena idea": gente quejándose) |
| 3. Competencia | Qué existe en Google, Product Hunt, tiendas de apps y GitHub, y su tracción |
| 4. Huecos | Las reseñas de 1 estrella de la competencia: ahí están los huecos |
| 5. Quién hay detrás | Empresa, tamaño y financiación del competidor más fuerte |
| 6. Búsquedas | Volumen mensual de 3–5 keywords del problema |
| 7. Veredicto | Nota 0–10 y CONSTRÚYELO / PIVOTA / OLVÍDALO con motivo |
| 8. Coste | Llamadas hechas y coste exacto de la validación |

## ¿Cuánto cuesta?

Se paga por llamada, en céntimos, al precio del proveedor (Treg no añade margen; su modelo es el volumen con los vendors). Una validación completa suele quedar en **unas decenas de céntimos** — la skill trae un presupuesto duro (~30 llamadas) para que no se dispare. Antes y después puedes ver el saldo y el desglose:

```bash
treg balance   # saldo y últimos cargos
treg audit     # cada llamada: a qué fuente y cuánto costó
```

Para comparar: hacer esto a mano son horas y varias suscripciones de herramientas de datos (Semrush $139/mo, Crunchbase $99/mo…). Aquí pagas solo lo que el agente usa.

## Personalízala

La skill es un archivo de Markdown: `skills/validar-idea/SKILL.md`. El criterio es tuyo y vive ahí:

- **Presupuesto de llamadas**: sube o baja el límite de ~30 llamadas / $0.50 en la sección *"Cómo llamar a las herramientas"*.
- **Umbrales del veredicto**: ajusta la puntuación del paso 7 a tu barra de riesgo.
- **Fuentes**: añade o quita pasos (p. ej. análisis de anuncios, tendencias de YouTube).

## Skill vs. otras

- [elcodabra/validate-idea-skills](https://github.com/elcodabra/validate-idea-skills) — framework de validación en 8 pasos sin datos en vivo
- [Vimalk0703/startup-validator-skill](https://github.com/Vimalk0703/startup-validator-skill) — investigación multi-agente con búsqueda web
- [ruanxinyang/micro-saas-validator](https://github.com/ruanxinyang/micro-saas-validator) — GO/CAUTION/PASS con web search

Este repo se diferencia en una cosa: **datos de pago reales** (volumen de búsqueda, reseñas de tiendas de apps, financiación de empresas) que la búsqueda web genérica no da, a precio por llamada.

## Licencia

[MIT](LICENSE)

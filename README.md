# treg-idea-validator

Skill de Claude Code que valida una idea de producto con **datos reales** antes de que programes nada: demanda en Reddit y Hacker News, competencia, reseñas de 1 estrella de la competencia y volumen de búsqueda. Al final, un veredicto: **CONSTRÚYELO, PIVOTA u OLVÍDALO** — con fuentes en cada afirmación.

Los datos los pone [**Treg**](https://treg.superdesign.dev) — *"OpenRouter, but for agent tools"*. Una conexión y tu agente accede a miles de endpoints de datos (Reddit, YouTube, App Store, keyword volume, enriquecimiento de empresas…) **pagando por llamada**, al precio del proveedor, sin suscripciones y sin que ninguna clave de API toque tu máquina.

## Instalar

**1. Treg en Claude Code** — dos comandos:

```
/plugin marketplace add superdesigndev/treg
/plugin install treg@treg
```

En el primer uso, el propio agente te guía con el resto: instala la CLI (`curl -fsSL https://treg.to/install.sh | sh`), inicias sesión (`treg login`) y conecta las herramientas (`treg mcp install`). Cuenta nueva verificada = **$1.00 gratis** de saldo.

**2. Esta skill**:

```bash
git clone https://github.com/edunavajas/treg-idea-validator.git
mkdir -p ~/.claude/skills
cp -r treg-idea-validator/skills/validar-idea ~/.claude/skills/
```

Funciona también en otros agentes — mira [OpenCode](#usar-en-opencode) abajo; en Cursor o Codex, copia la carpeta en su carpeta de skills.

### Usar en OpenCode

**1. Treg en OpenCode** — aquí no hay comandos `/plugin`; se hace con la CLI:

```bash
curl -fsSL https://treg.to/install.sh | sh   # instala la CLI (Python 3.12+)
treg login                                    # inicia sesión (GitHub o email)
treg mcp install                              # registra el MCP de treg en opencode
```

Opcional: `npx skills add superdesigndev/treg -s treg` añade además la skill general de treg (encontrar y llamar cualquier herramienta del catálogo).

**2. Esta skill** — dos opciones:

```bash
# Global (todos tus proyectos)
mkdir -p ~/.config/opencode/skills
cp -r treg-idea-validator/skills/validar-idea ~/.config/opencode/skills/

# O solo para un proyecto
mkdir -p .opencode/skills
cp -r treg-idea-validator/skills/validar-idea .opencode/skills/
```

Atajo: si ya la instalaste en `~/.claude/skills/`, **OpenCode la detecta automáticamente** — también lee esa carpeta.

**3. Úsala**: la skill lleva `opencode/slash: "true"`, así que en la TUI tecleas `/validar-idea <tu idea>` y aparece como comando, igual que en Claude Code.

## Uso

```
/validar-idea Hytale Creators: una plataforma web para que los creadores de Hytale suban y compartan sus mapas, mods y contenido. Público: jugadores y creadores de Hytale.
```

El agente no opina: decide qué herramienta necesita para cada cosa (Reddit aquí, keyword volume allá, reseñas de la competencia acullá) y te entrega un informe con veredicto y nota.

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

Se paga por llamada, en céntimos, al precio del proveedor — [Treg](https://treg.superdesign.dev) no añade margen. Una validación completa suele quedar en **decenas de céntimos**, y la skill trae presupuesto duro (~30 llamadas) para que no se dispare:

```bash
treg balance   # saldo y últimos cargos
treg audit     # cada llamada: a qué fuente y cuánto costó
```

Para comparar: hacerlo a mano son horas y varias suscripciones mensuales (Semrush $139/mo, Crunchbase $99/mo…).

## Personalízala

Todo el criterio vive en `skills/validar-idea/SKILL.md`: sube o baja el presupuesto de llamadas, ajusta los umbrales del veredicto o añade/quita pasos (análisis de anuncios, tendencias de YouTube…).

## Créditos

- [Treg](https://treg.superdesign.dev) — los datos. Open source: [superdesigndev/treg](https://github.com/superdesigndev/treg)
- Inspirado en [elcodabra/validate-idea-skills](https://github.com/elcodabra/validate-idea-skills), [Vimalk0703/startup-validator-skill](https://github.com/Vimalk0703/startup-validator-skill) y [ruanxinyang/micro-saas-validator](https://github.com/ruanxinyang/micro-saas-validator) — este se diferencia en usar datos de pago reales, no solo web search.

## Licencia

[MIT](LICENSE)

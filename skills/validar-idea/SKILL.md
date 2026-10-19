---
name: validar-idea
description: Valida una idea de producto con datos reales antes de programarla. Úsala cuando el usuario escriba /validar-idea seguido de una idea.
metadata:
  opencode/slash: "true"
---

# Validar una idea de producto

Tu trabajo es decidir si esta idea merece que alguien gaste sus fines de semana en construirla.
No eres su amigo: si la idea es mala, dilo.

## Reglas
- Usa las herramientas de Treg para conseguir datos reales. Elige la herramienta según la tarea.
- Cada afirmación lleva su fuente (enlace o herramienta usada).
- Si no encuentras datos sobre algo, dilo. No rellenes huecos con suposiciones.
- No des la razón por defecto. El entusiasmo no es una señal; la gente con el problema sí.

## Antes de empezar
1. Comprueba el acceso a Treg con `treg balance`. Debe responder con el saldo del equipo.
2. Si no responde, PARA y pide al usuario que lo configure: `curl -fsSL https://treg.to/install.sh | sh` y luego `treg login`. No sigas sin datos reales, y sobre todo no inventes los que faltan.

## Cómo llamar a las herramientas (y no quemar el saldo)
- Pide la tarea, no la herramienta: `treg catalog search "<lo que quieres hacer>"` lista los endpoints que lo resuelven, con precio y fiabilidad real (`WORKS`).
- Antes de llamar por primera vez a un endpoint, mira su precio con `treg catalog get <id>`. Entre proveedores de la misma capacidad hay diferencias de hasta 200×.
- Prefiere las herramientas enrutadas (`treg.web.search`, `treg.google.serp.organic`, …): eligen proveedor solas — clave propia del equipo primero, luego el más barato fiable — y saltan a otra si una falla.
- NUNCA pegues claves de API de proveedores en comandos ni en config. Treg inyecta las credenciales en su servidor. Si el equipo ya tiene clave propia de un proveedor, esa gana automáticamente y esa llamada no gasta saldo.
- El coste real de cada llamada está en la cabecera `X-Treg-Cost-Micro` (microdólares), no en el cuerpo de la respuesta.
- Un 4xx es un problema de tu petición: arréglala. No la repitas contra otros proveedores (eso sí cobra).
- Un 429 o 503 del proveedor: prueba el siguiente proveedor de la misma capacidad y dile al usuario cuál usaste.
- Presupuesto: una validación completa cabe en ~30 llamadas. Si vas por la mitad del informe y ya has gastado más de $0.50, deja de buscar, di qué tienes y pasa al veredicto.
- Si Treg responde 402 (saldo agotado), PARA y muéstrale al usuario el `topup_url` que viene en la respuesta. No sigas con suposiciones.
- Repetir una búsqueda idéntica paga un 10% (caché), pero no repitas por repetir: cada consulta nueva cuesta precio completo.

## Pasos
1. **La idea en una frase.** Para quién es y qué problema resuelve. Si no cabe en una frase, señálalo.
2. **Demanda.** Busca en Reddit, Hacker News y comunidades del nicho a gente que tenga el problema (quejas, peticiones, "¿alguien conoce algo para…?"). Cuenta cuántas evidencias encuentras y lo recientes que son.
3. **Competencia.** Busca qué existe ya en Google, Product Hunt, App Store / Google Play y GitHub. Para cada competidor: qué hace, si está vivo y cuánta tracción parece tener.
4. **Huecos.** Lee las reseñas negativas de los competidores principales y resume lo que la gente echa en falta.
5. **Quién hay detrás.** Del competidor más fuerte: empresa, tamaño aproximado y si ha levantado financiación (enriquecimiento de personas/empresas del catálogo de Treg).
6. **Búsquedas.** Volumen de búsqueda mensual de 3 a 5 palabras clave del problema (endpoints de keyword volume del catálogo).
7. **Veredicto.** Puntúa de 0 a 10: demanda, competencia (10 = hueco claro), facilidad para llegar al público. Termina con uno de estos veredictos: CONSTRÚYELO, PIVOTA u OLVÍDALO, y el motivo principal en dos frases.
8. **Coste.** Indica cuántas llamadas has hecho y lo que ha costado la validación: `treg balance` (saldo y últimos cargos) y `treg audit` (log de llamadas con su coste). Si algún resultado te fue especialmente útil, valóralo con `treg review <call_id> useful`.

## Formato del informe
- Veredicto y nota arriba del todo.
- Una sección por paso, breve, con sus fuentes.
- Si el veredicto es PIVOTA, añade la alternativa que tendría más sentido según los datos.

# Precios de competencia · Electrodomésticos · Lima

Panel interno de Media Lab con los precios que cada tienda publica hoy en su web, la política de envío en Lima y un catálogo filtrable con exportación a CSV.

**URL:** pendiente de publicar (carpeta de desarrollo `electro-competencia/`).

## Estructura

| Archivo | Qué es |
|---|---|
| `index.html` | Panel completo (HTML + CSS + JS, sin dependencias). Carga `data.json` al abrirse. |
| `config.json` | Única fuente de verdad: industria, mercado, monedas, tramos, fuentes (con adaptador y segmento), políticas de envío, textos y exportación. |
| `data.json` | Salida del recolector: config embebida, resumen, fuentes, tipo de cambio y productos. |
| `scripts/actualizar.mjs` | Recolector diario. Node ≥ 20, sin dependencias externas. |
| `scripts/servir.mjs` | Servidor estático local para revisar el panel (`node scripts/servir.mjs`). |

Para replicar a otra industria: copia esta carpeta y edita `config.json` (no se modifica el motor ni el panel).

## Actualizar los datos

```bash
node scripts/actualizar.mjs
```

El script consulta las 9 tiendas por el marcado estructurado (schema.org JSON-LD) de su página de categoría. Las tiendas que no lo expongan se reportan como "sin catálogo de precios", sin inventar datos. Las tiendas que publican en dólares se convierten a soles con tipo de cambio vivo (open.er-api.com). Si una web falla, se conserva su última lectura buena marcándola como error.

## Fuentes y cobertura

| Tienda | Rol | Adaptador | Segmento leído |
|---|---|---|---|
| Hiraoka | competencia | jsonld-categoria | Refrigeradoras, lavadoras, cocinas y microondas |
| La Curacao | competencia | jsonld-categoria | Electrodomésticos y tecnología |
| Falabella | competencia | jsonld-categoria | Línea electro: climatización, lavado y cocina |
| Ripley | competencia | jsonld-categoria | Electrodomésticos y tecnología |
| Sodimac | competencia | jsonld-categoria | Electro hogar y herramientas |
| Promart | competencia | jsonld-categoria | Electrodomésticos y mejoras para el hogar |
| Oechsle | competencia | jsonld-categoria | Electrodomésticos y tecnología |
| Efe | competencia | jsonld-categoria | Electrodomésticos y televisores |
| Marcimex | competencia | jsonld-categoria | Electrodomésticos y línea blanca |

La plataforma de cada tienda se marca como "Web propia" hasta verificar su motor en las primeras lecturas. Los segmentos **no son categorías equivalentes entre sí**: cada tienda define el suyo.

## Estado y limitaciones (primera versión)

Este es el panel en construcción para electrodomésticos:

- Los adaptadores `jsonld-categoria` aún no se han corrido contra las tiendas reales: la lista de plataformas es una hipótesis y las categorías base están por confirmar según qué exponga cada web.
- Las políticas de envío se declaran "No observado" hasta transcribir cada página pública con cita.
- No hay tienda propia asignada aún; el panel compara solo competencia (el rol "propia" queda reservado para cuando se defina).

## Reglas de uso (AGENTS.md)

- Dato observado, inferencia y cálculo propio están etiquetados en el panel y en el diálogo de metodología.
- No citar una cifra sin verificar la ficha de origen: precios y stock cambian sin aviso.
- Uso interno; requiere aprobación antes de compartirse fuera del equipo.
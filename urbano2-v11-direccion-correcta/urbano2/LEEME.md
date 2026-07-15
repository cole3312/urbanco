# URBANO — Proyecto completo (v5)

Tienda web gratuita en Netlify: catálogo público con carrito y pedido por WhatsApp,
panel de inventario (/admin.html) y editor visual del diseño (/editor.html).

**Toda la documentación técnica está en `BRIEFING-MAESTRO-URBANO.md`** (raíz del repo):
arquitectura, API, seguridad, historial de errores y el paso a paso de despliegue.

## Despliegue rápido

1. Subir este repositorio a GitHub (todo en UN solo commit — cada deploy consume 15 créditos de Netlify).
2. Netlify → Add new site → Import from Git → rama `main`:
   - Base directory: `urbano2`
   - Build command: (vacío)
   - Publish directory: `urbano2/site`
   - Functions directory: `urbano2/netlify/functions`
3. Crear la variable de entorno `URBANO_CLAVE` (Site configuration → Environment variables)
   y disparar un deploy nuevo. Esa clave protege el admin y el editor.
4. Verificar: `/api/urbano/productos` → `[]` · `/api/urbano/diseno` → `{}` · `/admin.html` pide clave.
5. Cargar el inventario a mano o importando el CSV de respaldo desde el panel.

## Estructura

```
urbano2/
├── netlify.toml               <- configuración y rutas de la API (no tocar)
├── package.json               <- dependencias (no tocar)
├── netlify/functions/         <- productos, categorias, imagenes, diseno (.mjs)
└── site/                      <- index (tienda), admin (inventario), editor (diseño)
```

WhatsApp configurado: +57 314 596 0887 · Los datos (productos, imágenes, diseño)
viven en Netlify Blobs del sitio — respáldalos exportando el CSV desde /admin.html.

## Novedades v6 — Categorías, destacados y descuentos

- **Tienda pública:** menú "Categorías" en el header (estilo dropdown) + fila de
  chips filtrables sobre el catálogo. Al elegir una categoría, la grilla se filtra
  y hace scroll al catálogo.
- **Sección "Destacados y más vendidos":** aparece automáticamente arriba del
  catálogo apenas se abre la página, mostrando los productos marcados como
  "Nuevo", "Más vendido" o que tengan descuento activo. Si no hay ninguno
  marcado, la sección se oculta sola.
- **Descuentos por producto:** nuevo campo `descuento` (0-95%). La tienda
  muestra el precio tachado + el precio final, y ese precio final es el que
  se usa en el carrito y en el mensaje de WhatsApp.
- **"Más vendido":** nuevo checkbox en el admin, independiente de "destacado",
  para resaltar productos con la insignia 🔥 TOP VENTAS.
- Todo esto se administra desde `/admin.html` (mismo formulario de siempre,
  con los campos nuevos) y el Excel de exportación/importación ahora incluye
  las columnas `descuento` y `masVendido`.

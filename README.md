# SEGSOL — Sistema de Gestión de Personal

Aplicación web de un solo archivo (HTML + CSS + JS embebidos) para la gestión operativa de personal: postulantes, registro de personal, asistencia diaria por rutas, cuadre de caja, mapas y rutas, liquidaciones de inventario, descuentos del personal y kardex/stock de EPP.

No requiere backend ni servidor: todo corre en el navegador y los datos se guardan en `localStorage`, por lo que **cada navegador/dispositivo tiene su propia base de datos local**. No hay sincronización entre equipos.

## Cómo usarlo

1. Descarga `index.html`.
2. Ábrelo directamente en el navegador (doble clic), o publícalo con GitHub Pages / cualquier hosting estático.
3. Inicia sesión con el usuario administrador por defecto:
   - **Usuario:** `bryantt2`
   - **Contraseña:** `bryan_04`
4. Desde el panel de **Administración** puedes crear más usuarios y elegir qué módulos puede ver cada uno.

> ⚠️ El login es un control de acceso a nivel de interfaz, pensado para organizar el acceso del equipo, **no es seguridad real**: las contraseñas se guardan en texto plano en el propio archivo/`localStorage` y cualquier persona con acceso a las herramientas de desarrollador del navegador podría saltárselo. No lo uses para proteger información verdaderamente sensible.

## Publicar con GitHub Pages

1. Sube este repositorio a GitHub.
2. Ve a **Settings → Pages**.
3. En "Source" elige la rama principal (`main`) y la carpeta raíz (`/`).
4. GitHub Pages servirá automáticamente `index.html` en la URL que te asigne.

## Módulos incluidos

- **Registro de Postulantes** — seguimiento de postulantes a personal.
- **Registro de Personal** — ficha de cada colaborador (nombre, cargo, datos).
- **Asistencia del Personal** — asistencia diaria organizada por rutas y placas.
- **Cuadre de Caja** — control de caja diaria.
- **Mapas y Rutas** — importación y organización de rutas.
- **Liquidaciones y Descuentos**
  - Liquidación de inventario (importación de PDF).
  - Descuentos del Personal: catálogo de productos, registro de descuentos ligados a la asistencia del día (ruta/placa/puesto automáticos), con opción de dividir un descuento entre varias personas (cada una con su propio puesto autocompletado) y exportación a Excel.
- **Kardex de EPP** — historial de entregas y stock de equipos de protección personal, con alertas de stock mínimo.
- **Administración** — creación de usuarios y permisos por módulo.

## Importación/Exportación

Varios módulos permiten importar datos desde Excel (`.xlsx`) y exportar tablas a Excel, usando [SheetJS](https://sheetjs.com/) (incluido embebido en el archivo, sin dependencias externas en tiempo de ejecución).

## Estructura del repositorio

```
index.html   → aplicación completa (HTML + CSS + JS), autocontenida
```

## Notas técnicas

- Un único archivo HTML autocontenido: no hay build step, no hay dependencias externas que instalar.
- Persistencia con `localStorage` del navegador (sin backend, sin base de datos externa).
- Sin librerías de framework: JavaScript vanilla.

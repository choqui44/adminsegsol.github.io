# SEGSOL — Sistema de Gestión de Personal

Aplicación web de un solo archivo (HTML + CSS + JS embebidos) para la gestión operativa de personal: postulantes, registro de personal, asistencia diaria por rutas, cuadre de caja, mapas y rutas, liquidaciones de inventario, descuentos del personal y kardex/stock de EPP.

No requiere backend propio: todo corre en el navegador. Los datos se guardan en la nube (Firebase Firestore) para que **todas las computadoras vean los mismos datos en tiempo real**, con un respaldo local (`localStorage`) que permite seguir trabajando sin internet y ponerse al día automáticamente en cuanto vuelve la conexión.

## Cómo usarlo

1. Descarga `index.html`.
2. Ábrelo directamente en el navegador (doble clic), o publícalo con GitHub Pages / cualquier hosting estático (recomendado, ver más abajo).
3. Inicia sesión con el usuario administrador por defecto:
   - **Usuario:** `bryantt2`
   - **Contraseña:** `bryan_04`
4. Desde el panel de **Administración** puedes crear más usuarios y elegir qué módulos puede ver cada uno.
5. En la barra de navegación, junto al nombre del usuario, aparece un indicador de sincronización:
   - **☁️ Sincronizado** — conectado a la nube, los cambios se comparten en vivo con las demás computadoras.
   - **⚠️ Sin conexión con la nube** — sin internet, sin acceso a Firebase, o falta terminar la configuración (ver sección de solución de problemas); la app sigue funcionando con los últimos datos conocidos localmente.

> ⚠️ El login y la sincronización son controles pensados para organizar el acceso y el trabajo en equipo, **no son seguridad de nivel bancario**: las contraseñas se guardan en texto plano y cualquier persona con las credenciales del proyecto de Firebase (incluidas en este mismo archivo, como es normal en apps web) y algo de conocimiento técnico podría, en teoría, leer o escribir en la base de datos. Para el uso que le da un equipo interno con el archivo compartido de forma privada es un nivel de protección razonable, pero no lo uses para datos que no podrían filtrarse bajo ninguna circunstancia.

## Los datos NO viven en este archivo — por eso sobreviven a las actualizaciones

Toda la información que carga el equipo (personal, asistencia, descuentos, EPP, etc.) se guarda en Firebase Firestore, **no dentro del archivo HTML**. El archivo solo contiene el código de la aplicación, así que puedes reemplazar `index.html` por una versión nueva todas las veces que quieras y la información cargada no se borra, siempre que:

1. No cambies el bloque `firebaseConfig` dentro del archivo (busca `const firebaseConfig = {` cerca del inicio del script principal).
2. No cambies los nombres de colección/documento que usa el código (`segsol/registros`, `segsol/asistencias`, `segsol/usuarios`, etc.).

### Si sigues viendo "⚠️ Sin conexión con la nube"

1. Ve a **console.firebase.google.com** → proyecto `segsol-3b6b4` → **Authentication → Sign-in method** → confirma que **Anonymous** está **Habilitado**.
2. Prueba abriendo la app desde una URL real (GitHub Pages) en vez de abrir el archivo local con doble clic.
3. Revisa la consola del navegador (F12 → Console) para ver el error exacto si el problema persiste.

## Publicar con GitHub Pages (recomendado)

1. Sube este repositorio a GitHub.
2. Ve a **Settings → Pages**.
3. En "Source" elige la rama principal (`main`) y la carpeta raíz (`/`).
4. GitHub Pages servirá automáticamente `index.html` en la URL que te asigne (`https://tuusuario.github.io/turepo/`).
5. Usa siempre esa URL en vez del archivo local — es más estable para la sincronización en la nube.

## Módulos incluidos

- **Registro de Postulantes** — seguimiento de postulantes a personal.
- **Registro de Personal** — ficha de cada colaborador (nombre, cargo, datos).
- **Asistencia del Personal** — asistencia diaria organizada por rutas y placas, con:
  - **Récord de asistencia por persona**: matriz mensual (✓/F/J/T por día) exportable a Excel con diseño.
  - **Tareo por Placa**: qué placa usó cada colaborador cada día del mes, coloreada por placa para ver de un vistazo cómo están armadas las tripulaciones (domingos resaltados en rojo), exportable a Excel con diseño.
  - **Historial por colaborador**: detalle individual también exportable a Excel con diseño.
- **Cuadre de Caja** — control de caja diaria.
- **Mapas y Rutas** — importación y organización de rutas.
- **Liquidaciones y Descuentos**
  - Liquidación de inventario (importación de PDF).
  - Descuentos del Personal: catálogo de productos, registro de descuentos ligados a la asistencia del día (ruta/placa/puesto automáticos), con opción de dividir un descuento entre varias personas (cada una con su propio puesto autocompletado) y exportación a Excel.
- **Kardex de EPP** — historial de entregas y stock de equipos de protección personal, con alertas de stock mínimo.
- **Administración** — creación de usuarios y permisos por módulo.

## Importación/Exportación

Varios módulos permiten importar datos desde Excel (`.xlsx`) y exportar tablas a Excel. Las exportaciones simples usan [SheetJS](https://sheetjs.com/); las exportaciones con diseño (colores, encabezados resaltados, resumen) como el récord, el tareo por placa y el historial de asistencia usan [ExcelJS](https://github.com/exceljs/exceljs). Ambas están embebidas en el archivo.

## Estructura del repositorio

```
index.html   → aplicación completa (HTML + CSS + JS), autocontenida
```

## Notas técnicas

- Un único archivo HTML: no hay build step. Las únicas dependencias externas por red son las librerías de Firebase (cargadas desde su CDN oficial vía `<script src>`, necesarias para la sincronización en la nube); SheetJS, Chart.js y ExcelJS van embebidas directamente en el archivo.
- Persistencia dual: Firebase Firestore como base de datos compartida en tiempo real, con `localStorage` como caché/respaldo local para el modo sin conexión.
- Sin librerías de framework: JavaScript vanilla.

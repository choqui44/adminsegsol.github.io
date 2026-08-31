# SEGSOL — Sistema de Gestión de Personal

Aplicación web de un solo archivo (`index.html`) para gestionar personal, asistencia, pagos, caja chica, mapas de rutas, liquidaciones, descuentos y EPP. No necesita servidor ni instalación: se abre en el navegador y funciona.

## Cómo usarlo

1. Abre `index.html` en Chrome (recomendado) o cualquier navegador moderno. Para que la sincronización en la nube funcione, el archivo debe abrirse desde una dirección `https://` (por ejemplo, GitHub Pages) — ver más abajo.
2. Inicia sesión. Usuario administrador por defecto (se crea automáticamente la primera vez que se abre la app en un dispositivo sin datos):
   - Usuario: `bryantt2`
   - Contraseña: `bryan_04`
3. Cambia esa contraseña por defecto en cuanto puedas, desde **Administración → Cambiar contraseña**.

## Sincronización en la nube (Firebase / Firestore)

La app usa Firebase Firestore para que los cambios hechos en una computadora se reflejen en las demás en tiempo real. El estado de la conexión se muestra en la barra superior:

- ☁️ **Sincronizado** — todo funcionando, los cambios se comparten con las demás computadoras.
- ⏳ **Conectando…** — se está estableciendo la conexión (normal justo al abrir la página).
- ⚠️ **Sincronización en la nube no disponible en este navegador** — el SDK de Firebase no cargó (revisa tu conexión a internet, o si el navegador está bloqueando `gstatic.com`).
- ⚠️ **Sin conexión con la nube (trabajando solo en este dispositivo)** — Firebase cargó pero no se pudo autenticar. Revisa:
  1. Que el proveedor **Anonymous** esté habilitado en Firebase Console → Authentication → Sign-in method.
  2. Que estés abriendo la app desde una URL `https://` real (por ejemplo GitHub Pages), no con doble clic desde `file://` — algunos navegadores bloquean la autenticación en ese modo.

Sin conexión a la nube, la app sigue funcionando normalmente guardando todo en este navegador (localStorage); en cuanto vuelva la conexión, se sincroniza sola.

### Los datos NO viven en este archivo

Toda la información (personal, asistencia, pagos, descuentos, etc.) vive en Firestore, no en `index.html`. Esto significa que puedes reemplazar/actualizar este archivo en tu repositorio de GitHub cuantas veces quieras (para agregar mejoras) **sin perder ningún dato**, siempre que:

1. No cambies el `firebaseConfig` (las credenciales del proyecto de Firebase).
2. No cambies los nombres de las colecciones/documentos internos que usa la app para guardar cada módulo.

## Publicar en GitHub Pages

1. Sube este archivo a un repositorio de GitHub (puede ser privado).
2. Ve a **Settings → Pages**, elige la rama (`main`) y la carpeta raíz.
3. GitHub te dará una URL `https://tuusuario.github.io/turepo/` — esa es la URL que debes compartir con tu equipo.

## Seguridad — cosas importantes antes de compartir el link

Esta es una aplicación sin servidor propio (todo corre en el navegador de cada persona), así que su nivel de seguridad tiene límites que debes conocer antes de compartir el enlace con tu equipo:

- **Las contraseñas se guardan como hash SHA-256** (no en texto plano) desde esta versión. Aun así, cualquier persona con el link y conocimientos técnicos podría, en teoría, leer o escribir directamente en la base de datos de Firestore usando la configuración que está en el propio archivo — eso es una limitación inherente a cualquier app 100% cliente sin backend propio.
- Para reducir ese riesgo:
  - Usa **contraseñas únicas y no obvias** para cada usuario, especialmente para las cuentas de Administración.
  - No compartas el link públicamente (redes sociales, grupos abiertos); compártelo solo con las personas que deben usarlo.
  - Revisa periódicamente, en Firebase Console → Firestore → Reglas, que la regla siga siendo `allow read, write: if request.auth != null;` (requiere estar autenticado, aunque sea de forma anónima).
  - Si en algún momento necesitas seguridad más estricta (por ejemplo, que cada colaborador solo pueda leer/escribir su propio documento incluso manipulando la consola del navegador), eso requiere reglas de Firestore basadas en un backend/Cloud Functions con roles reales — fuera del alcance de una app sin servidor. Avísame si quieres que lo evaluemos.
- Las **cuentas de colaborador** (ver abajo) ayudan mucho en la práctica: cada colaborador solo ve su propio pago, descuentos y asistencia desde la interfaz, y no tiene ningún botón de edición disponible.

## Módulos incluidos

- **Registro de Postulantes**: candidatos del proceso de reclutamiento, con dashboard de reclutamiento.
- **Registro de Personal**: base de datos maestra del personal.
- **Asistencia del Personal**: asistencia diaria por ruta/placa, registro de inasistencias, Récord de asistencia por persona (con exportación a Excel con diseño), Tareo por Placa (tripulaciones por camión/placa, día a día, con exportación a Excel), Dashboard de Inasistencias y Panel de Indicadores.
- **Pago y Asistencia por Persona** (nuevo): para cada colaborador, calcula su bono según su "monto x día" configurado (importable desde Excel con columnas DNI / Nombre del trabajador / Monto x día). Cada día con **falta injustificada** descuenta el monto x día del bono; los días con **falta justificada** o **tardanza** no se descuentan. También muestra los descuentos del personal de esa persona y su historial completo de asistencia, con exportación a Excel con diseño (secciones de Pago, Resumen de asistencia, Placas y rutas, Historial completo y Descuentos del personal).
- **Cuadre de Caja**: ingresos y egresos de caja chica.
- **Mapas y Seguimiento de Rutas**: ubicación de clientes desde tu Excel de hoja de ruta.
- **Liquidaciones y Descuentos**: importación de PDF de liquidación de inventario por camión, y Descuentos del Personal (con división entre varias personas y puesto/rol de cada una).
- **Kardex de EPP**: entrega de equipos de protección personal y kardex por colaborador.
- **Administración**: crear usuarios, definir contraseñas (guardadas como hash) y elegir a qué apartados tiene acceso cada uno. Incluye la opción de crear **cuentas de colaborador**: vinculadas a un colaborador puntual, solo pueden ver "Pago y Asistencia por Persona" y, dentro de ese apartado, únicamente su propio pago, sus descuentos y su asistencia — sin poder editar nada.

## Sobre las cuentas de colaborador

Desde **Administración → Crear usuario**, marca la casilla **"Cuenta de colaborador (solo ve su propio pago)"** y elige a qué colaborador se vincula (de la lista de Registro de Personal / Asistencia). Esa cuenta:

- Al iniciar sesión, entra directo a **Pago y Asistencia por Persona**.
- En el menú, solo ve **Inicio** y **Pago y Asistencia por Persona** (los demás apartados quedan ocultos, y también bloqueados si se intenta entrar por otra vía).
- Dentro de Pago y Asistencia, el selector de colaborador queda fijo en su propio nombre (no puede ver el pago de otra persona), y el panel de gestión de montos x día (importar/editar) queda oculto — solo puede **ver**, nunca editar.
- Puede exportar a Excel su propio detalle de pago, descuentos y asistencia.

# SEGSOL — Sistema de Gestión de Personal

Aplicación web de un solo archivo (`index.html`) para gestionar personal, asistencia, descuentos, caja chica, rutas, liquidaciones y EPP. Funciona completamente en el navegador (sin backend propio) y guarda todo en `localStorage`, con sincronización opcional en la nube vía Firebase Firestore para que varias computadoras vean los mismos datos en tiempo real.

## Cómo usarla

1. Descarga `index.html` (o publícalo con GitHub Pages, ver más abajo) y ábrelo en Chrome, Edge o Firefox.
2. Inicia sesión con el usuario administrador por defecto:
   - **Usuario:** `bryantt2`
   - **Contraseña:** `bryan_04`
3. Desde **Administración** puedes crear más usuarios, definir su contraseña y elegir a qué apartados tiene acceso cada uno (o si es otro administrador).

Todo lo que registres (personal, asistencia, descuentos, caja, etc.) se guarda automáticamente en el navegador. Si tienes conexión a internet, también se sincroniza con la nube (ver la sección de Firebase más abajo) para que el mismo negocio pueda verse desde varias computadoras.

## Menú lateral y pantalla de Inicio

La navegación es un menú lateral oscuro (columna fija a la izquierda) con:

- Tu usuario y avatar arriba, junto con el estado de sincronización en la nube.
- Un grupo **OPCIONES** con todos los apartados a los que tienes acceso.
- Un grupo **SESIÓN** al final, con el botón para cerrar sesión.

En pantallas angostas (celular/tablet) el menú se oculta y aparece un botón de hamburguesa (☰) arriba a la izquierda para abrirlo; toca fuera del menú o elige una opción para cerrarlo de nuevo.

La pantalla de **Inicio** muestra primero un resumen rápido (colaboradores registrados, asistencias del día, postulantes pendientes y saldo de caja actual) y luego los apartados agrupados por tema: **Gestión de Personal**, **Pagos y Caja**, **Operaciones y Logística** y **Administración**. Si un usuario no tiene permiso para ningún apartado de un grupo, ese grupo completo se oculta en vez de mostrarse vacío.

## Módulos incluidos

- **Registro de Postulantes**: candidatos del proceso de reclutamiento, con estado admitido/no admitido/pendiente y un dashboard de reclutamiento.
- **Registro de Personal**: base de datos maestra del personal contratado (datos laborales, personales, de nacimiento, domicilio, contacto y referencia).
- **Asistencia del Personal**: registro diario de asistencia por ruta y placa, con tareo por placa y récord de asistencia.
- **Asistencia y Descuentos por Persona**: consulta, para cada colaborador y en un rango de fechas, su récord de asistencia (asistencias, faltas, faltas justificadas, tardanzas, % de asistencia, placas y rutas en las que salió) y sus descuentos del personal, con el total descontado. Todo se puede exportar a Excel.
- **Cuadre de Caja**: ingresos y egresos de caja chica, con cuadre y saldo acumulado.
- **Mapas y Seguimiento de Rutas**: visualiza en un mapa la ubicación de tus clientes a partir de tu Excel de hoja de ruta, con filtros por ruta, viaje y otros campos, y una vista de avance de rutas con su propio **Dashboard MR** (ver más abajo).
- **Liquidaciones y Descuento del Personal**: importa el PDF de liquidación de inventario de cada camión y controla los descuentos del personal.
- **Kardex de Entrega de EPP**: registra la entrega de equipos de protección personal a cada colaborador y consulta su kardex individual, con control de stock y mínimos.
- **Administración**: crea usuarios, define contraseñas y permisos por apartado.

## Sobre las cuentas de colaborador

Desde Administración también puedes crear una **cuenta de colaborador**, vinculada a una persona puntual del Registro de Personal. Ese tipo de cuenta:

- Entra directo al apartado **Asistencia y Descuentos por Persona** (no ve la pantalla de Inicio ni el resto de apartados).
- Solo puede consultar su propia información — no puede ver la asistencia ni los descuentos de otros colaboradores.
- No puede editar nada en ese apartado.

Esto permite darle acceso de solo consulta a un colaborador para que vea su propia asistencia y sus descuentos, sin exponerle el resto del sistema.

## Historial de Avance de Rutas (varias fechas)

Cada vez que importas la Hoja de Ruta o la Hoja del BEES, sus registros se **guardan en tu historial en vez de reemplazar lo anterior**: si hoy importas el archivo del 1 de julio y mañana el del 2 de julio, terminas con el historial de ambos días guardado, no solo el del último archivo que subiste. Si vuelves a importar el archivo de un día que ya habías cargado, solo se actualiza/corrige ese día — el resto de tu historial no se toca.

La pestaña **Avance de Rutas** (tarjetas y tabla de clientes programados/entregados/rechazados) trabaja siempre sobre un solo día a la vez, para que los conteos no se mezclen entre fechas distintas. Arriba de los filtros hay un selector de **Fecha**: por defecto muestra el día más reciente que tengas guardado, pero puedes elegir cualquier otro día de tu historial, o "Todas las fechas" para ver el acumulado completo. El botón "Quitar filtros" no cambia la fecha elegida (solo limpia Ruta/Empresa/Viaje/Estado/Buscar), y "Vaciar TODO el historial de avance" borra permanentemente todas las fechas guardadas (útil si necesitas empezar de cero).

El **Dashboard MR** (ver abajo) sí usa el historial completo de todas las fechas para sus gráficas y tablas por día/semana, independientemente de qué fecha tengas seleccionada en la pestaña "Avance de Rutas".

## Dashboard MR (Motivo de Rechazo)

Dentro de **Mapas y Seguimiento de Rutas → Avance de Rutas** hay una pestaña adicional, **Dashboard MR**, con una visual de indicadores de rechazo (estilo reporte de MR) armada 100% a partir de los datos que ya importas ahí mismo (en especial la "Hoja del BEES", que trae la columna **MR** con el motivo de rechazo). No necesita ningún archivo, pestaña ni conexión adicional: se recalcula sola al entrar a la pestaña, al cambiar el rango de fechas "Desde/Hasta", o al presionar el botón **🔄 Actualizar**.

Incluye:

- Tarjetas de HL Programado, Entregado, No Entregado (rechazos totales + entregas parciales/modificadas), Cajas No Entregadas, MR% Volumen y MR% Pedidos.
- Gráfica de rechazos por día (MR% Volumen y MR% Pedido) y otra de Refusal Total vs Refusal Parcial en hectolitros con el % de MR superpuesto.
- Dos gráficas de dona (participación de rechazos por HL y por número de clientes) y tres gráficas de barras (Customer, Logistic, Sales) con el detalle por motivo.
- Una tabla de detalle por Responsable y Motivo, otra por Tipo de MR (Refusal Total / Refusal Parcial), y un resumen semanal en hectolitros.

Algunas decisiones que tomamos al construirlo, para que las tengas presentes:

- **Responsable por motivo**: el responsable se asigna automáticamente según las palabras que contenga el texto del motivo (sin importar mayúsculas, acentos, ni si trae o no un código delante): **Customer** (cerrado, sin dinero, ausente, sin envases, rechazado), **Logistic** (fuera de horario, atribución a ruta/camión), **Almacén** (error en carga, mala calidad), **Sales** (mal facturado, no hizo pedido, no ubicado) y **Externo** (asalto). Si el texto del motivo no contiene ninguna de estas palabras, o el cliente no entregado no tiene motivo registrado, se agrupa como **"(en blanco)"** en vez de asumirle un responsable al azar. Si tu negocio usa otras palabras o motivos que no están en esta lista, dínoslo y se agregan al mapeo.
- **Refusal Total vs Refusal Parcial**: "Refusal Total" son los clientes totalmente rechazados; "Refusal Parcial" son las entregas modificadas (se entregó una parte del pedido y se rechazó el resto). Ambos cuentan para el HL "No Entregado" y para el MR% Pedidos de este dashboard.
- **Semanas por rango de fechas, no por número "W##"**: el resumen semanal agrupa de lunes a domingo y etiqueta cada semana por su rango de fechas (por ejemplo "29/06 al 05/07") en lugar de un número de semana tipo "W26". No se pudo confirmar con certeza la convención de numeración de semana fiscal que usa tu negocio (el estándar ISO-8601 no coincidía con los ejemplos de referencia), así que se prefirió no arriesgar a mostrar un número de semana incorrecto.
- El resumen semanal solo puede agrupar registros que tengan fecha (columna FECHA de la Hoja del BEES); un registro sin fecha sí se cuenta en las tarjetas y tablas generales, pero no aparece en las gráficas por día ni en el resumen semanal.

## Sincronización en la nube (Firebase)

La aplicación intenta conectarse a un proyecto de Firebase (Firestore) para sincronizar los datos entre computadoras en tiempo real. El estado se muestra como una insignia junto a tu usuario en el menú lateral:

- **✅ Sincronizado** (verde): los cambios se están guardando y recibiendo de la nube con normalidad.
- **⏳ Conectando…** (gris): se está estableciendo la conexión.
- **⚠️ Sin conexión / no disponible** (rojo): no se pudo conectar a la nube. La aplicación sigue funcionando con normalidad usando solo el almacenamiento local de este navegador, pero los cambios no se compartirán con otras computadoras hasta que la conexión se restablezca.

**Importante:** si vas a alojar tu propia copia (por ejemplo con GitHub Pages) y quieres que la sincronización en la nube funcione, necesitas:

1. Tener un proyecto de Firebase con Firestore habilitado.
2. Reemplazar el bloque `firebaseConfig` dentro de `index.html` con las credenciales de tu propio proyecto.
3. En Firestore, agregar el dominio donde publiques la página (por ejemplo `tuusuario.github.io`) a la lista de dominios autorizados del proyecto de Firebase (Authentication → Settings → Authorized domains, si usas autenticación, o revisar las reglas de seguridad de Firestore si no).
4. Revisar las reglas de seguridad de Firestore para permitir lectura/escritura desde tu dominio.

Si no configuras Firebase (o no tienes conexión a internet), la aplicación funciona igual, solo que cada computadora guarda sus propios datos de forma independiente en su `localStorage`.

## Publicarla con GitHub Pages

1. Crea un repositorio en GitHub y sube `index.html` (puedes usar este mismo repositorio, tal como está empaquetado).
2. Ve a **Settings → Pages** en el repositorio.
3. En "Source", elige la rama (por ejemplo `main`) y la carpeta raíz (`/`).
4. Guarda. GitHub te dará una URL como `https://tuusuario.github.io/tu-repositorio/` donde ya podrás abrir la aplicación.

## Notas técnicas

- Todo el código (HTML, CSS y JavaScript) vive en un solo archivo `index.html`, sin dependencias de build ni servidor propio.
- Usa `localStorage` como almacenamiento principal y Firebase Firestore como sincronización opcional en la nube.
- Las exportaciones a Excel usan las librerías SheetJS y ExcelJS, incluidas dentro del mismo archivo.
- El mapa usa Leaflet.

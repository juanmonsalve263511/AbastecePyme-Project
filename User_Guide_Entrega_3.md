# Guía de uso de AbastecePyme

**Actualizada el 7 de noviembre de 2026.** Esta guía explica la versión actual de la aplicación y las mejoras incorporadas el 6 de noviembre. Los pasos se contrastaron con los nombres y controles de la interfaz. Esta actualización documental no modificó el catálogo; las fechas y el alcance de las pruebas se detallan en la sección 15.

AbastecePyme reúne proveedores, insumos, productos y sus conexiones. Permite consultar precios, estimar materiales, explorar posibles impactos y ordenar la preparación según las dependencias registradas. Mantén los datos al día: los resultados cambian cuando se editan elementos o relaciones.

## 1. Abrir la aplicación

En **esta instalación**, la dirección habitual es [AbastecePyme en el puerto 3001](http://127.0.0.1:3001/). El acceso **Abrir.url** apunta a esa dirección.

1. Inicia **MySQL** desde XAMPP.
2. Abre la carpeta del proyecto.
3. Abre una terminal en esa carpeta y escribe `.\Iniciar.cmd --port 3001`. Si ya está funcionando, utiliza su ventana existente.
4. Cuando se solicite, introduce la contraseña de MariaDB en la entrada oculta.
5. Abre la dirección anterior y recarga la página.

Mantén abierta la ventana del servidor mientras usas la aplicación. Para detenerlo, pulsa **Ctrl+C** en esa ventana.

En una instalación nueva, **Iniciar.cmd** sin opciones utiliza el puerto **3000**: [dirección predeterminada](http://127.0.0.1:3000/). Usa siempre la dirección que corresponda al puerto indicado al arrancar; abrir otro puerto puede mostrar una instancia anterior.

Si es la primera preparación del proyecto, consulta [Instalación y operación](SETUP.md). **Instalar.cmd** prepara las dependencias y compila la interfaz. **ConfigurarAdministrador.cmd** permite crear el primer administrador una sola vez, si aún no hay uno activo. Las cuentas posteriores se crean dentro de la aplicación.

La cuenta de acceso al sitio y la cuenta de MariaDB son distintas. No escribas la contraseña de MariaDB en el formulario de inicio de sesión del sitio.

## 2. Iniciar sesión y reconocer tu perfil

1. En la portada, pulsa **Iniciar sesión**.
2. Introduce tu **Usuario** y **Contraseña** del sitio.
3. Pulsa **Iniciar sesión** para entrar al espacio de trabajo.
4. Revisa tu nombre y perfil en la esquina superior.

**No existe registro público.** Si necesitas una cuenta, solicítala al administrador de esta instalación.

| Perfil | Qué puede hacer |
|---|---|
| **Consulta** | Leer catálogo, precios y relaciones; usar grafo, impacto, producción y consultas del Explorador de API; gestionar sus propios datos de cuenta. |
| **Editor** | Todo lo anterior, más crear, editar y eliminar elementos y relaciones mediante formularios o el Explorador de API. |
| **Administrador** | Todo lo anterior, más crear cuentas, modificar accesos y consultar la bandeja privada de solicitudes de contacto. |

Los controles de edición aparecen según tu perfil. Para salir, utiliza el botón **Cerrar sesión** junto a tu nombre. Si la sesión vence, vuelve a iniciar sesión.

## 3. Orientarte desde el nuevo Resumen

**Resumen** presenta los totales actuales de proveedores, insumos, productos y relaciones. Pulsa una categoría para abrir su sección.

Los cuatro accesos principales son:

- **Organiza tu catálogo:** abre la información de proveedores, insumos y productos.
- **Conecta precios y cantidades:** abre las relaciones de compra y uso de materiales.
- **Anticipa el impacto:** permite seguir las consecuencias o los requisitos de un elemento.
- **Prepara el siguiente paso:** abre el orden de producción y la detección de ciclos.

**Explorar el grafo** muestra la red. El acceso al **Explorador de API** permite trabajar con solicitudes explícitas; para tareas habituales puedes utilizar los formularios del catálogo y las relaciones.

La navegación lateral conserva **Resumen**, **Catálogo**, **Relaciones**, **Grafo de abastecimiento**, **Análisis de impacto**, **Orden de producción** y **Explorador de API**. Con perfil Administrador también aparecen **Solicitudes de contacto** y **Usuarios y perfiles**. **Página de inicio** vuelve a la portada sin cerrar tu sesión.

## 4. Consultar y mantener el catálogo

### Buscar y abrir detalles

1. Entra en **Catálogo**.
2. Elige **Proveedores**, **Insumos** o **Productos**.
3. Usa **Buscar por nombre o código** para filtrar la lista.
4. Pulsa el nombre de un elemento para ver sus detalles. El perfil Consulta también dispone de **Ver detalles**.
5. Desde esa ventana, **Ver conexiones en el grafo** abre su entorno de relaciones.

Cada tipo guarda información distinta:

| Tipo | Información propia |
|---|---|
| **Proveedor** | Nombre, código, descripción y datos de contacto opcionales. |
| **Insumo** | Nombre, código, descripción y unidad de compra y uso, por ejemplo kg, metro o unidad. |
| **Producto** | Nombre, código, descripción y precio de venta opcional en pesos colombianos. Los componentes de fabricación también pueden estar registrados como productos. |

### Crear un elemento — Editor o Administrador

1. Abre la pestaña del tipo que necesitas.
2. Pulsa **Agregar proveedor**, **Agregar insumo** o **Agregar producto**.
3. Escribe un **Código** único y un **Nombre**. El código comienza con una letra y admite letras, números, guion y guion bajo; el formulario lo guarda en mayúsculas.
4. Completa la información correspondiente al tipo.
5. Pulsa **Guardar cambios**.

El **código y el tipo son estables**: no se cambian al editar. Revisa ambos antes de crear. Si necesitas corregirlos, planifica un registro nuevo y revisa sus relaciones antes de retirar el anterior.

### Editar o eliminar

Para modificar un elemento, pulsa **Editar**, cambia los campos permitidos y elige **Guardar cambios**.

Para retirarlo:

1. Pulsa **Eliminar** junto al elemento.
2. Revisa el nombre y la cantidad de relaciones que también se retirarán.
3. Elige **Cancelar** para conservarlo o **Confirmar eliminación** para borrarlo.

Eliminar un elemento también elimina sus relaciones. No hay un botón para deshacer esta eliminación desde la página. Si aparece un conflicto porque el registro cambió, recarga y revisa la información antes de intentar otra vez.

### Precio de venta en COP

En **Productos**, la columna **Precio (COP)** muestra el importe por unidad. Si no se ha definido, aparece **Por definir**.

1. Pulsa **Editar** sobre el producto.
2. Completa **Precio de venta (COP)** con un importe no negativo, con hasta dos decimales.
3. Mantén **Precio de referencia** si es un ejemplo o un valor pendiente de validar. Desmárcalo cuando corresponda a un importe que hayas registrado y revisado.
4. Guarda los cambios. Dejar el campo vacío vuelve a mostrar **Por definir**; un precio cero es distinto de un campo vacío.

Las etiquetas **Precio de referencia** y **Precio registrado** describen cómo se ha marcado el dato. **Precio registrado no significa que la aplicación haya comprobado una cotización comercial vigente.** Los importes iniciales de referencia son editables; no se presentan como precios actuales del mercado.

### Entender la estimación de materiales

Al abrir los detalles de un producto aparece **Estimación de materiales**:

- Usa los insumos relacionados directamente con ese producto.
- Para cada insumo, toma la oferta de menor precio que esté registrada y la multiplica por la cantidad requerida.
- Muestra el proveedor elegido y señala los insumos que no tienen oferta.
- Si faltan ofertas, no hay requisitos suficientes o existen otras dependencias que no incorpora al cálculo, indica **Estimación incompleta**.
- Si intervienen valores de referencia, la estimación también lo señala.

Esta estimación **no es el precio de venta ni el costo total de fabricación**. No incorpora mano de obra, impuestos, transporte, margen ni el costo de dependencias generales; tampoco calcula automáticamente el costo completo de componentes anidados. No comprueba inventario ni disponibilidad.

Si cambias la unidad de un insumo, revisa sus precios y cantidades asociados para que todos utilicen la misma unidad.

## 5. Crear y revisar relaciones

Una flecha siempre va del **requisito hacia lo que depende de él**.

| Pestaña | Conexión | Valor |
|---|---|---|
| **Precios de proveedores** | Proveedor → insumo | Precio por una unidad del insumo, en COP. |
| **Insumos por producto** | Insumo → producto | Cantidad del insumo necesaria por unidad del producto. |
| **Dependencias generales** | Otras conexiones, como componente → producto | Sin precio ni cantidad. Indica que el origen debe prepararse antes que el destino. |

Los proveedores de un mismo insumo son **alternativas de compra**. Tener varias ofertas no obliga a comprar a todos. La cantidad de alternativas depende del catálogo actual; puede cambiar con las ediciones.

### Agregar una conexión — Editor o Administrador

1. Entra en **Relaciones** y pulsa **Agregar relación**.
2. Elige **Origen (requisito)** y **Destino (dependiente)**.
3. Si es proveedor → insumo, introduce **Precio por unidad (COP)**.
4. Si es insumo → producto, introduce **Cantidad requerida**, mayor que cero y con hasta cuatro decimales.
5. Marca **Usar como valor de referencia** cuando corresponda a un valor orientativo.
6. Pulsa **Guardar relación**.

El tipo se determina por los elementos seleccionados. Una dependencia general no lleva peso.

Usa **Buscar una conexión** para localizarla. **Editar** permite cambiar el valor y su condición de referencia cuando corresponda; los extremos de una relación existente no se cambian en ese formulario. Para conectar otros elementos, crea la conexión correcta y revisa si debes eliminar la anterior.

**Eliminar → Confirmar eliminación** retira solo la relación: conserva los elementos. La misma pareja dirigida no puede duplicarse. Las conexiones inversas, los ciclos y las autorrelaciones pueden quedar registradas; revisa su sentido con **Orden de producción**.

## 6. Explorar el grafo

1. Abre **Grafo de abastecimiento**.
2. En **Explorar alrededor de**, conserva **Toda la red** o elige un elemento.
3. Pulsa un nodo para ver sus datos y conexiones en el panel lateral.
4. Usa **Enfocar conexiones** para centrar la exploración en ese elemento.
5. Activa **Mostrar todos los pesos** para ver precios y cantidades.

Controles disponibles:

- **+ / −:** acercar o alejar. La rueda del ratón también modifica el zoom.
- **Arrastrar el fondo:** desplazar la vista. Al enfocar el dibujo, las flechas del teclado también lo desplazan.
- **Ajustar vista:** recuperar el encuadre del conjunto que estás viendo.
- **Restablecer:** quitar el enfoque y la selección y volver a la vista general.
- **Tab** y **Enter** o **Espacio:** recorrer y seleccionar los nodos con teclado.

Las columnas agrupan proveedores, insumos y productos. Una flecha proveedor → insumo muestra precio de compra; una flecha insumo → producto muestra cantidad usada. Las dependencias generales se representan sin peso.

## 7. Analizar el impacto — Feature 2

**Alcanzar** un elemento significa llegar a él siguiendo una cadena de dependencias. Por ejemplo:

**Maderas del Norte → Tablero de madera → Mesa de trabajo**

Desde el proveedor, el tablero es una conexión directa y la mesa se alcanza a través del tablero. Esto indica **impacto potencial**: no confirma que la fabricación vaya a detenerse, porque puede haber existencias o proveedores alternativos cuya disponibilidad no registra la aplicación.

### Consultar consecuencias

1. Abre **Análisis de impacto**.
2. En **¿Qué quieres conocer?**, elige **Consecuencias: ¿qué podría afectar?**.
3. Busca un nombre o código en **Buscar en el catálogo**.
4. En **Elemento inicial**, elige, por ejemplo, **Maderas del Norte**, si sigue presente.
5. Pulsa **Analizar conexiones**.
6. Revisa los resultados directos e indirectos.
7. Pulsa **Ver camino** en una fila para destacar su recorrido en ámbar en el grafo.

### Consultar requisitos

1. Cambia a **Requisitos: ¿de qué depende?**.
2. Elige un producto, por ejemplo **Mesa de trabajo**, si está en el catálogo.
3. Pulsa **Analizar conexiones**.

Esta opción recorre las conexiones hacia atrás para encontrar lo que el producto necesita. Las flechas guardadas conservan su dirección original.

Cómo interpretar el resultado:

- El origen se muestra como punto de partida, pero no se suma al total de otros elementos alcanzados.
- **Directo** significa una relación; **indirecto**, dos o más.
- **Ver camino** explica un recorrido con el menor número de relaciones, no la ruta más barata ni todos los caminos posibles.
- Los precios y las cantidades no determinan el alcance.
- Un resultado sin otros elementos alcanzables puede ser correcto; no equivale a un error.
- Este recorrido no es un orden de fabricación. Para eso utiliza la siguiente sección.

Los totales de los ejemplos dependen de las relaciones actuales. Si cambian los datos, vuelve a consultar.

## 8. Obtener un orden de producción — Feature 3

1. Abre **Orden de producción**.
2. En **Qué quieres organizar**, elige **Toda la red de abastecimiento** o un producto concreto.
3. Pulsa **Generar orden**.
4. Si aparece **Dependencias compatibles**, lee las etapas de arriba hacia abajo.
5. Despliega los **requisitos previos** de un elemento para entender su posición.
6. Abre **Ver el orden completo y la red analizada** para explorar el resultado completo.

Cada etapa reúne elementos cuyos requisitos ya quedaron situados en etapas anteriores. Dentro de una etapa puedes elegir el orden. Al seleccionar un producto, el análisis considera ese producto y sus requisitos transitivos; un ciclo ajeno a ese conjunto no tiene por qué impedir su resultado.

### Si aparece Ciclo detectado

Un elemento termina dependiendo de sí mismo a través de una o varias relaciones. La pantalla muestra **El camino que cierra el ciclo** y no presenta un orden parcial como solución válida.

1. Lee las relaciones señaladas.
2. Identifica si una flecha está invertida o no corresponde al proceso.
3. Si eres Editor o Administrador, pulsa **Revisar relaciones** y corrige la información necesaria.
4. Vuelve a generar el orden: puede haber más de un ciclo.

La lista de **elementos pendientes** puede incluir integrantes del ciclo y otros que dependen de él. Estar pendiente no significa pertenecer al ciclo mostrado.

El orden respeta las dependencias registradas. No calcula tiempos, capacidad, inventario ni disponibilidad de proveedores. Las ofertas siguen siendo alternativas; el resultado no obliga a comprar a todos los proveedores. El número de elementos y etapas cambia con el catálogo.

### Cómo comprobar los cuatro requisitos de esta función

| Requisito | Qué debes poder observar |
|---|---|
| **Generar un orden válido cuando no hay ciclos** | Cada requisito aparece antes del elemento que depende de él; puedes revisar tanto las etapas como el orden completo. |
| **Detectar y explicar un ciclo** | La pantalla identifica un camino que vuelve al elemento inicial y permite revisar las relaciones que lo forman. |
| **Evitar un orden falso cuando hay un ciclo** | Se muestra la advertencia y no se ofrece un orden parcial como si resolviera la red analizada. |
| **Comprobar un caso sin ciclo y otro con ciclo** | Hay ejemplos y pruebas repetibles para ambos resultados; no necesitas alterar tus datos de trabajo para comprobarlos. |

El [detalle de Feature 3](FEATURE3.md) relaciona cada requisito con su implementación y evidencia, explica los ejemplos y ofrece el procedimiento de comprobación sin base de datos.

## 9. Usar el Explorador de API

Esta sección permite ver qué solicita la aplicación y qué responde el servidor. **Catálogo** y **Relaciones** ofrecen formularios más sencillos para las operaciones habituales.

### Consultar información

1. Abre **Explorador de API**.
2. En **Operación**, elige una consulta **GET**: catálogo y relaciones, datos del grafo, impacto, estimación de materiales u orden y ciclos.
3. Selecciona el elemento, producto o dirección cuando se solicite.
4. Pulsa **Consultar información**.
5. Lee el panel **Respuesta**.

### Crear, actualizar o eliminar — Editor o Administrador

1. Elige **POST** para crear, **PATCH** para actualizar o **DELETE** para eliminar un elemento o relación.
2. Para actualizar o eliminar, selecciona el registro correspondiente.
3. Si aparece **Datos de la solicitud (JSON)**, revisa y edita los valores.
4. Conserva **Version** al actualizar o eliminar: identifica la versión del registro que estás revisando. No la inventes ni la incrementes por tu cuenta.
5. Pulsa **Revisar cambio**.
6. Comprueba el registro, los datos y el efecto indicado. La eliminación de un elemento también muestra las relaciones que se retirarán.
7. Pulsa **Confirmar y ejecutar** o **Cancelar**.
8. Revisa la respuesta. Después de guardar, el catálogo vuelve a solicitarse para mostrar los cambios.

En este editor, precios y cantidades se escriben entre comillas como texto decimal, con punto y sin separadores de miles: **"35000.00"** o **"2.5000"**. En relaciones, **SourceId** identifica el requisito y **TargetId** su dependiente. **Value** contiene el precio o la cantidad; para una dependencia general se usa **null**.

El perfil Consulta solo dispone de operaciones de lectura. Esta pantalla conserva los permisos de tu sesión; la creación y administración de cuentas se realiza en **Usuarios y perfiles**.

Si aparece **409**, puede haber un duplicado o un cambio posterior a la versión seleccionada. Recarga la página, selecciona de nuevo el registro y revisa la operación antes de repetirla.

## 10. Actualizar Mi cuenta

Pulsa tu nombre en la parte superior para abrir **Mi cuenta**.

- En **Datos básicos**, cambia **Nombre** o **Correo (opcional)** y pulsa **Guardar mis datos**.
- Para cambiar la contraseña, completa **Contraseña actual** y **Nueva contraseña**, de 12 a 128 caracteres, y pulsa **Actualizar contraseña**.

Cambiar la contraseña cierra las sesiones de esa cuenta; deberás volver a iniciar sesión. Desde Mi cuenta no cambias tu usuario ni te asignas otro perfil.

## 11. Crear cuentas y gestionar accesos — Administrador

### Crear una cuenta

1. Abre **Usuarios y perfiles** y pulsa **Crear usuario**.
2. Completa **Nombre completo**, **Usuario**, **Perfil** y, si corresponde, **Correo (opcional)**.
3. El usuario admite entre 3 y 40 letras, números, guiones o guion bajo.
4. Define una **Contraseña inicial** de 12 a 128 caracteres y repítela en **Confirmar contraseña**.
5. Pulsa **Crear usuario**.

Tu sesión de administrador permanece abierta. Entrega los datos de acceso por un medio privado; no se envían invitaciones por correo. La persona puede cambiar su contraseña en **Mi cuenta**.

### Cambiar el acceso

1. Busca la persona en **Usuarios y perfiles**.
2. Pulsa **Editar acceso**.
3. Elige el **Perfil** y marca o desmarca **Cuenta activa**.
4. Pulsa **Guardar acceso**.

Cambiar el acceso cierra las sesiones de esa cuenta. Si modificas tu propio acceso, deberás volver a entrar. La aplicación impide quitar o desactivar al último administrador activo.

## 12. Contacto público y bandeja privada

### Enviar una solicitud

Puedes abrir **Contacto** desde la portada sin iniciar sesión.

1. Completa **Nombre completo** y **Correo electrónico**.
2. Añade **Celular** y **Empresa** si deseas compartirlos; son opcionales.
3. Elige un tema en **¿Sobre qué quieres conversar?**.
4. Escribe **Tu solicitud**, entre 20 y 2.000 caracteres.
5. Lee los términos y marca la autorización para guardar tus datos y solicitud.
6. Pulsa **Enviar solicitud**.
7. Conserva la **Referencia** que aparece en la confirmación.

El formulario muestra el avance de **Tus datos**, **Tu solicitud** y **Autorización**. La confirmación indica que la solicitud quedó guardada para consulta del administrador. **No significa que se haya enviado un correo ni una respuesta automática.**

### Consultar solicitudes — Administrador

1. Entra en **Solicitudes de contacto**.
2. Pulsa **Actualizar** para obtener los registros disponibles.
3. Selecciona una solicitud para leer su mensaje y los datos compartidos.
4. Pulsa **Marcar como leída** si corresponde.
5. Usa **Anterior** y **Siguiente** para recorrer las páginas.

Las etiquetas distinguen **Nueva** y **Leída**. Las fechas se presentan en horario de Colombia. La bandeja no envía respuestas automáticas y no está disponible para Consulta o Editor.

### Datos de contacto publicados

La aplicación muestra los siguientes datos:

- **Centro de Negocios One Plaza**, Torre Norte, piso 3.
- **Carrera 43A #5A-113**, El Poblado, Medellín, Colombia.
- **Celular: +57 311 335 2866**.

El teléfono abre la opción de llamada del dispositivo cuando esté disponible; el enlace del mapa abre la ubicación en Google Maps. Estos datos se muestran en la portada, Contacto y las secciones de ubicación del sitio.

## 13. Páginas públicas y novedades

Los enlaces públicos permiten abrir **Servicios**, **Nuestra historia**, **Locaciones**, **Contacto**, **Términos y condiciones** y **Novedades**. Puedes volver a la portada mediante **Volver al inicio** o el nombre de AbastecePyme.

Para registrar interés en novedades:

1. Abre **Suscríbete a novedades** en el pie de página.
2. Introduce **Tu correo electrónico**.
3. Marca la autorización para guardarlo en esta instalación.
4. Pulsa **Suscribirme** y espera la confirmación.

Se guarda la solicitud con consentimiento. **El envío de boletines y confirmaciones por correo todavía no está habilitado.** Esta suscripción no crea una cuenta de acceso. Para consultas sobre el registro o su retiro, sigue el canal de atención indicado en los términos de la instalación.

## 14. Pantallas pequeñas y preferencias

En una pantalla estrecha, el botón **Abrir navegación** despliega las secciones. Elige una opción para cerrar el menú y abrirla. Algunas tablas pueden desplazarse horizontalmente dentro de su contenedor; el grafo dispone de sus propios controles de zoom y desplazamiento.

La interfaz se adapta a tamaños de celular, pero esta instalación solo escucha en el equipo donde se inicia. Abrir `127.0.0.1` en otro teléfono no conecta con ese equipo; el acceso desde otros dispositivos requiere configurar un despliegue separado.

En el pie de la página y en el acceso encontrarás:

- **Sonidos: activados/desactivados.** Permite tonos breves al iniciar o cerrar sesión. Están desactivados inicialmente y dependen del soporte y los permisos del navegador.
- **Reducir movimiento: sí/no.** Disminuye las animaciones. La aplicación también respeta la preferencia de movimiento reducido del sistema.

Las preferencias se guardan en ese navegador cuando permite almacenamiento local; no se sincronizan entre dispositivos.

Para recorrer controles con teclado, usa **Tab** y **Mayús+Tab**. El foco visible indica dónde estás. Puedes cerrar una ventana con su botón de cierre o con **Escape**, siempre que no esté bloqueada mientras se guarda una operación.

## 15. Mejoras del 6 de noviembre

La versión actual incorpora:

- Un **Resumen centrado en tareas**, navegación más compacta, tablas más legibles y un pie azul acorde con la marca.
- Imágenes más ligeras y adaptadas al tamaño de pantalla.
- Carga de secciones cuando se necesitan y un grafo que evita cálculos repetidos.
- Mensajes más claros ante problemas de conexión o respuestas inesperadas.
- Cancelación de lecturas reemplazadas y protección frente a respuestas de una sesión anterior.
- Mejoras de foco, ayudas de campos, cierre de ventanas y presentación móvil.
- Una verificación unificada y separación de los datos de la versión educativa anterior.

El [informe de revisión del 6 de noviembre](REVIEW_2026-11-06.md) registra **68 pruebas Python y 48 JavaScript aprobadas**, además de compilación y comprobaciones de interfaz. Son resultados históricos de esa entrega: **la suite completa no se volvió a ejecutar el 7 de noviembre para actualizar esta guía**. El informe también describe los criterios y herramientas usados, incluida la guía de diseño incorporada al proyecto.

El **7 de noviembre** se comprobaron por separado los criterios de producción: **9 pruebas aprobadas**, sin conexión de red ni acceso a la base de datos. El [documento de Feature 3](FEATURE3.md) recoge esa comprobación específica; no equivale a repetir todas las pruebas de la aplicación.

## 16. Resolver problemas habituales

| Lo que ocurre | Qué hacer |
|---|---|
| La página no abre o aparece un error de conexión | Comprueba que MySQL y la ventana de la aplicación estén abiertos. En esta instalación utiliza el puerto 3001; revisa la dirección mostrada al arrancar. |
| El puerto está ocupado | Si es la instancia correcta, úsala. Para reiniciar, detén esa instancia con Ctrl+C. Si eliges otro puerto al arrancar, abre exactamente esa nueva dirección. |
| Ves una versión anterior o faltan secciones | Comprueba el puerto y recarga. Después de actualizar código, vuelve a compilar e iniciar la aplicación siguiendo SETUP. Recargar solo el navegador no actualiza un servidor Python que quedó abierto. |
| Aparece un 404 genérico al consultar impacto o producción | Puede estar abierto un servidor anterior. Reinicia la aplicación actual en su puerto habitual y recarga. |
| El 404 dice “Elemento no encontrado” o “Producto no encontrado” | Ese registro puede haber sido eliminado. Recarga el catálogo y selecciona uno existente. |
| Aparece 400 o 422 | Revisa los campos señalados, códigos, formato de correo, consentimiento y valores. En el Explorador de API revisa también el JSON y los decimales. |
| Aparece 401 | La sesión falta o venció. Inicia sesión de nuevo. |
| Aparece 403 | Puede faltar permiso para la operación o haber caducado la validación de la sesión. Revisa tu perfil y recarga. Si trabajas con la API, usa el Explorador integrado del mismo sitio. |
| Aparece 409 | Puede existir un duplicado o alguien pudo editar el registro. Recarga, selecciona la versión actual y revisa antes de repetir. También puede impedir retirar al último administrador activo. |
| Aparece 413 | La solicitud es demasiado grande. Reduce el contenido y respeta los límites de los campos. |
| Aparece 429 | Se han realizado demasiados intentos en poco tiempo. Espera aproximadamente un minuto antes de volver a intentar. |
| Aparece 503 o un mensaje de base de datos | Comprueba MySQL. Si persiste, pide al responsable de la instalación que revise el arranque y la conexión. |
| La estimación parece demasiado baja o está incompleta | Revisa unidades, cantidades, ofertas ausentes y dependencias generales. La cifra solo representa los materiales que el cálculo puede incluir. |
| Producción muestra un ciclo | Revisa la cadena indicada y las relaciones; no es una recomendación de producir en ese orden. Corrige los datos y vuelve a analizar. |
| No aparece Editar, Eliminar o Crear usuario | Comprueba el perfil. Consulta no modifica catálogo y solo Administrador crea cuentas. |
| No llega un correo tras contacto, suscripción o creación de cuenta | El envío de correo no está habilitado. Revisa la confirmación en pantalla o consulta al administrador. |

Si una solicitud falla sin confirmar el resultado, comprueba primero si el cambio quedó guardado antes de enviarlo de nuevo. Para reportar un problema, indica sección, mensaje mostrado y acción que intentabas realizar; no incluyas contraseñas.

## 17. Documentación complementaria

- [Instalación, arranque, respaldo y verificación](SETUP.md).
- [Detalle del análisis de impacto](FEATURE2.md).
- [Significado del orden, ciclos y criterios de producción](FEATURE3.md).
- [Ampliación de catálogo y precios del 2 de noviembre](CATALOG_UPDATE_2026.md): describe el estado de aquella entrega; tus datos pueden haber cambiado.
- [Contacto y preferencias de movimiento](CONTACT_AND_MOTION.md).
- [Revisión y evidencia del 6 de noviembre](REVIEW_2026-11-06.md).

Para una comprobación local, **Verificar.cmd** o **npm run check** ejecutan las pruebas y una compilación temporal sin pedir credenciales de MariaDB. No sustituyen una prueba de integración con una base destinada a pruebas ni una revisión visual. El procedimiento completo está en SETUP.

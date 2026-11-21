# Guía de uso de AbastecePyme

**Actualizada el 16 de noviembre de 2026.** Esta guía incorpora movimiento libre y selección múltiple de nodos, filtros y orden del catálogo, información por revisar, lectura del grafo, costos mínimo/máximo, precio calculado y las funciones anteriores. La evidencia se distingue por fecha en la sección 16; el video del 14 de noviembre no muestra las novedades del día 16.

AbastecePyme reúne proveedores, insumos, productos y sus conexiones. Permite consultar precios, estimar materiales, explorar posibles impactos y ordenar la preparación según las dependencias registradas. Mantén los datos al día: los resultados cambian cuando se editan elementos o relaciones.

## Seleccionar una opción y verla en el grafo

En **Grafo de abastecimiento**, selecciona un producto y pulsa **Resaltar opción económica** o **Resaltar opción costosa** en su panel de detalles, junto al grafo o debajo de él según el espacio disponible. Una marca **✓**, conexiones gruesas y una leyenda identifican proveedores, insumos y componentes de la opción completa. Funciona también en **Orden de producción**, dentro de **Ver el orden completo y la red analizada**.

**Quitar selección de costo** restaura la vista anterior. Cambiar de producto o actualizar los datos limpia la selección. Si el cálculo está incompleto o existe un ciclo, los botones no se habilitan. [Detalles y capturas](COST_SELECTION_2026-11-16.md).

## Leer y encuadrar el grafo

**Ajustar red** muestra todos los elementos de la red visible. **Tamaño legible** centra y acerca el elemento seleccionado; puedes arrastrar el fondo, usar **+ / −** o las flechas del teclado para explorar los demás. Al recorrer nodos con el teclado, el encuadre revela el que recibe el foco.

En pantallas con poco espacio, el detalle queda debajo del grafo. Sus opciones de costo muestran el mismo resaltado; desplázate hacia el dibujo para verlo. El precio de venta aparece antes del desglose y las alternativas se presentan juntas cuando caben. La pausa de flechas y la preferencia de movimiento reducido siguen disponibles.

[Detalle de las mejoras y su verificación](READABILITY_PERFORMANCE_2026-11-16.md).

## 1. Abrir la aplicación

En **esta instalación**, la dirección habitual es [AbastecePyme en el puerto 3001](http://127.0.0.1:3001/). El acceso **Abrir.url** apunta a esa dirección.

1. Inicia **MySQL** desde XAMPP.
2. Abre la carpeta del proyecto.
3. Abre una terminal en esa carpeta y escribe `.\Iniciar.cmd --port 3001`. Si ya está funcionando, utiliza su ventana existente.
4. Cuando se solicite, introduce la contraseña de MariaDB en la entrada oculta.
5. Abre la dirección anterior y recarga la página.

Mantén abierta la ventana del servidor mientras usas la aplicación. Para detenerlo, pulsa **Ctrl+C** en esa ventana.

**Entrega anterior de resaltado, del 16 de noviembre:** el reinicio quedó confirmado por el usuario. Se comprobó que la página de 3001 responde y sirve exactamente la interfaz compilada de aquella entrega. La consulta de estimación sin iniciar sesión pidió autenticación, como corresponde. Los costos se probaron funcionalmente con datos sintéticos; no se hizo una consulta autenticada de costos sobre MariaDB.

Para futuras actualizaciones del servidor, detén AbastecePyme desde su ventana con **Ctrl+C**, ejecuta otra vez `.\Iniciar.cmd --port 3001` con MySQL iniciado e introduce la contraseña en la entrada oculta. El nuevo reinicio de legibilidad y rendimiento también fue confirmado; el puerto 3001 sirve la interfaz actualizada.

**El reinicio para habilitar el guardado de posiciones por usuario ya quedó confirmado en 3001.** Para ver el último aumento de velocidad de la portada, recarga con **Ctrl+F5**; no requiere otro reinicio. Si actualizas una copia anterior al guardado por usuario, compila y reinicia AbastecePyme con `Iniciar.cmd --port 3001`, introduciendo la contraseña de MariaDB en su aviso oculto. Esa ampliación añade una tabla de preferencias y rutas Python. MySQL permanece iniciado.

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

**Resumen** presenta los totales actuales de proveedores, insumos, productos y relaciones. Pulsa una categoría para abrir su sección. La sección de datos por revisar identifica productos con costos incompletos o ciclos, datos de referencia e insumos sin ofertas. Cada grupo abre el catálogo con el filtro correspondiente. Son observaciones del catálogo; no representan falta de existencias ni ventas realizadas.

Los cuatro accesos principales son:

- **Organiza tu catálogo:** abre la información de proveedores, insumos y productos.
- **Conecta precios y cantidades:** abre las relaciones de compra y uso de materiales.
- **Anticipa el impacto:** permite seguir las consecuencias o los requisitos de un elemento.
- **Prepara el siguiente paso:** abre el orden de producción y la detección de ciclos.

**Explorar el grafo** muestra la red. El acceso al **Explorador de API** permite trabajar con solicitudes explícitas; para tareas habituales puedes utilizar los formularios del catálogo y las relaciones.

La navegación lateral incluye **Resumen**, **Tablero de abastecimiento**, **Catálogo**, **Relaciones**, **Grafo de abastecimiento**, **Análisis de impacto**, **Orden de producción** y **Explorador de API**. Con perfil Administrador también aparecen **Solicitudes de contacto** y **Usuarios y perfiles**. **Página de inicio** vuelve a la portada sin cerrar tu sesión.

## 4. Usar el Tablero de abastecimiento — Feature 4

El **Tablero de abastecimiento** reúne el catálogo, las consecuencias potenciales y el orden o alerta de ciclo. Permite responder «¿qué podría pasar si falta este elemento?» y comparar un requisito distinto sin guardar ese ensayo.

### Consultar una posible indisponibilidad

1. Abre **Tablero de abastecimiento**.
2. Usa **Buscar en el catálogo** y selecciona el proveedor o insumo en **Elemento no disponible**.
3. En **Alcance del orden estructural**, elige **Toda la red de abastecimiento** o un producto concreto. Este filtro solo limita el orden; el impacto se consulta en toda la red.
4. Mantén **Sin cambio de dependencias** y pulsa **Simular indisponibilidad**.
5. Revisa qué elementos podrían verse afectados y el orden o ciclo de producción.
6. Pulsa **Ver camino** en un resultado para destacar su conexión con el origen. **Ver impacto** muestra ese recorrido; **Ver ciclo** o **Destacar ciclo en el grafo** permiten revisar una contradicción cuando existe.

En **Orden estructural**, despliega **Ver las … etapas de preparación** o **Ver el orden completo**. Los nombres y las cantidades corresponden al análisis seleccionado.

El dibujo empieza con **Enfocar conexiones**: muestra el origen y toda su cadena de consecuencias, también las más lejanas. **Mostrar grafo completo** permite ver toda la red actual o del escenario. En modo ciclo, el enfoque muestra solo el ciclo señalado. Cambiar el alcance visible no cambia los resultados del análisis.

Por ejemplo, **Maderas del Norte → Tablero de madera → Panel preparado → Estante modular** explica cómo un proveedor podría repercutir en un producto a través de sus requisitos. Los nombres son ejemplos; utiliza los elementos que existan en tu catálogo.

El elemento seleccionado no se elimina. La aplicación analiza sus conexiones y conserva las dependencias al preparar el orden. Un orden válido no demuestra que puedas fabricar mientras falte el requisito.

### Comparar un requisito distinto

Puedes ensayar **un cambio a la vez**:

- **Agregar una dependencia general:** elegir **Origen: requisito** y **Destino: dependiente** para una conexión que aún no exista. No lleva precio ni cantidad.
- **Retirar una relación existente:** elegir **Relación que se retiraría** para analizar cómo sería la red sin ella. Sus elementos se conservan.

1. En **Probar un cambio temporal**, elige una opción de **Cambio de dependencia** y completa sus campos.
2. Pulsa **Comparar escenario**.
3. En **Qué cambia en el escenario**, revisa los elementos que empiezan o dejan de estar alcanzados y el estado del orden. Usa **Red actual** y **Escenario temporal** para cambiar la vista; **Ver camino anterior** explica una consecuencia que desaparece del escenario.
4. Si aparece un ciclo, revisa su camino cerrado; no se presenta un orden parcial como una solución.
5. Pulsa **Descartar cambio temporal**. Esto limpia el ensayo y sus resultados; pulsa otra vez **Simular indisponibilidad** para analizar la red registrada.

Las altas temporales **proveedor → insumo** e **insumo → producto** se rechazan porque necesitan precio o cantidad. Para registrarlas o editarlas con sus valores, utiliza **Relaciones**.

Consulta, Editor y Administrador pueden simular. **El cambio temporal no se guarda en el catálogo.** Si después decides modificar una relación real, utiliza las operaciones de Catálogo o Relaciones con perfil Editor o Administrador y revisa la confirmación correspondiente.

**Registrar dependencia** abre Relaciones para los perfiles que pueden editar; Consulta ve **Consultar relaciones**. Después de una edición real, vuelve al tablero, pulsa **Actualizar catálogo** si corresponde y repite el análisis. **Abrir catálogo** permite revisar los elementos completos.

### Entender la comparación

- **Impacto potencial:** el camino demuestra una dependencia registrada, no un faltante confirmado. Puede haber existencias o proveedores alternativos.
- **Orden de preparación:** respeta requisitos; no calcula capacidad, fechas o duración.
- **Producto concreto:** analiza sus requisitos. Un ciclo de otra rama puede no afectar ese producto.
- **Sin alcanzados:** es un resultado válido para un elemento existente sin consecuencias en la red.
- **Datos cambiados:** vuelve a analizar después de editar el catálogo o cambiar la selección; evita interpretar un resultado de una consulta anterior.

Un código que ya no exista y un dato inválido producen mensajes de error, no un análisis vacío. Recarga el catálogo si otro usuario pudo eliminar el elemento.

El [documento de Feature 4](FEATURE4.md) explica el cambio de requisitos y sus cuatro casos de aceptación. Puedes seguir el [video del recorrido, de 2:15 y sin voz](media/Feature4-Recorrido.webm), con títulos y [textos SRT](media/Feature4-Recorrido.srt), o preparar una [demostración aislada](FEATURE4_VIDEO.md) con datos sintéticos, sin usar ni modificar MariaDB. La cuenta publicada para esa demostración **no sirve en la instalación normal**.

## 5. Consultar y mantener el catálogo

### Buscar y abrir detalles

1. Entra en **Catálogo**.
2. Elige **Proveedores**, **Insumos** o **Productos**.
3. Usa **Buscar por nombre o código** para filtrar la lista.
4. Pulsa el nombre de un elemento para ver sus detalles. El perfil Consulta también dispone de **Ver detalles**.
5. Desde esa ventana, **Ver conexiones en el grafo** abre su entorno de relaciones.

### Filtrar y ordenar

- En **Productos**, usa **Mostrar** para ver todos, costos incompletos, ciclos o datos de referencia. Un costo incompleto necesita revisar sus insumos, ofertas o dependencias; un ciclo aparece en su filtro separado.
- En **Insumos**, el filtro **Sin ofertas de proveedor** muestra los que no tienen una relación de precio registrada. Un precio de cero sí cuenta como oferta.
- En **Ordenar por**, elige nombre o, para productos, costo/venta de menor a mayor o de mayor a menor. Los importes pendientes siempre quedan al final.
- Puedes combinar el filtro con la búsqueda por nombre o código. El contador muestra cuántos registros ves del total de esa categoría.
- **Limpiar filtros** restablece búsqueda, filtro y orden. El filtro y el orden quedan en el enlace de la página para volver a la misma vista; la búsqueda escrita no se guarda en el enlace.
- Si no hay coincidencias, cambia o limpia los filtros. Si la categoría está vacía, los perfiles con edición pueden agregar un elemento; el perfil Consulta debe solicitarlo a un editor o administrador.

Cada tipo guarda información distinta:

| Tipo | Información propia |
|---|---|
| **Proveedor** | Nombre, código, descripción y datos de contacto opcionales. |
| **Insumo** | Nombre, código, descripción y unidad de compra y uso, por ejemplo kg, metro o unidad. |
| **Producto** | Nombre, código y descripción. El costo y el precio de venta se calculan con sus requisitos y ofertas. Los componentes de fabricación también se registran como productos. |

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

### Costo de producción y precio de venta en COP

En **Productos** aparecen dos columnas calculadas:

- **Costo de producción (COP):** el costo mínimo de los materiales necesarios para una unidad del producto, incluidos los de sus componentes.
- **Precio de venta (COP):** ese costo mínimo más **25 %**.

Ya no escribes un precio manual al crear o editar un producto. Si faltan datos, aparece **Pendiente de calcular**; si hay una contradicción, **Revisar ciclo**. Una oferta válida de cero puede producir un costo cero y no se confunde con un dato ausente.

Para obtener o cambiar estos importes, revisa en **Relaciones** los insumos que usa el producto, sus cantidades, las ofertas de proveedores y los componentes que necesita. Después vuelve a cargar los datos y consulta el producto.

### Comparar proveedores y materiales

1. Pulsa el nombre del producto o **Ver detalles**.
2. En **Costo de producción**, revisa primero **Opción más económica**.
3. Abre **Ver proveedores e insumos** de **Opción más costosa**.
4. Compara el proveedor, la cantidad, el precio unitario y el subtotal de cada material.
5. Revisa el **Precio de venta**, identificado como **Costo mínimo + 25 %**.
6. Abre **Componentes incluidos** para conocer los productos utilizados como componentes y sus cantidades.

Cada opción incluye todos los materiales del producto: la económica toma la menor oferta registrada de cada material; la costosa toma la mayor. Son combinaciones completas de abastecimiento, no el precio de una sola ruta del grafo.

### Cómo se cuentan los componentes

Una conexión **producto → producto** incorpora **una unidad del producto de origen** como componente del dependiente. Si un componente aparece en varias ramas, se suman todos sus usos. También se suman los insumos directos del producto final.

Por ejemplo, si un panel requiere dos unidades de material y el producto necesita ese panel a través de dos componentes distintos, se cuentan cuatro unidades. Los materiales de los componentes ya están incluidos en el desglose: no vuelvas a sumar el componente como otro costo.

Las cantidades del mismo material se agrupan antes de calcular importes. Cada subtotal se redondea a dos decimales, elevando el medio centavo; se suman las filas y sobre el mínimo se aplica el 25 %. El [detalle del cálculo](GRAPH_COSTS_2026-11-16.md) incluye ejemplos.

### Si el cálculo está incompleto

**Cálculo incompleto** indica ofertas ausentes, requisitos sin definir o dependencias que no permiten calcular una cantidad. **Dependencias con ciclo** indica una contradicción entre los requisitos del producto. En ambos casos se explican los motivos y no se presenta un total ni un precio de venta definitivo.

Estos importes solo cubren los materiales calculables. No incluyen mano de obra, impuestos, transporte, capacidad o inventario. La aplicación no comprueba disponibilidad ni cotizaciones externas. Si cambias la unidad de un insumo, revisa que sus precios y cantidades sigan usando esa unidad.

La advertencia de valores de referencia se conserva cuando corresponde a las ofertas o cantidades comparadas. Un valor sin esa marca tampoco implica una cotización comercial verificada.

Los precios manuales de versiones anteriores se conservan por compatibilidad de la API, pero no se usan para calcular los costos ni se muestran como el precio de venta de estas columnas.

## 6. Crear y revisar relaciones

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

## 7. Explorar el grafo

1. Abre **Grafo de abastecimiento**.
2. En **Explorar alrededor de**, conserva **Toda la red** o elige un elemento.
3. Pulsa un nodo para ver sus datos y conexiones en el panel lateral.
4. Usa **Enfocar conexiones** para centrar la exploración en ese elemento.
5. Activa **Mostrar todos los pesos** para ver precios y cantidades.

### Mover nodos y seleccionar un grupo

1. **Arrastra un nodo** con el botón izquierdo para colocarlo libremente. Sus conexiones lo acompañan.
2. Usa **Ctrl + clic** para añadir o retirar nodos del grupo. En macOS también funciona **Cmd + clic**. El contorno discontinuo y el contador indican cuáles están seleccionados para mover.
3. **Arrastra cualquiera de los nodos marcados** para mover el grupo completo, conservando la separación entre ellos.
4. Para seleccionar por rectángulo, activa **Seleccionar área** y arrastra sobre el fondo, o utiliza **Shift + arrastre** sobre el fondo. Se incluyen los nodos que toca el rectángulo; mantén **Ctrl/Cmd** para sumar esa área al grupo existente.
5. Pulsa **Escape** para cancelar un gesto en curso y limpiar la selección para mover. **Limpiar selección** también vacía el grupo.
6. Usa **Restaurar distribución** para volver a las posiciones automáticas. Conserva los elementos y sus relaciones.

Estos controles funcionan también en los grafos de impacto, producción y tablero. Mover nodos o seleccionar un grupo no cambia el producto inspeccionado, las rutas de impacto, la opción de costo ni los resultados del análisis. Un clic normal sigue abriendo el detalle de un solo nodo.

### Teclado y encuadre

| Control | Resultado |
|---|---|
| **+ / −** o rueda del ratón | Acercar o alejar. |
| Arrastrar el fondo sin Shift y con **Seleccionar área** desactivado | Desplazar la vista. Vuelve a pulsar **Seleccionar área** para desactivarlo. |
| Flechas del teclado con foco en el grafo | Desplazar la vista. |
| **Alt + flechas** | Mover 10 unidades del dibujo; **Alt + Shift + flechas** mueve 40. La distancia visible depende del zoom. Con foco en el fondo se mueve el grupo seleccionado; con foco en un nodo, se mueve ese nodo o su grupo. Si no hay grupo, puede moverse el nodo cuyo detalle está abierto. |
| **Tab**, después **Enter** o **Espacio** | Recorrer nodos y abrir su detalle. **Ctrl/Cmd + Enter/Espacio** añade o retira el nodo del grupo. |
| **Ajustar red** | Mostrar el conjunto visible, incluidas las posiciones manuales. |
| **Tamaño legible** | Centrar y ampliar el nodo seleccionado. |
| **Restablecer** | Quitar enfoque y selección, restaurar las posiciones automáticas y volver a la vista general. |

### Qué se conserva

Las posiciones se guardan automáticamente en tu cuenta al terminar de mover los cuadros. Cada sección —Grafo, Impacto, Producción y Tablero— conserva una distribución independiente. Recargar, salir y volver o iniciar sesión recupera las posiciones; también se recuperan desde otro navegador conectado al mismo servidor. Enfocar, alternar costos o mostrar la red completa mantiene el acomodo de los cuadros ocultos.

Espera la confirmación de guardado antes de cerrar. Si falla la conexión, usa el control para reintentar; queda un borrador de coordenadas separado por usuario en ese navegador. **Restaurar distribución** y **Restablecer** guardan la vuelta a las posiciones automáticas. No se guardan zoom, enfoque o selección temporal. La última escritura prevalece si usas la misma cuenta en dos pestañas simultáneamente. [Detalles del guardado y sus límites](USER_GRAPH_LAYOUTS_2026-11-16.md).

La distribución inicial agrupa proveedores, insumos y productos en columnas. Al mover nodos libremente se ocultan esos encabezados para no atribuir tipos a una posición. Una flecha proveedor → insumo muestra precio de compra; una flecha insumo → producto muestra cantidad usada. Las dependencias generales se representan sin peso. Mover un nodo no invierte ninguna relación.

Las flechas animadas se desplazan continuamente del requisito hacia su dependiente. Usa **Pausar flechas** o **Reanudar flechas**. Si activaste movimiento reducido en el sitio o el sistema, aparece **Movimiento reducido** y la animación se detiene; las flechas fijas conservan su dirección.

Al seleccionar un **Producto** en este grafo, el lateral muestra **Costo de producción**, con la opción más económica, la más costosa y el precio calculado. Usa todos los proveedores pertinentes del catálogo, aunque algunos no aparezcan en el dibujo enfocado. Los grafos de escenarios del tablero mantienen su función de impacto y orden; no presentan un costo base como si correspondiera al cambio temporal.

## 8. Analizar el impacto — Feature 2

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

## 9. Obtener un orden de producción — Feature 3

1. Abre **Orden de producción**.
2. En **Qué quieres organizar**, elige **Toda la red de abastecimiento** o un producto concreto.
3. Pulsa **Generar orden**.
4. Si aparece **Dependencias compatibles**, lee las etapas de arriba hacia abajo.
5. Despliega los **requisitos previos** de un elemento para entender su posición.
6. Abre **Ver el orden completo y la red analizada** para explorar el resultado completo.
7. Selecciona un nodo de **Producto** en ese grafo para comparar sus costos y proveedores en el panel lateral. El precio no cambia el orden de las etapas.

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

## 10. Usar el Explorador de API

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

Si utilizas **SalePrice** e **IsPriceReference** al editar un producto por API, esos campos corresponden al precio manual conservado para compatibilidad. No reemplazan el precio calculado que muestran las columnas nuevas. La consulta de estimación devuelve ahora mínimo, máximo, materiales, componentes y el precio calculado.

### Analizar el tablero desde la API — Todos los perfiles

1. En **Operación**, elige **POST · Análisis del tablero**.
2. Revisa **Datos de la solicitud (JSON)**. **ElementId** indica el origen; **ProductId** es el producto cuyo orden quieres consultar, o **null** para toda la red.
3. Conserva **Change: null** para no cambiar dependencias, o usa el cambio **Add** o **Remove** explicado en [el contrato de Feature 4](FEATURE4.md).
4. Pulsa **Analizar escenario** y revisa la respuesta.

Esta opción usa POST para enviar los parámetros, pero **no guarda cambios**. Por eso está disponible para Consulta y no muestra la confirmación de una escritura. El editor permite comprobar un código inexistente o un dato inválido y leer el error correspondiente.

Si aparece **409**, puede haber un duplicado o un cambio posterior a la versión seleccionada. Recarga la página, selecciona de nuevo el registro y revisa la operación antes de repetirla.

## 11. Actualizar Mi cuenta

Pulsa tu nombre en la parte superior para abrir **Mi cuenta**.

- En **Datos básicos**, cambia **Nombre** o **Correo (opcional)** y pulsa **Guardar mis datos**.
- Para cambiar la contraseña, completa **Contraseña actual** y **Nueva contraseña**, de 12 a 128 caracteres, y pulsa **Actualizar contraseña**.

Cambiar la contraseña cierra las sesiones de esa cuenta; deberás volver a iniciar sesión. Desde Mi cuenta no cambias tu usuario ni te asignas otro perfil.

## 12. Crear cuentas y gestionar accesos — Administrador

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

## 13. Contacto público y bandeja privada

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

## 14. Páginas públicas y novedades

Los enlaces públicos permiten abrir **Servicios**, **Nuestra historia**, **Locaciones**, **Contacto**, **Términos y condiciones** y **Novedades**. Puedes volver a la portada mediante **Volver al inicio** o el nombre de AbastecePyme.

Para registrar interés en novedades:

1. Abre **Suscríbete a novedades** en el pie de página.
2. Introduce **Tu correo electrónico**.
3. Marca la autorización para guardarlo en esta instalación.
4. Pulsa **Suscribirme** y espera la confirmación.

Se guarda la solicitud con consentimiento. **El envío de boletines y confirmaciones por correo todavía no está habilitado.** Esta suscripción no crea una cuenta de acceso. Para consultas sobre el registro o su retiro, sigue el canal de atención indicado en los términos de la instalación.

## 15. Pantallas pequeñas y preferencias

En una pantalla estrecha, el botón **Abrir navegación** despliega las secciones. Elige una opción para cerrar el menú y abrirla. Algunas tablas pueden desplazarse horizontalmente dentro de su contenedor; el grafo dispone de sus propios controles de zoom y desplazamiento.

La interfaz se adapta a tamaños de celular, pero esta instalación solo escucha en el equipo donde se inicia. Abrir `127.0.0.1` en otro teléfono no conecta con ese equipo; el acceso desde otros dispositivos requiere configurar un despliegue separado.

En el pie de la página y en el acceso encontrarás:

- **Sonidos: activados/desactivados.** Permite tonos breves al iniciar o cerrar sesión. Están desactivados inicialmente y dependen del soporte y los permisos del navegador.
- **Reducir movimiento: sí/no.** Disminuye las animaciones. La aplicación también respeta la preferencia de movimiento reducido del sistema.

Las preferencias se guardan en ese navegador cuando permite almacenamiento local; no se sincronizan entre dispositivos.

Para recorrer controles con teclado, usa **Tab** y **Mayús+Tab**. El foco visible indica dónde estás. Puedes cerrar una ventana con su botón de cierre o con **Escape**, siempre que no esté bloqueada mientras se guarda una operación.

## 16. Mejoras recientes y evidencia

La entrega de **movimiento libre y selección múltiple del 16 de noviembre** añade arrastre de nodos y grupos, selección por área y controles de teclado al grafo compartido. La verificación completa y la compilación final aprobaron; su [documento específico](FREE_GRAPH_LAYOUT_2026-11-16.md) reúne uso, alcance y evidencia. Es una actualización de interfaz: se recibe con **Ctrl+F5**, sin reiniciar el servidor. Las cifras y los recorridos de las entregas anteriores se conservan a continuación con su fecha.

El **16 de noviembre** se incorporan el enfoque inicial del tablero, las flechas animadas con pausa y movimiento reducido, y la comparación de costos de materiales y componentes. El precio de venta se calcula como mínimo más 25 %. El [documento de grafos y costos](GRAPH_COSTS_2026-11-16.md) registra el contrato, los límites y el estado de verificación de esta entrega.

La verificación de ese día aprobó **108 pruebas Python y 64 JavaScript**, con compilación de la interfaz y revisión visual de costos, enfoque, animación y móvil. Las capturas inicial y posterior a cambiar una oferta muestran importes distintos porque documentan ese cambio controlado. Todo ese recorrido utilizó datos sintéticos; no modificó MariaDB operativa.

El **14 de noviembre**, Feature 4 incorporó el tablero, la comparación temporal de requisitos y los entregables de demostración. Su [matriz de aceptación](FEATURE4.md) y la [evidencia audiovisual](FEATURE4_VIDEO.md) registran los resultados y el estado de comprobación de esta entrega.

Ese día, la verificación completa aprobó **88 pruebas Python y 59 JavaScript**, además de compilar la interfaz. Incluye los casos del tablero sobre datos sintéticos en memoria; no representa una nueva prueba de la base MariaDB operativa.

### Mejoras del 6 de noviembre

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

## 17. Resolver problemas habituales

| Lo que ocurre | Qué hacer |
|---|---|
| La página no abre o aparece un error de conexión | Comprueba que MySQL y la ventana de la aplicación estén abiertos. En esta instalación utiliza el puerto 3001; revisa la dirección mostrada al arrancar. |
| El puerto está ocupado | Si es la instancia correcta, úsala. Para reiniciar, detén esa instancia con Ctrl+C. Si eliges otro puerto al arrancar, abre exactamente esa nueva dirección. |
| Ves una versión anterior o faltan secciones | Comprueba el puerto. Si solo cambió la interfaz, compílala y pulsa Ctrl+F5; no requiere reinicio. Si cambió Python, vuelve a iniciar la aplicación siguiendo SETUP. |
| No aparecen los controles para mover nodos | Con la interfaz actual compilada, pulsa Ctrl+F5. Esta mejora no requiere reiniciar AbastecePyme ni MariaDB. |
| El fondo dibuja un rectángulo en vez de desplazarse | Desactiva Seleccionar área y suelta Shift para volver a arrastrar la vista. |
| La distribución cambió al recargar o salir de la sección | Comprueba que usas la misma cuenta y sección. Espera la confirmación de guardado; si aparece un error, usa Reintentar. Las posiciones se guardan por usuario en el servidor. Restaurar distribución guarda la organización automática. Si actualizas una versión anterior, activa primero el servidor con soporte de preferencias. |
| Aparece un 404 genérico al consultar impacto o producción | Puede estar abierto un servidor anterior. Reinicia la aplicación actual en su puerto habitual y recarga. |
| El tablero devuelve 404 al analizar | La instancia de 3001 puede seguir usando el servidor anterior. Cierra su ventana con Ctrl+C, ejecuta Iniciar.cmd --port 3001 e introduce la contraseña de MariaDB en la entrada oculta. Luego recarga. |
| El 404 dice “Elemento no encontrado” o “Producto no encontrado” | Ese registro puede haber sido eliminado. Recarga el catálogo y selecciona uno existente. |
| Aparece 400 o 422 | Revisa los campos señalados, códigos, formato de correo, consentimiento y valores. En el Explorador de API revisa también el JSON y los decimales. |
| Aparece 401 | La sesión falta o venció. Inicia sesión de nuevo. |
| Aparece 403 | Puede faltar permiso para la operación o haber caducado la validación de la sesión. Revisa tu perfil y recarga. Si trabajas con la API, usa el Explorador integrado del mismo sitio. |
| Aparece 409 | Puede existir un duplicado o alguien pudo editar el registro. Recarga, selecciona la versión actual y revisa antes de repetir. También puede impedir retirar al último administrador activo. |
| Aparece 413 | La solicitud es demasiado grande. Reduce el contenido y respeta los límites de los campos. |
| Aparece 429 | Se han realizado demasiados intentos en poco tiempo. Espera aproximadamente un minuto antes de volver a intentar. |
| Aparece 503 o un mensaje de base de datos | Comprueba MySQL. Si persiste, pide al responsable de la instalación que revise el arranque y la conexión. |
| El costo parece demasiado bajo o está incompleto | Revisa cantidades, ofertas, unidades y usos de componentes. No incluye mano de obra, impuestos ni transporte. Los datos incompletos o cíclicos no producen un total definitivo. |
| No puedo escribir el precio del producto en el formulario | Las columnas actuales son calculadas: costo mínimo y mínimo más 25 %. Revisa los insumos, componentes y ofertas para cambiar el cálculo. |
| No veo movimiento en las flechas | Comprueba si pulsaste Pausar flechas o activaste movimiento reducido en el sitio o el sistema. La dirección se sigue mostrando con flechas fijas. |
| Producción muestra un ciclo | Revisa la cadena indicada y las relaciones; no es una recomendación de producir en ese orden. Corrige los datos y vuelve a analizar. |
| No aparece Editar, Eliminar o Crear usuario | Comprueba el perfil. Consulta no modifica catálogo y solo Administrador crea cuentas. |
| No llega un correo tras contacto, suscripción o creación de cuenta | El envío de correo no está habilitado. Revisa la confirmación en pantalla o consulta al administrador. |

Si una solicitud falla sin confirmar el resultado, comprueba primero si el cambio quedó guardado antes de enviarlo de nuevo. Para reportar un problema, indica sección, mensaje mostrado y acción que intentabas realizar; no incluyas contraseñas.

## 18. Documentación complementaria

- [Instalación, arranque, respaldo y verificación](SETUP.md).
- [Movimiento libre, selección múltiple y controles de teclado](FREE_GRAPH_LAYOUT_2026-11-16.md).
- [Guardado por usuario, reintentos y animación continua](USER_GRAPH_LAYOUTS_2026-11-16.md).
- [Grafos, costos y precio calculado del 16 de noviembre](GRAPH_COSTS_2026-11-16.md).
- [Tablero, comparación temporal y aceptación de Feature 4](FEATURE4.md).
- [Demostración aislada y evidencia del video](FEATURE4_VIDEO.md).
- [Pitch de 60–90 segundos](FEATURE4_PITCH.md) y [presentación visual](media/Feature4-Pitch.html).
- [Detalle del análisis de impacto](FEATURE2.md).
- [Significado del orden, ciclos y criterios de producción](FEATURE3.md).
- [Ampliación de catálogo y precios del 2 de noviembre](CATALOG_UPDATE_2026.md): describe el estado de aquella entrega; tus datos pueden haber cambiado.
- [Contacto y preferencias de movimiento](CONTACT_AND_MOTION.md).
- [Revisión y evidencia del 6 de noviembre](REVIEW_2026-11-06.md).

Para una comprobación local, **Verificar.cmd** o **npm run check** ejecutan las pruebas y una compilación temporal sin pedir credenciales de MariaDB. No sustituyen una prueba de integración con una base destinada a pruebas ni una revisión visual. El procedimiento completo está en SETUP.


## Animación de la ilustración principal

En el homepage, los puntos de «Red de producción» se mueven continuamente. La última actualización aumenta su velocidad otro **20 %** respecto a la versión anterior. Usa **Pausar animación** o **Reanudar animación** bajo la ilustración. **Reducir movimiento** en el sitio o en el sistema mantiene la imagen estática. Para ver el ajuste con la interfaz ya compilada, recarga con **Ctrl+F5**; no requiere reiniciar la aplicación.

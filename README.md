# AbastecePyme

Aplicación web local para organizar proveedores, insumos, productos y dependencias de producción. Combina **React**, **FastAPI/Python** y **MariaDB**, con un grafo interactivo, precios y cantidades, análisis de impacto y orden de preparación.

**Documentación actualizada: noviembre de 2026.** Incluye distribución de grafos guardada por cuenta, animación continua de la portada con un aumento adicional del 20 % de velocidad, movimiento libre y selección múltiple de nodos, filtros y orden del catálogo, resumen de datos por completar, controles de lectura del grafo y respuesta optimizada del tablero, además de la comparación de costos, resaltado de opciones y funciones anteriores. La evidencia se registra por fecha; el video del 14 de noviembre corresponde a aquella versión.

## Funcionalidades

| Sección | Qué permite hacer |
|---|---|
| Inicio y páginas públicas | Conocer servicios, historia, ubicación, contacto, términos y novedades; acceder al inicio de sesión. |
| Resumen | Ver cantidades y grupos de datos por revisar, con acceso directo al catálogo filtrado y a las demás funciones. |
| Tablero de abastecimiento | Simular la indisponibilidad de un elemento, ver impacto y producción juntos, destacar el camino o ciclo y comparar una dependencia temporal sin guardarla. |
| Catálogo | Buscar y gestionar proveedores, insumos y productos; filtrar costos incompletos, ciclos, referencias e insumos sin ofertas, y ordenar importes con precisión. Los productos muestran costo mínimo de materiales y componentes y precio calculado en COP; sus detalles comparan mínimo y máximo. |
| Relaciones | Gestionar precios de proveedores, cantidades de insumos por producto y dependencias generales. |
| Grafo de abastecimiento | Mover nodos o grupos, seleccionar por área, explorar conexiones con flechas animadas, pausa y movimiento reducido; enfocar, mostrar pesos, comparar costos y resaltar proveedores, insumos y conexiones de cada opción. |
| Análisis de impacto | Consultar consecuencias potenciales o requisitos directos e indirectos y explicar cada resultado mediante un camino. |
| Orden de producción | Obtener etapas y un orden válido para toda la red o un producto; identificar un ciclo cuando existe contradicción. |
| Explorador de API | Consultar información y, según el perfil, crear, actualizar o eliminar elementos y relaciones con revisión previa. |
| Usuarios y perfiles | Crear cuentas y gestionar su perfil y estado; disponible solo para administradores. |
| Mi cuenta | Actualizar nombre y correo propios y cambiar la contraseña. |
| Solicitudes de contacto | Consultar mensajes recibidos, paginar y marcarlos como leídos; disponible solo para administradores. |
| Preferencias | Activar sonidos breves de sesión o reducir el movimiento. Se respetan también las preferencias de movimiento reducido del sistema. |

El formulario público de Contacto registra la solicitud en MariaDB con consentimiento. La suscripción a novedades registra el correo autorizado. **El envío de notificaciones y boletines por correo no está configurado.**

## Accesos y perfiles

Las cuentas del sitio son diferentes de la cuenta de conexión a MariaDB. **No existe registro público:** solo un administrador puede crear usuarios después de iniciar sesión.

| Acción | Consulta | Editor | Administrador |
|---|:---:|:---:|:---:|
| Consultar catálogo, relaciones, grafo, estimaciones, impacto y producción | Sí | Sí | Sí |
| Analizar y comparar un escenario temporal en el tablero | Sí | Sí | Sí |
| Usar consultas del Explorador de API | Sí | Sí | Sí |
| Crear, editar y eliminar elementos y relaciones | — | Sí | Sí |
| Crear cuentas, cambiar perfiles y activar/desactivar accesos | — | — | Sí |
| Leer solicitudes de contacto y marcarlas como leídas | — | — | Sí |
| Modificar datos y contraseña de su propia cuenta | Sí | Sí | Sí |

Los perfiles internos son `Viewer`, `Editor` y `Administrator`. El servidor comprueba los permisos, además de los controles de la interfaz. Cambiar el acceso de una cuenta revoca sus sesiones; cambiar la contraseña obliga a iniciar sesión nuevamente. No se permite dejar el sistema sin un administrador activo. Las contraseñas del sitio se almacenan como hashes Argon2id.

## Abrir la instalación actual

En el equipo de desarrollo se utiliza [http://127.0.0.1:3001/](http://127.0.0.1:3001/).

1. Inicia **MySQL** desde XAMPP.
2. Si AbastecePyme está detenido, abre PowerShell en la carpeta del proyecto y ejecuta:

   ```powershell
   .\Iniciar.cmd --port 3001
   ```

3. Escribe la contraseña de MariaDB cuando la solicite la consola; la entrada es oculta.
4. Abre la dirección indicada por el servidor e inicia sesión con tu cuenta del sitio.

`Iniciar.cmd` sin argumentos usa el puerto **3000**. El puerto **3001** corresponde a esta instalación; en otra copia debes usar la dirección que indique su arranque. Una instancia antigua en otro puerto puede mostrar una versión diferente.

**Activación de la entrega anterior (resaltado), el 16 de noviembre:** el usuario confirmó el reinicio. La aplicación de 3001 respondió **HTTP 200** y su página de entrada coincidió byte por byte con la compilación de aquella entrega en `frontend/dist`. La consulta de estimación sin sesión devolvió **401**, como corresponde. No se realizó una consulta autenticada de costos sobre MariaDB; su validación funcional sigue siendo la de la instancia sintética descrita en la documentación.

En futuras actualizaciones del servidor, detén la instancia anterior de AbastecePyme con **Ctrl+C** y ejecuta `.\Iniciar.cmd --port 3001` con MySQL iniciado. Introduce la contraseña en la entrada oculta y recarga. El reinicio de la actualización de legibilidad y rendimiento también quedó confirmado: HTTP 200 e interfaz compilada coincidente. Los recorridos funcionales se verificaron con datos sintéticos, sin consultar la base operativa autenticada.

**Distribución guardada por usuario:** su reinicio en 3001 ya quedó confirmado. La última mejora de velocidad de la portada solo cambia la interfaz: una vez compilada, basta con **Ctrl+F5**, sin reiniciar. Si actualizas una copia anterior al guardado por cuenta, esa ampliación sí incorpora Python y la tabla `UserGraphLayouts`: compila, reinicia AbastecePyme con `Iniciar.cmd --port 3001` e introduce la contraseña de MariaDB en el aviso oculto. No reinicies MariaDB. [Detalles y uso](docs/USER_GRAPH_LAYOUTS_2026-11-16.md).

## Instalar una copia nueva

### Requisitos

- Windows y Python 3.14.
- Node.js 22.12 o posterior; el proyecto se comprobó con Node 24.
- MariaDB 10.4 o posterior y el controlador **MySQL ODBC 26.7 Unicode Driver**.
- Cuenta MariaDB `usuario_python3`, preparada por el administrador del servidor con `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `CREATE`, `ALTER` e `INDEX` sobre `AbastecePyme.*`. No requiere `DROP`.

La conexión no secreta está en `backend/Database.py`: servidor `127.0.0.1`, puerto `3306`, base `AbastecePyme`. La aplicación utiliza esa base independiente; no utiliza `gym_admin_management` ni requiere reparar tablas del sistema.

### Pasos

1. Descomprime el proyecto e inicia MariaDB en XAMPP.
2. Ejecuta **Instalar.cmd**. Crea el entorno Python, instala las dependencias fijadas y compila React.
3. Ejecuta **ConfigurarAdministrador.cmd** una vez. Introduce la contraseña de MariaDB y crea el primer usuario del sitio con una contraseña de 12 a 128 caracteres.
4. Opcionalmente, ejecuta **AmpliarCatalogo.cmd** para incorporar los proveedores alternativos, componentes, dependencias y precios de referencia de la ampliación.
5. Ejecuta **Iniciar.cmd** o **Iniciar.cmd --port 3001** y abre la dirección que aparezca.
6. Desde **Usuarios y perfiles → Crear usuario**, agrega las demás cuentas.

Los programas solicitan la contraseña de MariaDB de forma oculta y la mantienen en memoria; no la incluyas en archivos, argumentos ni en el repositorio.

La semilla `data/catalogo.json` contiene 40 elementos y 92 relaciones. Su huella se verifica al iniciar: **no edites ese archivo para cambiar el catálogo operativo**. Usa la aplicación o la API. La ampliación es independiente del arranque normal y registra su aplicación para no duplicar datos ni sobrescribir ediciones posteriores.

Como referencia histórica, la lectura del 6 de noviembre de 2026 mostró 48 elementos y 128 relaciones. Los totales y el número de proveedores por insumo pueden cambiar al editar el catálogo; no son restricciones permanentes. Detalles en [Catálogo y precios](docs/CATALOG_UPDATE_2026.md).

## Cómo se interpreta la red

La dirección elegida es **requisito → dependiente**. Una relación `A → B` significa: **«Para preparar B, necesito A»**. No es un enlace sin dirección: invertirlo cambia su significado.

| Relación | Peso y significado |
|---|---|
| Proveedor → insumo | Precio de compra en COP por una unidad del insumo. Varios proveedores representan alternativas de compra. |
| Insumo → producto | Cantidad de unidades del insumo necesarias por una unidad de producto. |
| Dependencia general | Un elemento debe estar preparado antes que otro. No lleva precio ni cantidad. |

Los códigos y tipos de los elementos permanecen estables después de crearlos. Eliminar un elemento elimina también sus relaciones; eliminar una relación conserva sus elementos. La interfaz informa y solicita confirmación antes de eliminar.

### Organizar visualmente el grafo

Estos controles están disponibles en el grafo compartido de abastecimiento, impacto, producción y tablero:

| Acción | Control |
|---|---|
| Mover un nodo | Arrastrarlo con el botón izquierdo. |
| Añadir o retirar nodos del grupo | **Ctrl/Cmd + clic** sobre cada nodo. |
| Seleccionar por rectángulo | Activar **Seleccionar área** y arrastrar el fondo, o usar **Shift + arrastre** sobre el fondo. Con **Ctrl/Cmd**, sumar el área al grupo actual. |
| Mover el grupo | Arrastrar cualquiera de sus nodos marcados; conservan su separación. |
| Mover con teclado | **Alt + flechas** mueve 10 unidades del dibujo; **Alt + Shift + flechas**, 40. Desde el fondo se mueve el grupo seleccionado; un nodo con foco puede mover su grupo o moverse solo. |
| Cancelar o limpiar | **Escape** cancela el gesto en curso y limpia la selección para mover. |
| Volver a la distribución automática | **Restaurar distribución** devuelve los nodos a sus posiciones iniciales. |

Para desplazar la vista, arrastra el fondo con **Seleccionar área** desactivado y sin Shift. La selección para mover es independiente del producto inspeccionado y de los resaltados de costos o caminos: mover nodos no cambia relaciones, cantidades, precios ni resultados.

Las posiciones terminadas se guardan automáticamente en la cuenta, de forma independiente para Grafo, Impacto, Producción y Tablero. Se recuperan al recargar, volver a la sección o iniciar sesión, incluso desde otro navegador conectado al mismo servidor. Enfocar, alternar costos o mostrar el grafo completo conserva el acomodo. **Restaurar distribución** guarda el retorno a las posiciones automáticas. El zoom y la selección no se guardan. [Guardado, recuperación y límites](docs/USER_GRAPH_LAYOUTS_2026-11-16.md).

### Costos mínimo/máximo y precio de venta calculado

**Catálogo → Productos** muestra **Costo de producción (COP)** —el mínimo calculable— y **Precio de venta (COP)** —ese mínimo más **25 %**—. Ambos son valores derivados; el formulario de producto ya no solicita un precio manual.

Al abrir un producto se presentan primero **Opción más económica** y después **Opción más costosa**, con proveedor, material, cantidad y subtotal. El mismo panel aparece al seleccionar un producto en el grafo de abastecimiento o en el grafo desplegado de Orden de producción.

El cálculo incluye cantidades de insumos directos y de componentes anidados. Cada dependencia **producto → producto** consume **una unidad** del componente de origen. Los usos compartidos se suman; después se agregan las cantidades de cada material. Cada subtotal se redondea a dos decimales con **HALF_UP**, los totales suman esas filas y el precio de venta es **mínimo × 1,25**, también redondeado a dos decimales.

Mínimo y máximo representan **combinaciones completas de abastecimiento**, eligiendo respectivamente la menor o mayor oferta de cada material. No son el costo de una única ruta del grafo. Si faltan requisitos/ofertas o hay un ciclo que afecta al producto, los importes finales son `null` y la interfaz explica el motivo. Una oferta válida de cero conserva un costo cero.

Se mantienen las advertencias de valores de referencia. No se verifican cotizaciones, inventario ni disponibilidad; no se incluyen mano de obra, transporte o impuestos. Revisa que precios y cantidades usen la misma unidad del material. Las reglas, ejemplos de redondeo y contrato están en [Grafos y costos](docs/GRAPH_COSTS_2026-11-16.md).

El **precio manual anterior** permanece en `ProductPrices` y en los campos de compatibilidad de la API. Consultar costos no lo modifica ni lo utiliza como costo de un componente; la nueva interfaz muestra únicamente el precio calculado.

## Feature 2 — Qué significa “alcanzar” en este negocio

En AbastecePyme, **alcanzar un elemento significa llegar a él siguiendo una cadena de dependencias del grafo**.

Por ejemplo:

**Maderas del Norte → Tablero de madera → Mesa de trabajo**

Desde **Maderas del Norte**, se alcanza directamente el tablero e indirectamente la mesa. Esto expresa que una dificultad de suministro del proveedor **podría repercutir** en el insumo y el producto. Es impacto potencial: puede existir otro proveedor, inventario o disponibilidad que el sistema no registra. Los pesos monetarios y las cantidades no determinan el alcance.

En **Análisis de impacto**, selecciona el elemento, elige **Consecuencias** o **Requisitos** y pulsa **Analizar conexiones**. En Requisitos se recorre en sentido inverso para explicar de qué depende un producto, conservando la dirección almacenada de las flechas. **Ver camino** muestra una explicación y la resalta en el grafo.

El recorrido BFS propio de Python utiliza adyacencias, una cola y elementos visitados. Evita repeticiones y bucles ante ciclos. Devuelve un camino de menor número de relaciones hacia cada resultado, no todos los caminos ni el más barato. El origen se visita, pero no se cuenta como otro afectado. Una relación representa alcance directo; dos o más, indirecto.

Se diferencia un elemento inexistente (`404`) de un elemento existente sin alcanzados (`200`, `NoReachable`) y de una cadena alcanzada (`200`, `Reached`). Los totales dependen del catálogo vigente. **Alcance no equivale a orden de fabricación.** Contrato y pruebas en [Feature 2](docs/FEATURE2.md).

## Feature 3 — Orden y ciclos de producción

La sección **Orden de producción** responde a los cuatro requisitos del brief:

| Requisito de Feature 3 | Respuesta de la aplicación |
|---|---|
| Generar un orden válido sin ciclos | El algoritmo de Kahn, escrito en Python, prepara primero los elementos sin requisitos pendientes y devuelve una secuencia completa y etapas. |
| Detectar y reportar un ciclo útil | Un DFS iterativo propio devuelve una cadena cerrada con nombres, códigos y relaciones. La pantalla explica cada dependencia y destaca el ciclo en el grafo. |
| No presentar un orden falso | Si existe un ciclo en el alcance analizado, `Order=[]` y `Stages=[]`; se muestra **Ciclo detectado** y los pendientes, sin ofrecer una secuencia parcial como solución. |
| Aceptación de un caso acíclico y uno cíclico | Las pruebas de `tests/python/test_production.py` comprueban ambos casos y otros límites con datos en memoria. El procedimiento y los resultados están en [Feature 3](docs/FEATURE3.md). |

Selecciona **Toda la red de abastecimiento** o un producto y pulsa **Generar orden**. Para un producto se incluyen sus requisitos directos e indirectos. Dentro de una etapa puedes elegir el orden; entre etapas debes respetar la precedencia. **Ver el orden completo y la red analizada** permite explorar el resultado.

Un ciclo ajeno al producto seleccionado no invalida su análisis, pero sí el de toda la red. Los elementos pendientes incluyen dependientes del ciclo; no todos pertenecen al ciclo mostrado. Después de corregir una relación, vuelve a analizar, porque puede haber otros ciclos.

El análisis es de precedencia: no calcula tiempos, inventario, capacidad ni fechas. Las ofertas son alternativas; el orden no obliga a comprar a todos los proveedores. Los algoritmos usan estructuras de Python, **sin bibliotecas de grafos**.

## Feature 4 — Tablero de abastecimiento demostrable

El **Tablero de abastecimiento** reúne la selección del catálogo, las consecuencias potenciales de una indisponibilidad y el orden o ciclo de producción. Ambos análisis utilizan la misma lectura de datos. Puedes destacar un camino para explicar un resultado y revisar la cadena que causa una contradicción.

Desde el 16 de noviembre, el dibujo empieza en **Enfocar conexiones**, con toda la cadena de consecuencias del origen; en modo ciclo se muestran sus elementos y relaciones. **Mostrar grafo completo** amplía la vista sin cambiar el análisis. Las flechas se desplazan continuamente del requisito al dependiente; **Pausar flechas**, **Reanudar flechas** y la preferencia de movimiento reducido controlan la animación.

El cambio de requisitos se prueba como una comparación temporal: **añadir una dependencia general o retirar una relación existente**. La respuesta presenta la red registrada, el escenario y sus diferencias. El ensayo no guarda cambios ni elimina el elemento seleccionado para aparentar que las dependencias desaparecieron.

Las altas temporales proveedor → insumo e insumo → producto se rechazan porque requieren precio o cantidad. Usa **Relaciones** para registrar o editar esas conexiones con sus valores. Las modificaciones reales siguen reservadas a Editor y Administrador; todos los perfiles pueden simular.

| Criterio del brief | Correspondencia |
|---|---|
| Catálogo, impacto y orden/alerta integrados | Selección única y resultados base/escenario obtenidos con una lectura de catálogo por solicitud. |
| Red y consulta relevante visibles | Grafo con camino de impacto o ciclo destacado. |
| Adaptar el cambio de requisitos | Comparación de una dependencia añadida o retirada, con diferencias de alcanzados y estado de producción. |
| Cadena válida, inexistente, dato inválido y ciclo | Casos identificados en la [matriz de aceptación](docs/FEATURE4.md); el ciclo no devuelve un orden parcial como solución. |
| Documentación, bitácora, video y pitch | Guía, [bitácora IA](docs/bitacora-ia.md), video real y presentación visual enlazados desde esta entrega. Su estado de comprobación está en [la evidencia audiovisual](docs/FEATURE4_VIDEO.md). |

La [demostración aislada](docs/FEATURE4_VIDEO.md) permite registrar una dependencia y repetir el recorrido con datos sintéticos en memoria. Su cuenta de prueba solo sirve allí; no es una cuenta de la instalación normal. No requiere MariaDB.

La simulación indica **impacto potencial**, no falta de inventario confirmada. La producción sigue expresando precedencia, sin fechas ni capacidad. Los resultados y archivos de Feature 4 se documentan por separado de las cifras históricas de las entregas anteriores.

## API

La interfaz y la API se sirven desde el mismo origen. El **Explorador de API** ofrece catálogo, grafo, impacto, estimación, producción, análisis del tablero y creación/edición/eliminación de elementos y relaciones. Para escribir, muestra **Revisar cambio → Confirmar y ejecutar**. **POST · Análisis del tablero** está disponible para todos los perfiles: **Analizar escenario** ejecuta una consulta, sin confirmación de escritura ni cambios guardados. Las rutas de cuentas y contacto se utilizan desde sus pantallas específicas.

| Método y ruta | Acceso / función |
|---|---|
| `POST /api/auth/login` | Público; iniciar sesión. |
| `GET /api/auth/session` | Usuario autenticado; leer la sesión propia. |
| `POST /api/auth/logout` | Usuario autenticado; cerrar sesión. |
| `GET /api/catalog`, `GET /api/graph` | Todos los perfiles; catálogo y red. |
| `GET /api/products/{ProductId}/estimate` | Todos los perfiles; costos mínimo/máximo de materiales y componentes, desglose y venta calculada. |
| `GET /api/impact/{ElementId}?Direction=Consequences` | Todos los perfiles; impacto. Acepta también `Requirements`. |
| `GET /api/production` | Todos los perfiles; orden/ciclos de toda la red. |
| `GET /api/production?ProductId=PROD-ESTANTE` | Todos los perfiles; producto y requisitos. Sustituye el ID si no existe en tu catálogo. |
| `POST /api/dashboard/analyze` | Todos los perfiles; análisis base y escenario temporal. Recibe `ElementId`, `ProductId` opcional y `Change` opcional. No guarda el ensayo. |
| `POST /api/elements` | Editor/Administrador; crear elemento. |
| `PATCH /api/elements/{ElementId}` | Editor/Administrador; editar con `Version`. |
| `GET /api/elements/{ElementId}/deletion-preview` | Editor/Administrador; revisar el efecto de eliminar. |
| `DELETE /api/elements/{ElementId}?Version=N` | Editor/Administrador; eliminar elemento y sus relaciones. |
| `POST`, `PATCH`, `DELETE /api/relations` | Editor/Administrador; crear, editar o eliminar una relación. Edición y eliminación requieren `Version` en el cuerpo. |
| `GET`, `POST /api/users` | Administrador; listar y crear cuentas. |
| `PATCH /api/users/{UserId}` | Administrador; cambiar perfil/estado con `Version`. |
| `PATCH /api/account` | Usuario autenticado; modificar sus datos básicos. |
| `POST /api/account/password` | Usuario autenticado; cambiar su contraseña. |
| `GET /api/account/graph-layouts/{LayoutKey}` | Todos los perfiles; recuperar únicamente la distribución de su propia cuenta. |
| `PUT /api/account/graph-layouts/{LayoutKey}` | Todos los perfiles; guardar o restablecer su distribución. `LayoutKey`: `Graph`, `Impact`, `Production` o `SupplyBoard`. No modifica el catálogo. |
| `POST /api/contact-requests` | Público; guardar solicitud con consentimiento. |
| `GET /api/contact-requests?Limit=20&Offset=0` | Administrador; consultar la bandeja paginada. |
| `PATCH /api/contact-requests/{RequestId}/read` | Administrador; marcar como leída. |
| `POST /api/subscriptions` | Público; registrar correo con consentimiento. |

Las escrituras autenticadas y el análisis POST del tablero exigen sesión, `X-CSRF-Token` y `Origin` válido; las pantallas los incorporan. Los formularios públicos también validan el origen y sus datos. No existe `POST /api/auth/register`. El [contrato del tablero](docs/FEATURE4.md) describe los campos, errores y comparación sin persistencia.

Los importes viajan como texto decimal, por ejemplo `"35000.00"` o `"2.5000"`. Se usan `Decimal` en Python y columnas decimales en MariaDB. En las escrituras de compatibilidad, `SalePrice: null` retira el precio manual anterior; omitirlo conserva el valor. `IsPriceReference` corresponde a ese precio manual; `IsDemo` se conserva como nombre interno para los pesos de referencia de relaciones.

Esa escritura de `SalePrice` mantiene el **precio manual de compatibilidad**. En lectura, cada producto del catálogo añade `ProductCosts`; su `SalePrice` y el devuelto por `/estimate` son **calculados**, no persistidos. `MinimumCost`, `MaximumCost` y ese precio calculado son `null` con `Status: Incomplete` o `Cycle`. El [contrato del 16 de noviembre](docs/GRAPH_COSTS_2026-11-16.md) distingue ambos significados y detalla `Materials`, `Components` e `Issues`.

Los conflictos de versión o duplicados producen `409`; los datos inválidos, `422`; falta de sesión, `401`; falta de permiso, `403`. Un ciclo de producción devuelve `200` con `Status: Cycle`: el análisis funcionó y encontró una contradicción. Los cálculos de impacto, orden y estimación son consultas; no cambian existencias ni crean órdenes persistentes.

## Contacto y páginas públicas

Los datos publicados se centralizan en `frontend/src/BusinessInfo.js`: **Centro de Negocios One Plaza, Torre Norte, piso 3, Carrera 43A #5A-113, El Poblado, Medellín, Colombia**, teléfono **+57 311 335 2866** y enlace al mapa.

El formulario de Contacto muestra avance mientras se completan los datos, la solicitud y el consentimiento. Las solicitudes se guardan en la bandeja privada de administradores, con referencia y estado de lectura. No se publican mensajes de visitantes y no se envían correos automáticamente. La suscripción a novedades registra el consentimiento de manera separada.

Las páginas de servicios, historia, locaciones y términos forman parte de la presentación del proyecto. La página no implementa compras, pagos, inventario real ni facturación.

## Cómo está construida

| Capa | Responsabilidad |
|---|---|
| React + Vite | Navegación, formularios, perfiles visibles, estados de carga y componentes cargados bajo demanda. |
| SVG propio | Dibujo del grafo, selección, resaltado de caminos, zoom y desplazamiento. |
| FastAPI + Python | API, validación, permisos, sesiones, operaciones de catálogo y algoritmos BFS/Kahn/DFS. |
| MariaDB + ODBC | Catálogo, precios, relaciones, usuarios, sesiones, suscripciones, solicitudes y registro de migraciones. |

```text
backend/          API, reglas, algoritmos y modelo de datos
frontend/src/     Interfaz React y estilos
frontend/public/  Imágenes WebP en varias resoluciones
frontend/dist/    Compilación generada; no se versiona
data/catalogo.json   Semilla protegida de importación
tests/            Pruebas Python, JavaScript e integración separada
docs/             Guía, diseño de datos y evidencia de las funcionalidades
Start.py          Arranque de FastAPI y migraciones pendientes
SetupAdmin.py     Configuración del primer administrador
EnrichCatalogue.py   Ampliación opcional del catálogo
Verify.py         Verificación sin conexión a MariaDB
```

Los identificadores propios se escriben en inglés y PascalCase, de acuerdo con el criterio del proyecto; se respetan los nombres exigidos por Python, React y HTML. El [modelo relacional](docs/SCHEMA.md) describe claves, tipos y persistencia.

## Mejoras del 6 de noviembre de 2026

- Resumen orientado a tareas, navegación compacta, tablas más legibles, pie de página renovado y corrección del desbordamiento móvil observado.
- Foco visible, ayudas de campos asociadas a sus controles, cierre de diálogos coherente durante guardados y controles adaptados a móvil.
- Fotografías WebP con anchos de 640, 960 y 1536 px: el conjunto público pasó de **11.077.060 a 1.698.076 bytes**, aproximadamente **84,7 % menos**, incluyendo todas las variantes.
- Carga diferida de secciones, reutilización del dibujo del grafo y menos búsquedas repetidas al calcular su distribución.
- Cancelación de lecturas sustituidas y descarte de respuestas antiguas para proteger la selección y la sesión vigentes.
- Consultas SQL acotadas para operaciones individuales y cierre de conexiones cuando falla su configuración.
- Compresión de respuestas elegibles; caché prolongada para paquetes con huella, revalidación para imágenes y `no-store` para API, HTML y errores.
- Verificación unificada y separación de los datos de la versión anterior respecto a la semilla protegida.

Consulta el [informe de revisión](docs/REVIEW_2026-11-06.md) para mediciones, archivos modificados y límites de la evidencia.

## Desarrollo y comprobaciones

Después de editar la interfaz:

```powershell
npm run build --prefix frontend
```

Si el servidor ya está activo y solo cambiaste la interfaz, pulsa **Ctrl+F5** después de compilar; no lo reinicies. Si está detenido, inícialo con `.\Iniciar.cmd --port 3001`. Si cambiaste Python, detén el proceso con **Ctrl+C** en su consola y vuelve a iniciarlo. No hace falta reiniciar MariaDB. `npm run dev --prefix frontend` sirve Vite de forma aislada; no conecta la sesión ni la API. Usa el flujo anterior para probar la aplicación completa.

Para comprobar el proyecto:

```powershell
.\Verificar.cmd
```

El verificador comprueba sintaxis, pruebas Python/JavaScript y compilación en una carpeta temporal. No necesita MariaDB ni credenciales y no reemplaza `frontend/dist`. Algunas pruebas HTTP levantan y cierran un servidor local temporal.

| Comando | Alcance |
|---|---|
| `npm run check` | Verificación completa, incluida compilación. |
| `npm test` | Pruebas Python/JavaScript sin compilar. |
| `npm run test:python` | Pruebas Python sin MariaDB. |
| `npm run test:node` | Pruebas JavaScript. |
| `npm run test:legacy` | Regresiones de la versión anterior y protección de la semilla. |

La revisión del **6 de noviembre** registró **68 pruebas Python y 48 JavaScript aprobadas**, además de compilación y comprobación visual. Son resultados fechados, no una afirmación de que esa suite completa se haya repetido en cada edición documental. La aceptación actualizada de Feature 3 está en [su documento](docs/FEATURE3.md).

El **14 de noviembre**, `Verify.py` completo aprobó **88 pruebas Python y 59 JavaScript**, además de la compilación Vite de **49 módulos**. La aceptación de Feature 4 incluye los cinco casos de `tests/python/test_supply_board_journey.py`, con autenticación y API reales sobre un adaptador SQLite en memoria; están incluidos en ese total. La [matriz y evidencia](docs/FEATURE4.md) distinguen esta comprobación de una integración con MariaDB. La [guía del video](docs/FEATURE4_VIDEO.md) permite repetir el recorrido visual en la misma clase de entorno aislado.

El **16 de noviembre**, la verificación completa aprobó **108 pruebas Python y 64 JavaScript (172)** y compiló **53 módulos**. Se comprobaron costos compartidos entre pantallas, actualización tras cambiar una oferta, enfoque/completo, movimiento/pausa, movimiento reducido y móvil con datos sintéticos. Las [capturas y resultados](docs/GRAPH_COSTS_2026-11-16.md) distinguen el estado inicial y el posterior a editar la oferta. No se probó esta versión contra MariaDB operativa.

`tests/RunChecks.py` es una integración separada que crea y retira datos de prueba en MariaDB. No se ejecuta con el verificador general; úsala únicamente en una base destinada a pruebas. Los detalles están en [SETUP](docs/SETUP.md).

## Problemas frecuentes

| Situación | Qué revisar |
|---|---|
| La página no abre | MySQL debe estar iniciado y la consola de AbastecePyme abierta. Usa la URL que indica esa consola. |
| Impacto o producción responde `404` para todo | Comprueba que no estés usando el servidor antiguo del puerto 3000. Reinicia el proceso de AbastecePyme actualizado en 3001 y recarga. |
| La nueva ruta del tablero responde `404` | Puede seguir abierto el servidor anterior de 3001. Detén esa ventana con Ctrl+C, ejecuta Iniciar.cmd --port 3001, introduce la contraseña de MariaDB en la entrada oculta y recarga. |
| Dice «Elemento/Producto no encontrado» | Recarga el catálogo y selecciona un registro que todavía exista. |
| No aparece Crear usuario | Solo se muestra con una sesión de Administrador. |
| No aparecen Editar o Eliminar | El perfil Consulta es de lectura. Un administrador puede revisar el acceso. |
| El cambio del tablero no aparece guardado en Relaciones | Es un escenario temporal. Para modificar el catálogo usa las operaciones persistentes con un perfil Editor o Administrador. |
| El tablero rechaza añadir proveedor → insumo o insumo → producto | Esas conexiones necesitan un precio o una cantidad. Regístralas en Relaciones; las altas temporales admiten dependencias generales. |
| Error `409` al guardar | Otra edición cambió la versión o hay un duplicado. Recarga y revisa los datos antes de repetir. |
| MariaDB informa permiso denegado | Revisa con el administrador los privilegios de `usuario_python3` sobre `AbastecePyme.*`; el rol de la página no concede permisos SQL. |
| No llegan correos | El envío está aplazado. Los mensajes de Contacto se leen dentro de la bandeja del administrador. |

## Compartir en GitHub y conservar datos

Sube el contenido del proyecto, manteniendo `backend`, `frontend/src`, `frontend/public`, el lockfile de la interfaz, `tests`, `docs`, los programas de instalación/arranque/verificación y la semilla original. No sustituyas los archivos fuente por el ZIP dentro del repositorio.

Conserva también `docs/media/Feature4-Recorrido.webm`, `docs/media/Feature4-Recorrido.srt`, `docs/media/Feature4-Pitch.html` y sus guiones: son entregables de Feature 4. No sustituyas el video por un enlace a un archivo que solo exista fuera del proyecto.

No subas contraseñas, archivos de configuración secretos, bases exportadas, cuentas o sesiones reales, respaldos, `work`, `.venv`, `node_modules`, `runtime` ni `frontend/dist`. El `.gitignore` excluye estos recursos locales; conserva también `.gitattributes` para no alterar los bytes de la semilla. Cada instalación configura su propia cuenta del sitio y su conexión MariaDB.

Antes de futuras migraciones, conserva un respaldo consistente de estructura y datos. La aplicación escucha únicamente en `127.0.0.1`: compartir el código en GitHub **no publica un sitio accesible por Internet**. El despliegue externo requiere una preparación específica.

### Versión anterior

`src/` y `public/` conservan la implementación educativa anterior como referencia. `node src/server.js` la inicia explícitamente y guarda sus ediciones en `data/catalogo-legacy.json`, una copia independiente. No sincroniza con MariaDB y rechaza usar la semilla protegida como archivo operativo. La aplicación actual está en `backend/` y `frontend/`.

### Skills y apoyos utilizados

En la revisión reciente se instaló **web-design-guidelines** dentro del proyecto para comprobar criterios de interfaz y accesibilidad. Se utilizaron **frontend-design** para composición visual, **vercel-react-best-practices** para React, **systematic-debugging** para diagnóstico y **verification-before-completion** para verificación. Las decisiones y los límites de la revisión se documentan en el [informe técnico](docs/REVIEW_2026-11-06.md).

Feature 4 reutiliza las guías existentes de planificación, diseño, React, pruebas y verificación. Se evaluó Playwright y se utilizó el navegador integrado para las comprobaciones visuales. La [bitácora](docs/bitacora-ia.md) separa decisiones, trabajo asistido y evidencia fechada. No se atribuye al equipo una revisión personal que aún no haya realizado.

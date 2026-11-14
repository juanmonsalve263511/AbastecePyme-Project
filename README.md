
# AbastecePyme

Aplicación web local para organizar proveedores, insumos, productos y dependencias de producción. Combina **React**, **FastAPI/Python** y **MariaDB**, con un grafo interactivo, precios y cantidades, análisis de impacto y orden de preparación.

## Funcionalidades

| Sección | Qué permite hacer |
|---|---|
| Inicio y páginas públicas | Conocer servicios, historia, ubicación, contacto, términos y novedades; acceder al inicio de sesión. |
| Resumen | Ver cantidades del catálogo y acceder directamente a catálogo, relaciones, impacto, producción y grafo. |
| Catálogo | Buscar, consultar detalles y gestionar proveedores, insumos y productos. Los productos incluyen precio de venta en COP y estimación de materiales. |
| Relaciones | Gestionar precios de proveedores, cantidades de insumos por producto y dependencias generales. |
| Grafo de abastecimiento | Explorar elementos y conexiones; seleccionar, enfocar, mostrar pesos, acercar, alejar, desplazar y ajustar la vista. |
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

### Precio de venta y estimación de materiales

**Catálogo → Productos → Precio (COP)** muestra el precio de venta por unidad. Un importe vacío se presenta como **Por definir**; cero es válido. El precio de venta es independiente del costo estimado de materiales.

Al abrir los detalles de un producto, la estimación toma el menor precio registrado de proveedor para cada insumo directo y lo multiplica por la cantidad requerida. No incluye mano de obra, impuestos, transporte ni el costo recursivo de dependencias generales. Si faltan ofertas o hay requisitos que no cubre ese cálculo, la estimación se indica como incompleta.

Las etiquetas **Precio de referencia** y **Cantidad de referencia** identifican valores de referencia; **Precio registrado** y **Cantidad registrada** indican que se desmarcó esa condición al guardar. La aplicación no verifica cotizaciones externas ni disponibilidad. Revisa la unidad del insumo y sus pesos asociados antes de interpretar un costo.

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

## API

La interfaz y la API se sirven desde el mismo origen. El **Explorador de API** ofrece catálogo, grafo, impacto, estimación, producción y creación/edición/eliminación de elementos y relaciones. Para escribir, muestra **Revisar cambio → Confirmar y ejecutar**. Las rutas de cuentas y contacto también existen, pero se utilizan desde sus pantallas específicas, no desde ese selector.

| Método y ruta | Acceso / función |
|---|---|
| `POST /api/auth/login` | Público; iniciar sesión. |
| `GET /api/auth/session` | Usuario autenticado; leer la sesión propia. |
| `POST /api/auth/logout` | Usuario autenticado; cerrar sesión. |
| `GET /api/catalog`, `GET /api/graph` | Todos los perfiles; catálogo y red. |
| `GET /api/products/{ProductId}/estimate` | Todos los perfiles; estimación de materiales. |
| `GET /api/impact/{ElementId}?Direction=Consequences` | Todos los perfiles; impacto. Acepta también `Requirements`. |
| `GET /api/production` | Todos los perfiles; orden/ciclos de toda la red. |
| `GET /api/production?ProductId=PROD-ESTANTE` | Todos los perfiles; producto y requisitos. Sustituye el ID si no existe en tu catálogo. |
| `POST /api/elements` | Editor/Administrador; crear elemento. |
| `PATCH /api/elements/{ElementId}` | Editor/Administrador; editar con `Version`. |
| `GET /api/elements/{ElementId}/deletion-preview` | Editor/Administrador; revisar el efecto de eliminar. |
| `DELETE /api/elements/{ElementId}?Version=N` | Editor/Administrador; eliminar elemento y sus relaciones. |
| `POST`, `PATCH`, `DELETE /api/relations` | Editor/Administrador; crear, editar o eliminar una relación. Edición y eliminación requieren `Version` en el cuerpo. |
| `GET`, `POST /api/users` | Administrador; listar y crear cuentas. |
| `PATCH /api/users/{UserId}` | Administrador; cambiar perfil/estado con `Version`. |
| `PATCH /api/account` | Usuario autenticado; modificar sus datos básicos. |
| `POST /api/account/password` | Usuario autenticado; cambiar su contraseña. |
| `POST /api/contact-requests` | Público; guardar solicitud con consentimiento. |
| `GET /api/contact-requests?Limit=20&Offset=0` | Administrador; consultar la bandeja paginada. |
| `PATCH /api/contact-requests/{RequestId}/read` | Administrador; marcar como leída. |
| `POST /api/subscriptions` | Público; registrar correo con consentimiento. |

Las escrituras autenticadas exigen sesión, `X-CSRF-Token` y `Origin` válido; los formularios y el explorador los incorporan. Los formularios públicos también validan el origen y sus datos. No existe `POST /api/auth/register`.

Los importes viajan como texto decimal, por ejemplo `"35000.00"` o `"2.5000"`. Se usan `Decimal` en Python y columnas decimales en MariaDB. `SalePrice: null` elimina el precio de venta; omitirlo en una edición conserva el valor. `IsPriceReference` corresponde al precio de venta; `IsDemo` se conserva como nombre interno para los pesos de referencia de relaciones.

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
.\Iniciar.cmd --port 3001
```

Si el servidor ya está activo y cambiaste Python, detenlo con **Ctrl+C** en su consola y vuelve a iniciarlo. No hace falta reiniciar MariaDB. Recarga el navegador después de compilar. `npm run dev --prefix frontend` sirve Vite de forma aislada; no conecta la sesión ni la API. Usa el flujo anterior para probar la aplicación completa.

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

`tests/RunChecks.py` es una integración separada que crea y retira datos de prueba en MariaDB. No se ejecuta con el verificador general; úsala únicamente en una base destinada a pruebas. Los detalles están en [SETUP](docs/SETUP.md).

## Problemas frecuentes

| Situación | Qué revisar |
|---|---|
| La página no abre | MySQL debe estar iniciado y la consola de AbastecePyme abierta. Usa la URL que indica esa consola. |
| Impacto o producción responde `404` para todo | Comprueba que no estés usando el servidor antiguo del puerto 3000. Reinicia el proceso de AbastecePyme actualizado en 3001 y recarga. |
| Dice «Elemento/Producto no encontrado» | Recarga el catálogo y selecciona un registro que todavía exista. |
| No aparece Crear usuario | Solo se muestra con una sesión de Administrador. |
| No aparecen Editar o Eliminar | El perfil Consulta es de lectura. Un administrador puede revisar el acceso. |
| Error `409` al guardar | Otra edición cambió la versión o hay un duplicado. Recarga y revisa los datos antes de repetir. |
| MariaDB informa permiso denegado | Revisa con el administrador los privilegios de `usuario_python3` sobre `AbastecePyme.*`; el rol de la página no concede permisos SQL. |
| No llegan correos | El envío está aplazado. Los mensajes de Contacto se leen dentro de la bandeja del administrador. |

## Compartir en GitHub y conservar datos

Sube el contenido del proyecto, manteniendo `backend`, `frontend/src`, `frontend/public`, el lockfile de la interfaz, `tests`, `docs`, los programas de instalación/arranque/verificación y la semilla original. No sustituyas los archivos fuente por el ZIP dentro del repositorio.

No subas contraseñas, archivos de configuración secretos, bases exportadas, cuentas o sesiones reales, respaldos, `work`, `.venv`, `node_modules`, `runtime` ni `frontend/dist`. El `.gitignore` excluye estos recursos locales; conserva también `.gitattributes` para no alterar los bytes de la semilla. Cada instalación configura su propia cuenta del sitio y su conexión MariaDB.

Antes de futuras migraciones, conserva un respaldo consistente de estructura y datos. La aplicación escucha únicamente en `127.0.0.1`: compartir el código en GitHub **no publica un sitio accesible por Internet**. El despliegue externo requiere una preparación específica.

### Versión anterior

`src/` y `public/` conservan la implementación educativa anterior como referencia. `node src/server.js` la inicia explícitamente y guarda sus ediciones en `data/catalogo-legacy.json`, una copia independiente. No sincroniza con MariaDB y rechaza usar la semilla protegida como archivo operativo. La aplicación actual está en `backend/` y `frontend/`.

### Skills y apoyos utilizados

En la revisión reciente se instaló **web-design-guidelines** dentro del proyecto para comprobar criterios de interfaz y accesibilidad. Se utilizaron **frontend-design** para composición visual, **vercel-react-best-practices** para React, **systematic-debugging** para diagnóstico y **verification-before-completion** para verificación. Las decisiones y los límites de la revisión se documentan en el [informe técnico](docs/REVIEW_2026-11-06.md).

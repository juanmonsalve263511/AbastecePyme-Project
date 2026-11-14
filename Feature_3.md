# Feature 3 — Orden y ciclos de producción

Actualización documental: **7 de noviembre de 2026**. Se contrastaron los cuatro criterios de Feature 3 del brief `05-brief-abastecepyme.md` con el algoritmo, la API, la pantalla y las pruebas del proyecto. En esta revisión se ejecutaron únicamente las **9 pruebas puras de producción: todas aprobadas**. No se consultó ni modificó MariaDB ni se repitió la integración SQL.

## Correspondencia con los cuatro criterios de la guía

El coordinador de producción necesita saber qué preparar primero y reconocer cuándo las dependencias hacen imposible una secuencia válida.

| Criterio de Feature 3 | Cómo se cumple | Evidencia comprobable |
|---|---|---|
| Generar un orden de trabajo válido cuando las dependencias no formen ciclos | `BuildProductionPlan` aplica Kahn: libera primero los elementos sin requisitos pendientes y después sus dependientes. Devuelve `Status="Ready"`, un `Order` completo y `Stages` por etapas. | [Implementación propia](../backend/Production.py), prueba `test_acyclic_diamond_respects_every_prerequisite` y sección «Orden válido de preparación» de la [pantalla](../frontend/src/components/Production.jsx). |
| Detectar y reportar un ciclo con información útil | Si Kahn deja elementos pendientes, `FindCycle` usa DFS iterativo para obtener un camino dirigido cerrado. `CycleRelations` identifica las relaciones que lo forman; la pantalla muestra nombres, códigos, frases de requisito y el ciclo resaltado. | `FindCycle`, prueba `test_cycle_has_a_closed_real_path_and_no_partial_order` y sección «Ciclo detectado» de la pantalla. |
| No presentar un orden falso si hay contradicción | Con `Status="Cycle"`, la respuesta devuelve siempre `Order=[]` y `Stages=[]`, aunque exista una rama independiente que pudiera prepararse. La pantalla muestra una alerta y no presenta etapas como solución. | Las dos listas vacías se comprueban en la aceptación cíclica; la interfaz solo dibuja etapas cuando `Status` es `Ready`. |
| Incluir pruebas de aceptación de un caso acíclico y uno cíclico | Las dos primeras pruebas descritas abajo usan grafos sintéticos con resultados esperados explícitos. Se acompañan de siete pruebas de alcance, límites y consistencia. | [tests/python/test_production.py](../tests/python/test_production.py): **9 pruebas aprobadas el 7 de noviembre de 2026**. |

La tabla distingue implementación, presentación y evidencia ejecutada. La inspección del código de la interfaz en esta actualización no constituye una nueva sesión de aceptación visual.

## Qué significa el orden para AbastecePyme

La dirección elegida es **A → B: A es un requisito que habilita B**. La frase equivalente es «Para preparar B, necesito A». Por tanto, A debe aparecer antes que B en cualquier orden válido de las relaciones registradas.

Ejemplo: Proveedor → Tablero de madera → Mesa de trabajo. Las etapas agrupan elementos cuyos requisitos quedaron en etapas anteriores. Dentro de una misma etapa no hay dependencias entre sus integrantes y puede elegirse su orden.

Se obtiene un **orden estructural de preparación**. No se calculan inventario, disponibilidad real, tiempos, capacidad, fechas ni recursos compartidos. Los precios y cantidades no alteran la precedencia. Las ofertas representan alternativas de compra: incluir los proveedores de la red no obliga a comprarles a todos ni selecciona automáticamente un proveedor.

Feature 2 usa BFS para responder qué elementos se alcanzan y cuál es el impacto potencial. Feature 3 usa Kahn para responder qué requisitos deben preceder a cada preparación; son preguntas distintas.

## Algoritmos propios y representación

El código de [backend/Production.py](../backend/Production.py) usa diccionarios, conjuntos, listas e iteradores de Python. No importa NetworkX, `graphlib` ni una biblioteca que resuelva los algoritmos de grafos.

1. `BuildProductionPlan` construye un mapa por identificador, las listas de sucesores y los requisitos entrantes. Los conjuntos deduplican las parejas dirigidas para no contar dos veces un mismo requisito.
2. Para toda la red incluye todos los elementos, incluso los aislados. Si se selecciona un producto, recorre sus requisitos hacia atrás e incluye únicamente ese producto y sus requisitos transitivos.
3. Kahn cuenta los requisitos pendientes dentro del alcance. Los elementos con contador cero forman la primera etapa.
4. Al procesar una etapa, reduce el contador de cada dependiente. Los que llegan a cero forman la etapa siguiente. Repite hasta agotar los elementos que puede liberar.
5. Si quedan elementos pendientes, `FindCycle` recorre esa parte mediante DFS iterativo. Mantiene el camino activo y una pila de iteradores; una arista que regresa a un integrante del camino revela un ciclo. Devuelve el segmento implicado y repite su primer identificador para cerrarlo.
6. Si hay ciclo, descarta el orden y las etapas parciales. Devuelve por separado el ciclo demostrado y todos los elementos pendientes.

Los recorridos son iterativos: la prueba con una cadena y un ciclo de **1.500 elementos** verifica que no dependan del límite de recursión. El núcleo Kahn/DFS cuesta O(V+E); ordenar identificadores, sucesores y relaciones para dar resultados reproducibles añade trabajo de ordenación. Una cota general es O(V log V + E log E), con almacenamiento O(V+E), incluida la respuesta. Puede haber varios órdenes válidos; esta implementación devuelve uno determinista.

## Dos casos de aceptación explicados

### Caso acíclico: dos requisitos antes de DD

Las relaciones son `AA → BB`, `AA → CC`, `BB → DD` y `CC → DD`.

| Etapa | Elementos | Justificación |
|---|---|---|
| 1 | AA | No tiene requisitos previos. |
| 2 | BB, CC | Ambos requieren AA, ya preparado. |
| 3 | DD | Requiere tanto BB como CC, preparados en la etapa anterior. |

Resultado esperado y comprobado:

```json
{
  "Status": "Ready",
  "Order": ["AA", "BB", "CC", "DD"],
  "Stages": [["AA"], ["BB", "CC"], ["DD"]],
  "Cycle": []
}
```

La prueba `test_acyclic_diamond_respects_every_prerequisite` comprueba esos valores y que el análisis no modifique los datos de entrada. La alternativa AA, CC, BB, DD también sería válida en términos de negocio, pero el orden por identificador hace reproducible la salida entregada.

### Caso cíclico: un dependiente bloqueado y una isla independiente

Las relaciones son `AA → BB → CC → AA`, más `CC → DD`. `FREE` es un elemento aislado.

AA termina requiriéndose a sí mismo a través de BB y CC. DD queda bloqueado porque depende de CC, pero **DD no forma parte del ciclo**. FREE puede liberarse durante Kahn, pero eso no resuelve la contradicción del alcance completo.

```json
{
  "Status": "Cycle",
  "Order": [],
  "Stages": [],
  "Cycle": ["AA", "BB", "CC", "AA"],
  "Unresolved": ["AA", "BB", "CC", "DD"]
}
```

`CycleRelations` contiene las tres relaciones del camino cerrado. La prueba `test_cycle_has_a_closed_real_path_and_no_partial_order` verifica el ciclo exacto, las listas de orden vacías, los cuatro pendientes y que DD no aparezca en `Cycle`. Así demuestra tanto la detección útil como la ausencia de un orden falso.

Estos escenarios existen en memoria durante las pruebas. No es necesario crear un ciclo en el catálogo operativo para demostrar el criterio.

## Alcance y estados de la respuesta

| Situación | Estado y comportamiento |
|---|---|
| Red o alcance no vacío sin ciclos | `Ready`: devuelve orden y etapas completos. |
| Un ciclo en cualquier parte del alcance | `Cycle`: `Order=[]`, `Stages=[]` y cero etapas en `Summary`; devuelve un ciclo real y los elementos pendientes. |
| Catálogo vacío, sin producto seleccionado | `Empty`: no hay orden, etapas ni ciclo. No es un error. |
| Producto con un ciclo ajeno a sus requisitos | Puede ser `Ready`; la contradicción ajena queda fuera de su alcance. |
| Producto cuyos requisitos alcanzan un ciclo | `Cycle`; no se presenta una solución parcial. |

`Unresolved` incluye integrantes de ciclos y elementos que dependen de ellos. No debe interpretarse como la lista de integrantes del ciclo mostrado. `Cycle` identifica un testigo: puede haber otros ciclos, por lo que después de corregir una relación debe repetirse el análisis. La aplicación no elimina ni corrige relaciones automáticamente.

## API y permisos

La ruta está definida en [backend/Main.py](../backend/Main.py):

```http
GET /api/production
GET /api/production?ProductId=PROD-ESTANTE
```

La primera consulta analiza toda la red. La segunda analiza Estante modular y sus requisitos transitivos. Requieren una sesión activa de **Consulta, Editor o Administrador**; los identificadores internos de esos perfiles son `Viewer`, `Editor` y `Administrator`. El análisis lee el catálogo vigente y no escribe datos de negocio. Consulta puede analizar; solo Editor y Administrador pueden corregir relaciones mediante los flujos de edición autorizados.

| Campo | Significado |
|---|---|
| `Status` | `Ready`, `Cycle` o `Empty`. |
| `Scope`, `ProductId` | Alcance `Network` o `Product`, y producto seleccionado si aplica. |
| `Algorithm` | `KahnWithIterativeCycleSearch`. |
| `Order`, `Stages` | Identificadores en secuencia y por etapas. Vacíos si hay ciclo. |
| `Cycle`, `CycleRelations` | Camino cerrado y relaciones que lo justifican. |
| `Unresolved` | Elementos que no pudieron liberarse. |
| `Summary` | Cantidades de elementos, relaciones y etapas del alcance. |
| `Graph` | Elementos, nombres y relaciones incluidos; permite presentar el resultado de forma comprensible. |

HTTP **200** también puede contener `Cycle`: la consulta funcionó y detectó una contradicción. HTTP **401** indica sesión ausente o vencida; **404**, producto inexistente; **422**, identificador inválido o selección de un elemento que no es producto.

## Cómo usar la pantalla

Los textos siguientes corresponden a [frontend/src/components/Production.jsx](../frontend/src/components/Production.jsx):

1. Inicia sesión y abre **Orden de producción**.
2. En **Qué quieres organizar**, elige **Toda la red de abastecimiento** o un producto.
3. Pulsa **Generar orden**. Mientras responde, la pantalla indica que está revisando dependencias.
4. Si aparece **Dependencias compatibles**, revisa las etapas. Cada elemento muestra nombre, tipo e identificador; expande sus requisitos previos cuando los tenga.
5. Abre **Ver el orden completo y la red analizada** para consultar la secuencia y el grafo.
6. Si aparece **Ciclo detectado**, sigue **El camino que cierra el ciclo**, las frases «Para preparar X, necesito Y» y las relaciones resaltadas. El apartado de elementos pendientes explica por qué un dependiente bloqueado no necesariamente pertenece al ciclo.
7. Un Editor o Administrador dispone de **Revisar relaciones**. Debe corregir la relación que no represente el proceso y generar de nuevo el análisis. No conviene eliminar una dependencia válida solo para ocultar una contradicción.

Al cambiar el alcance se limpia el resultado anterior. También se descarta un resultado cuando cambia el catálogo cargado en la interfaz; las solicitudes sustituidas no deben sobrescribir la consulta vigente. Si otra persona cambia datos que aún no se han actualizado en tu pantalla, vuelve a consultar el catálogo y genera el orden.

## Pruebas ejecutadas el 7 de noviembre de 2026

Desde la raíz del proyecto, con el entorno Python instalado:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m unittest discover -s tests/python -p test_production.py -v
```

Resultado de esta revisión: **Ran 9 tests in 0.011s — OK**, código de salida 0. El tiempo corresponde a esa ejecución y puede variar entre equipos.

| Prueba | Comportamiento cubierto |
|---|---|
| `test_acyclic_diamond_respects_every_prerequisite` | Orden y etapas exactos del diamante acíclico; conserva la entrada. |
| `test_cycle_has_a_closed_real_path_and_no_partial_order` | Ciclo cerrado real; distingue DD bloqueado; descarta orden y etapas parciales. |
| `test_target_scope_ignores_unrelated_cycle_but_not_its_requirements` | Un ciclo ajeno no invalida el producto, pero uno conectado a sus requisitos sí. |
| `test_empty_and_isolated_elements_are_valid` | Catálogo vacío y elementos aislados. |
| `test_self_cycle_is_reported` | Autorrelación como ciclo cerrado AA → AA. |
| `test_long_graph_and_cycle_do_not_depend_on_recursion_limit` | Cadena y ciclo de 1.500 nodos con recorridos iterativos. |
| `test_invalid_graph_and_target_fail_explicitly` | Referencias inexistentes, identificadores duplicados, producto ausente y tipo incorrecto. |
| `test_duplicate_edges_do_not_create_false_cycles` | Relaciones duplicadas en la entrada no inflan los grados. |
| `test_result_is_deterministic_when_input_order_changes` | El mismo grafo produce la misma respuesta aunque se invierta el orden de entrada. |

Estas pruebas importan el módulo de producción y construyen listas en memoria. No requieren servidor, red, credenciales ni base de datos.

## Evidencia histórica y límites de las cifras

| Fecha y alcance | Evidencia registrada | Cómo debe interpretarse |
|---|---|---|
| 7 de noviembre de 2026, esta actualización | 9 pruebas puras de producción aprobadas. | Es la ejecución nueva descrita en este documento. |
| 6 de noviembre de 2026, revisión general | **68 pruebas Python y 48 JavaScript aprobadas**, además de compilación y comprobaciones de interfaz. | Resultado histórico del [informe de revisión](REVIEW_2026-11-06.md); no se volvió a ejecutar la suite completa para esta actualización. |
| 6 de noviembre de 2026, lectura del catálogo real | **48 elementos y 128 relaciones**, sin modificar datos de negocio. | Fotografía fechada del catálogo; no es una constante de la aplicación ni una nueva prueba de integración SQL. |
| 6 de noviembre de 2026, lectura real y análisis de Estante modular | `PROD-ESTANTE`: **Ready, 12 elementos, 14 relaciones y 4 etapas**. | Resultado registrado al leer el catálogo real y ejecutar `BuildProductionPlan` en aquella revisión. No se volvió a consultar el 7 de noviembre ni es una cantidad garantizada para Estante. |
| 2 de noviembre de 2026, integración MariaDB/FastAPI | **15 pruebas aprobadas y 1 omitida**, para no intervenir el administrador permanente. | Evidencia histórica de integración; no se repitieron esas operaciones el 7 de noviembre. |
| 2 de noviembre de 2026, catálogo previo a la ampliación | **40 elementos, 92 relaciones, Ready y 3 etapas**. | Dato histórico sustituido por la ampliación posterior; no debe presentarse como estado actual. |

La aceptación histórica de [tests/integration/test_api.py](../tests/integration/test_api.py), `test_production_order_and_cycle_acceptance`, prepara productos temporales, comprueba una cadena válida, añade el cierre del ciclo y verifica `Cycle`, `Order=[]` y `Stages=[]`. Es una prueba que escribe datos de prueba y hace limpieza; no forma parte del comando puro anterior ni fue ejecutada en esta revisión documental.

El informe del 6 de noviembre también registra un escenario de interfaz con datos preparados que produjo 12 elementos y 4 etapas para Estante. Esa comprobación visual y el análisis de la lectura real son evidencias distintas, aunque sus cifras coincidan; ninguna equivale a una nueva ejecución de toda la integración SQL.

La [ampliación del catálogo](CATALOG_UPDATE_2026.md) y el [informe del 6 de noviembre](REVIEW_2026-11-06.md) explican la evolución de los datos y la validación general. Los elementos, relaciones y etapas pueden variar legítimamente al editar el catálogo.

## Guion breve de demostración

1. Explica una flecha con la frase «Para preparar B, necesito A». Presenta el orden como precedencia, sin atribuirle cálculos de inventario o duración.
2. Con una sesión de Consulta, Editor o Administrador, abre **Orden de producción**, selecciona un producto del catálogo y pulsa **Generar orden**. Muestra las etapas y los requisitos de un elemento. Usa las cantidades que devuelva esa consulta, sin fijar cifras históricas como resultado obligatorio.
3. Ejecuta el comando de las nueve pruebas puras y muestra las dos aceptaciones centrales. Explica el diamante AA → BB/CC → DD y el ciclo AA → BB → CC → AA con DD bloqueado y FREE independiente.
4. Señala que, en el caso cíclico aislado, `Order` y `Stages` quedan vacíos. Relaciona esa salida con la alerta y el camino que implementa la pantalla, sin afirmar que se provocó un ciclo en el catálogo operativo.
5. Cierra mostrando la tabla de los cuatro criterios y distingue las nueve pruebas recién ejecutadas de los resultados históricos de suites generales e integración.

Este guion demuestra la aceptación cíclica sin introducir contradicciones de prueba en los datos de negocio.

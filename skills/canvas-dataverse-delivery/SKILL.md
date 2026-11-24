---
name: canvas-dataverse-delivery
description: Use when an agent builds or changes a Power Apps Canvas app backed by Dataverse (via Canvas Authoring MCP, PAC CLI or coauthoring). Enforces schema discovery before design, a minimal vertical slice, and separate evidence for compile, save, publish, runtime and read-back.
version: 0.3.0
author: Bruno de los Ríos (Under the Platform)
license: MIT
platforms: [linux, macos, windows]
metadata:
  status: experimental
  hermes:
    tags: [Power Apps, Canvas, Dataverse, ALM, delivery]
    related_skills: [canvas-app, configure-canvas-mcp, dv-metadata]
---

# Entrega de apps Canvas + Dataverse

Procedimiento para que un agente construya o modifique una app Canvas sobre Dataverse **con oficio**: sin inventar el esquema, con alcance acotado y con una prueba propia para cada estado. Complementa las skills oficiales de Microsoft (`canvas-app`, `configure-canvas-mcp`) en lo que no cubren: gates, evidencia y documentación.

> Estado: **experimental**. Nace de laboratorios reales, pero todavía no se ha repetido en suficientes apps distintas. Ver `README.md` del repositorio.

## Cuándo usarla

- Crear o cambiar una app Canvas respaldada por Dataverse.
- Preparar un vertical slice, una alpha o su documentación y evidencias.
- **No** es permiso para tocar producción, licencias, seguridad o datos reales. Esas decisiones siempre las toma una persona.

## Principios

1. **El contrato vale más que el prompt.** Esquema real, convenciones y decisiones de negocio por escrito antes de generar nada.
2. **Preguntar antes de suponer.** Ante una ambigüedad de negocio, para y ofrece opciones. Resuelve tú lo técnico; devuelve solo las decisiones de negocio.
3. **Cada estado tiene su prueba.** Compilado, guardado, publicado, funcionando en Player y leído de vuelta en Dataverse son hitos distintos.
4. **El agente trabaja con los permisos de quien lo usa.** Entorno de desarrollo, rol mínimo y aprobación explícita antes de escribir o publicar.

## Procedimiento

1. **Baseline y gates.** Inspecciona repo, app, solución y entorno existentes; reutiliza lo que haya. Define actor, datos, criterios de aceptación, límites y una semilla de datos ficticios. Pide autorización para escrituras, publicación, conectores o servicios de pago. No registres IDs de tenant o entorno, URLs privadas, credenciales ni datos reales. *Criterio: alcance y aceptación quedan por escrito.*
2. **Descubrimiento antes del diseño.** Lee el esquema real de Dataverse (MCP de Dataverse, PAC, Web API o tipos generados) y compáralo con la app existente. No inventes nombres lógicos, tipos, opciones, relaciones ni propiedad. Comprueba que las columnas Choice tienen etiquetas: un valor `100000000` sin etiqueta es un defecto de modelado, no algo que resolver adivinando. *Criterio: cada campo de pantalla se corresponde con el esquema verificado o con un cambio aprobado.*
3. **Vertical slice mínimo.** Implementa primero listado → alta → persistencia → detalle → regreso. Prueba obligatorios, Choice, fechas y formato regional, moneda, vacíos y errores. Deja fuera aprobaciones, edición o procesos posteriores hasta que se autoricen. *Criterio: el registro ficticio se ve tras guardar; si no hay lectura directa de Dataverse, se anota el límite.*
4. **Fuente Canvas controlada.** Con Canvas Authoring MCP, sincroniza a un directorio dedicado que contenga solo los `.pa.yaml` de esa app. Edita de forma dirigida y conserva propiedades y bindings que nadie pidió cambiar. Compila tras cada ola coherente de cambios. Al terminar, vuelve a sincronizar, compara con la fuente canónica y compila otra vez. El servicio puede omitir valores por defecto o normalizar el orden: no sobrescribas la fuente local sin revisar el diff. *Criterio: YAML parseable y compilación final sin bloqueos.*
5. **Diagnósticos y runtime.** Ejecuta App Checker y accesibilidad. Trata los **avisos de delegación como errores** de revisión: con pocos datos parecen inofensivos, con volumen real dejan registros fuera. Prueba en Player, no solo en Studio. Si una acción de Computer Use devuelve `unverifiable`, vuelve a capturar el estado y no afirmes éxito. *Criterio: cada resultado tiene evidencia y un estado preciso.*
6. **Compilar no es validar.** Contrasta cada regla de negocio con su comportamiento real, no solo con la compilación. Una fórmula que busca una palabra en un texto puede compilar con cero errores y hacer otra cosa. *Criterio: cada criterio de aceptación tiene una comprobación observada.*
7. **Consentimiento y fuentes.** Si Studio muestra términos de coautoría, no los aceptes en nombre de la persona: pide que los revise y acepte ella; un silencio o un timeout no es aceptación. El MCP no añade tablas por sí solo: si `list_data_sources` no devuelve la tabla, hay que añadirla en el panel Datos de Studio y verificarla con `list_data_sources` y `get_data_source_schema` antes de escribir Power Fx. Una tabla en la solución no equivale a una fuente de datos de la app.
8. **Capturas seguras.** Captura cada hito en el momento, con datos ficticios, y recorta antes de guardar. Revisa URL, IDs, cuenta, barras del navegador y datos privados. Añade identificador, pie, fecha y qué demuestra (y qué no) cada imagen. Plantilla en `references/registro-evidencia.md`.
9. **Documentación y métricas.** Actualiza README, decisiones, bitácora, matriz de pruebas, índice de evidencias y pendientes. Distingue observación, declaración de la persona, inferencia y recomendación. Ver `references/metricas.md`.
10. **Cierre.** Ejecuta tests, parseo de YAML, `git diff --check` y lint. Revisa `git status`; no hagas push ni publicación externa sin autorización. Declara qué quedó sin verificar y cuál es el siguiente gate.

## Pedir cambios de forma revisable

Formula cada cambio como una historia de usuario con **invariantes explícitas**: qué hay que cambiar y qué no se puede tocar (consultas, esquema, identificador de página, comportamiento ya validado). Un cambio con invariantes se revisa en minutos; uno sin ellas obliga a revisar toda la app.

## Diseño UX/UI en Canvas

1. Parte de la guía de diseño de Canvas vigente y de la especificación UX del proyecto; prioriza patrones Canvas/Fluent sobre guías genéricas de frontend web.
2. Analiza cada pantalla por tarea: qué busca la persona, qué decide, qué errores puede cometer, densidad, navegación, accesibilidad y tamaño de pantalla. Revisa la app real y los controles disponibles antes de elegir componentes.
3. Un único botón principal por contexto, jerarquía tipográfica y espaciado coherentes. No inventes KPI ni contadores ni imágenes decorativas.
4. Elige controles por la decisión que presentan: radio visible para pocas opciones excluyentes, desplegable para listas largas, tarjeta por fila navegable (con los huecos de imagen a `Blank()` si no hay imagen real). Dentro de una Gallery, envuelve cada fila en un contenedor AutoLayout que se estire a sus límites.
5. Conserva etiquetas, orden de foco, mensajes de error accionables, datos introducidos tras un error, estado comunicado también sin color y áreas táctiles suficientes.
6. Implementa pantalla a pantalla y compila tras cada una. Valida en Studio y en Player por separado, a 360, 768 y 1366 px, antes de llamarlo responsive.
7. Si aparece una pantalla de inicio de sesión, no toques credenciales: pausa y pide a la persona que se autentique.

## Trampas frecuentes

- La autorización para crear un registro de prueba no autoriza activar flujos ni servicios de pago.
- Una captura de un formulario vacío no prueba un envío; un aviso de «publicando» no prueba una publicación terminada.
- Una página generada que no recibe acceso a datos (por ejemplo, sin `dataApi` en sus props) es un dibujo, aunque parezca terminada.
- No supongas numeración automática ni unicidad si no las has visto en el esquema o en ejecución.
- Un `GUID` válido pero equivocado (por ejemplo, el ID de organización de Dataverse en lugar del ID de entorno) despliega en el sitio equivocado: verifica identificadores contra la fuente antes de escribir.
- Avisar de un riesgo no es evitarlo: si detectas una ambigüedad, resuélvela con la persona antes de compilar.

## Checklist

- [ ] Alcance, esquema y autorizaciones verificados.
- [ ] Recursos existentes reutilizados; ningún metadato adivinado; Choice con etiquetas.
- [ ] Vertical slice con datos ficticios comprobado en Player, o límite declarado.
- [ ] Fuente Canvas sincronizada y compilada tras el último cambio.
- [ ] Sin avisos de delegación pendientes.
- [ ] Diagnósticos y pruebas de ejecución documentados por separado.
- [ ] Capturas legibles, saneadas y enlazadas.
- [ ] Métricas con alcance, fuente y salvedades.
- [ ] Tests y diff correctos; ninguna publicación o push dado por hecho.
- [ ] Pendientes y siguiente gate explícitos.

# Métricas de trabajo con agentes

Medir sirve para comparar modelos, agentes y enfoques con algo más que impresiones.

## Qué registrar por tarea

| Campo | Notas |
|---|---|
| Agente, modelo y proveedor | Y el modo de facturación: suscripción, pago por uso, local. |
| Tiempo de pared | Del primer al último mensaje. Incluye pausas. |
| Actividad aproximada | Suma de intervalos entre mensajes por debajo de un umbral (indica cuál: 5 o 10 min). Es un indicador, no un cronómetro. |
| Tokens | Entrada, salida, lectura de caché y razonamiento, **por separado**. La caché no se suma a la entrada. |
| Coste | Real, estimado, incluido en suscripción o desconocido. «Incluido» no es «gratis». |
| Correcciones | Cuántas veces hubo que pedir un cambio para el mismo criterio. |
| Resultado | Compila, pasa tests, funciona en Player, se lee de vuelta. |
| Defectos encontrados en revisión | Y quién los encontró: el agente o una persona. |

## Comparar modelos o agentes

- Misma tarea con datos ficticios, mismo esquema, mismo prompt, mismas herramientas y mismos criterios.
- Puntúa: exactitud del esquema, compilación y tests, defectos, turnos de corrección, tiempo y coste.
- Una respuesta fluida que no compila o no se lee de vuelta vale menos que un resultado probado.
- Los modelos locales o gratuitos sirven para resúmenes, borradores y checklists si alguien los revisa; Power Fx ambiguo y decisiones de riesgo, a un modelo de mayor capacidad.
- No declares un ganador con una sola tarea: escenarios equivalentes no son idénticos.

## Lo que no es una métrica

- La capacidad del equipo (CPU, RAM) no es su uso real, salvo que haya telemetría.
- Un contador a cero porque no se midió debe aparecer como desconocido, no como cero.

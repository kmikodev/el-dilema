# Estado

El handoff entre sesiones: lo de hoy, no las reglas. Las reglas permanentes están en
`CLAUDE.md` y el concepto en `CONCEPTO.md`. Este fichero se actualiza al cerrar cada
fase, y se lee al empezar cualquier sesión.

**Actualizado:** 11 de septiembre de 2026, al cerrar la fase 02.

## Dónde estamos

Fase 02 cerrada y publicada, tag `02-decision`. La siguiente es **03, datos**. No hay
una sola línea de código todavía.

## Qué se decidió en la 02

La aplicación es **el dilema** y la charla es **en remoto por Meet**. Las cinco
candidatas, por qué ganó ésta y las decisiones cerradas están en `CONCEPTO.md`: para
continuar no hace falta releer la deliberación, basta ese documento.

## Qué bloquea

Cuatro cosas, todas con issue abierta:

- **El dilema concreto no existe** (#1). Sin él no hay nada que construir ni que
  ensayar, y es la pieza de la que depende todo lo demás.
- **El piloto con personas reales** sigue sin decidirse (#1). Los dos ataques del
  abogado del diablo coinciden en que sin él el mejor momento de la charla es una
  tirada de dados. No necesita código: el enunciado, diez personas y un minuto de
  plazo.
- **El identificador de dispositivo** (#2) hay que decidirlo *antes* de tocar datos,
  porque es esquema y no se cambia después. De él depende poder afirmar que la sala se
  movió.
- **El presupuesto de horas** (#3) no está fijado, y las dos estimaciones disponibles
  difieren en un factor de tres sobre el mismo trabajo.

## Lo siguiente

La fase 03, datos. Pero conviene cerrar antes #1 y #2: la primera decide qué se
guarda, la segunda decide cómo.

## Qué está roto ahora mismo

- **El MCP de GitHub no conecta** cuando la sesión se arranca con `claude` a secas:
  falta `GITHUB_PAT` en el entorno del proceso. Es el fallo ya documentado en
  `CLAUDE.md`, y se arregla arrancando con `./bin/dilema`. Las 29 issues de la fase 02
  hubo que crearlas con `gh`, que sí funciona y sirve de salida de emergencia.
- **La prueba del anclaje no se puede ejecutar desde dentro del repositorio.** Cada
  subagente recibe `CLAUDE.md` y el estado de git al arrancar, así que conoce el
  concepto y el nombre del remoto sin leer nada. Está en la issue #5 con el alcance
  corregido y explicado en `CONCEPTO.md`. No la repitas a ciegas.
- Nada más: como no hay código, no hay nada más que pueda estar fallando.

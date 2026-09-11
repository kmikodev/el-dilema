# el-dilema

*(nombre provisional del repositorio)*

Una aplicación construida de principio a fin con Claude Code, para una charla interna
de dos horas. Lo que se va a enseñar en la charla es **el proceso**, no el resultado,
así que la historia de este repositorio importa tanto como el código.

La aplicación es **el dilema**: la sala se posiciona ante una pregunta con dos posturas
defendibles y escribe por qué; el agente no cuenta los votos, sino que agrupa las
respuestas en posturas que nadie definió de antemano, nombra la premisa que casi todos
comparten sin darse cuenta y escribe el contraargumento que la ataca. `CONCEPTO.md`
cuenta qué hace, por qué ganó a las otras cuatro candidatas y qué sigue abierto.

## Lo que ya está decidido

- Dos horas, público mixto: gente técnica y gente que no lo es.
- **La charla es en remoto, por Google Meet.** No hay sala, ni proyector, ni wifi
  compartido: unas cuarenta personas, cada una en su casa y en su propia red, con Meet
  abierto en un portátil.
- La usan esas cuarenta a la vez durante la charla, **igual desde el móvil que desde el
  portátil**.
- **Qué aplicación es.** El dilema, descrita en `CONCEPTO.md`.
- La vista común vive en dos sitios a la vez: el ponente la comparte por Meet y además
  cada asistente puede abrirla en su navegador.
- Esa vista común es una lluvia tipo Matrix **acoplada a datos reales**, no un fondo
  decorativo.
- Tiene que haber **un agente que razone de verdad**, no un `if` disfrazado.
- Despliegue serverless, con dominio propio.
- Tiene que dar juego para enseñar algo en cada fase: datos, agente, interfaz,
  despliegue. Si es demasiado simple no hay nada que contar; si es demasiado
  ambiciosa, no llega a tiempo.

## Lo que no está decidido

Sigue siendo bastante, y se nombra como hipótesis hasta que se decida. Lo que bloquea
antes de construir: **el dilema concreto** que se lanza, si hay un **piloto con personas
reales** que lo valide antes de la charla, si se guarda un **identificador de
dispositivo** —del que depende poder afirmar que la sala se movió— y **cuántas horas**
hay para construir. Abiertos también el stack, el dominio y buena parte del guion.

La lista completa está al final de `CONCEPTO.md`, que es donde se mantiene.

## Estado

Elegida la aplicación, no hay nada construido. La deliberación que llevó hasta aquí
está resumida en `CONCEPTO.md`. Lo que salió de ella y merece seguimiento vive como
issues en GitHub.

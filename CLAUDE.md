# Cómo se trabaja en este repositorio

Este fichero describe **el método de trabajo**, no el proyecto. Qué aplicación se
construye aquí todavía no está decidido, y decidirlo es parte de lo que se enseña.

## Regla primera: no inventar el proyecto

El README separa a propósito lo que está decidido de lo que no. Esa separación es
la fuente de verdad.

- Si algo del proyecto no está decidido, **se pregunta**. No se rellena el hueco
  con una suposición razonable, ni se propone «de momento asumo que…».
- No escribas en `CLAUDE.md`, en los agentes ni en las skills nada que dé por
  supuesto qué es la aplicación, qué stack usa o qué datos maneja.
- Cuando una decisión se tome, se escribe en el README y pasa a ser un hecho.
  Hasta entonces es una hipótesis y se nombra como tal.

## Idioma

- Documentación, commits, issues, comentarios y conversación: **español**.
- Identificadores de código (variables, funciones, ficheros, ramas): **inglés**.
- No se traducen los términos técnicos establecidos: se dice *commit*, *deploy*,
  *prompt*, *serverless*, no «entrega» ni «despliegue sin servidor».

## Git

- Rama única: `main`. No se crean ramas salvo petición explícita.
- **Los marcadores del proceso son tags, nunca ramas.** Un tag es un punto fijo
  al que saltar durante la charla; una rama se mueve y deja de marcar nada.
- **Nunca una rama con el mismo nombre que un tag.** Git acepta las dos cosas y
  entonces `git checkout 01-setup` es ambiguo: resuelve a la rama y te lleva a
  un sitio distinto del que marcaste. Delante de público eso no se depura.
- **No se hace `push` nunca sin que se pida**. El remoto es `origin`
  (`kmikodev/el-dilema`).
- **No se commitea hasta cerrar una fase.** El trabajo se acumula en el árbol de
  trabajo; una fase se cierra con un único commit. Si durante una fase parece
  necesario commitear, se pregunta antes: normalmente significa que en realidad
  son dos fases.
- El historial es material de la charla. Se lee en pantalla delante de gente que
  no ha visto el código. Un commit debe entenderse sin abrir el diff.

### Formato de commit

Conventional Commits con el asunto en español:

```
tipo(ámbito): descripción en minúscula, sin punto final

Cuerpo opcional en español explicando la decisión: qué se probó, qué falló
y por qué se acabó donde se acabó. El cuerpo es lo que se cuenta en la charla.
```

Tipos: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `perf`.
El ámbito es libre y descriptivo (`agente`, `datos`, `ui`, `deploy`, `config`).

### Tags

Cada fase cerrada lleva un **tag anotado** `NN-nombre`, con el número a dos
dígitos para que ordenen solos:

```
01-setup
02-decision
03-datos
04-agente
05-interfaz
06-despliegue
```

El mensaje del tag resume qué quedó funcionando al final de esa fase. Sirve para
saltar a cualquier punto del proceso durante la charla, así que se escribe
pensando en quién hará ese salto en directo.

Nunca se crea una rama con uno de estos nombres: haría ambiguo el `checkout`.

## Cerrar una fase

No se cierra a mano. Se usa la skill `cerrar-fase`, que comprueba el árbol,
enseña el diff, hace el commit, crea el tag anotado y recuerda actualizar el
README. Si algo no cuadra, la skill se para y pregunta.

## Secretos

- `.env` está excluido en `.git/info/exclude`, que es local y no se versiona,
  para que el nombre del fichero de credenciales no viaje en el repositorio.
  **No se lee, ni se imprime, ni se cita.** Si hace
  falta saber qué variables contiene, se miran las claves con
  `sed -E 's/=.*/=<oculto>/' .env`, nunca los valores.
- Ninguna credencial entra en un fichero versionado. `.mcp.json` y
  `.claude/settings.json` se commitean, así que sólo contienen referencias a
  variables de entorno; los valores viven en `.env`, que está ignorado.
- El hook `pre-commit` de `.githooks/` es la red: aborta el commit si detecta un
  `.env` en el índice o algo con forma de token de GitHub en los cambios. Opera
  sobre git, así que sigue protegiendo aunque la sesión corra sin permisos.
- Si un secreto llega a tocar el índice de git, se para todo y se avisa antes de
  seguir.

## Puesta en marcha desde cero

Tras clonar el repositorio hacen falta tres cosas. Ninguna la versiona git, y si
falta cualquiera de ellas el MCP de GitHub no conecta:

1. **El token.** `cp .env.example .env` y rellenar `GITHUB_PAT`.
2. **Los hooks.** `git config core.hooksPath .githooks`.
3. **La aprobación del servidor MCP.** Un `.mcp.json` de proyecto arranca en
   `Pending approval` y hay que aprobarlo una vez, la primera vez que Claude
   Code lo pregunta.

Se comprueban las tres de golpe con `claude mcp list`, ejecutado desde la raíz y
con el entorno cargado. Debe salir `github ... ✔ Connected`.

## Arrancar la sesión

Se arranca con `./bin/dilema`, no con `claude` a secas.

El motivo no es cosmético. `.mcp.json` lleva `${GITHUB_PAT}` en vez del token, y
esa expansión lee **el entorno del proceso que lanza `claude`**: no lee el
`.env`, ni el bloque `env` de `.claude/settings.local.json`. Si arrancas con
`claude` a pelo, el header sale como `Bearer ` vacío, el servidor responde 400 y
`/mcp` dice `Failed to reconnect to github`. `bin/dilema` carga el `.env` y
arranca Claude Code por ti, y falla con un mensaje claro si falta el token.

Al clonar de nuevo hay que reactivar los hooks, que git no versiona:

```bash
git config core.hooksPath .githooks
```

## El servidor MCP de GitHub

Configurado a **nivel de proyecto** en `.mcp.json`, que sí se commitea porque no
contiene ningún secreto: sólo la referencia `${GITHUB_PAT}`. El token vive
únicamente en `.env`.

Cuidado con los scopes, que se confunden. `claude mcp add-json ... --scope local`
guarda en `~/.claude.json`; `--scope project` **sobrescribe `.mcp.json` con el
token en línea**, que es justo lo que aquí no queremos. Si existen los dos, el de
`local` tapa al de `project`. Para ver cuál está activo: `claude mcp list` y
`claude mcp get github`.

Por eso hay un hook `pre-commit` en `.githooks/` que aborta el commit si detecta
un `.env` en el índice o algo con forma de token de GitHub en los cambios. Es la
red por si alguien regenera `.mcp.json` con el comando equivocado.

El servidor debe llamarse `github`: los permisos de `.claude/settings.json`
allowan las lecturas por el prefijo `mcp__github__`, y con otro nombre dejarían
de casar.

La URL apunta al toolset de issues (`/mcp/x/issues`), no al servidor completo:
9 herramientas en vez de 47, mucho menos ruido en el contexto. Si en alguna fase
hacen falta pull requests o workflows, se amplía la URL, pero no antes de
necesitarlo.

### Cuando no conecta

`Failed to reconnect to github` tiene dos causas distintas, y pueden darse a la
vez: arreglar una sola deja el síntoma igual. `claude mcp list` distingue cuál
es, porque las nombra explícitamente.

- `Missing environment variables: GITHUB_PAT` — la sesión se arrancó con
  `claude` en vez de con `./bin/dilema`, el header salió como `Bearer ` vacío y
  el servidor devolvió 400. Se sale y se arranca con el launcher.
- `⏸ Pending approval` — el servidor de `.mcp.json` no está aprobado todavía.
  Se aprueba cuando Claude Code lo pregunta al arrancar. Queda registrado en
  `~/.claude.json`, en `enabledMcpjsonServers` de este proyecto; si ahí aparece
  una lista vacía, es que nunca se aprobó o se rechazó.

Ninguna de las dos se arregla editando `.mcp.json`: el fichero es correcto en
ambos casos.

## Decidir antes de construir

Las decisiones de producto se toman con los subagentes de `.claude/agents/`:
`producto`, `viabilidad`, `charla` y `abogado-del-diablo`. Son de sólo lectura:
deliberan, no construyen. Todo lo que salga de esa deliberación y merezca
seguimiento se convierte en una issue de GitHub mediante el servidor MCP
configurado en `.mcp.json`.

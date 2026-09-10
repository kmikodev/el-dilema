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

Cada fase cerrada lleva un **tag anotado** con el nombre de la fase:

```
fase-0-decision
fase-1-datos
fase-2-agente
fase-3-interfaz
fase-4-despliegue
```

El mensaje del tag resume qué quedó funcionando al final de esa fase. Sirve para
saltar a cualquier punto del proceso durante la charla.

## Cerrar una fase

No se cierra a mano. Se usa la skill `cerrar-fase`, que comprueba el árbol,
enseña el diff, hace el commit, crea el tag anotado y recuerda actualizar el
README. Si algo no cuadra, la skill se para y pregunta.

## Secretos

- `.env` está en `.gitignore` y **no se lee, ni se imprime, ni se cita**. Si hace
  falta saber qué variables contiene, se miran las claves con
  `sed -E 's/=.*/=<oculto>/' .env`, nunca los valores.
- Ninguna credencial entra en un fichero versionado. `.mcp.json` y
  `.claude/settings.json` se commitean, así que sólo contienen referencias a
  variables de entorno; los valores viven en `.claude/settings.local.json`,
  que está ignorado.
- Si un secreto llega a tocar el índice de git, se para todo y se avisa antes de
  seguir.

## El servidor MCP de GitHub

La configuración del MCP **no vive en el repositorio**: vive en `~/.claude.json`,
porque lleva el token en línea. Es la vía que documenta GitHub para Claude Code.
`.mcp.json` está en `.gitignore` para que un fichero local con credenciales no
pueda commitearse por accidente.

A cambio, la configuración no viaja con el repo. Para no perderla, el comando que
la crea se documenta aquí. Se ejecuta **en la terminal, no dentro de Claude
Code**, desde la raíz del proyecto, y después se reinicia Claude Code:

```bash
export GITHUB_PAT="$(grep '^GITHUB_PAT=' .env | cut -d= -f2-)"
claude mcp add-json github "{\"type\":\"http\",\"url\":\"https://api.githubcopilot.com/mcp/x/issues\",\"headers\":{\"Authorization\":\"Bearer $GITHUB_PAT\"}}"
```

Se comprueba con `claude mcp list` y `claude mcp get github`.

La URL apunta al toolset de issues (`/mcp/x/issues`), no al servidor completo:
9 herramientas en vez de 47, que es mucho menos ruido en el contexto. Si en
alguna fase hacen falta pull requests o workflows, se rehace el servidor con la
URL ampliada, pero no antes de necesitarlo.

Si `/mcp` dice `Failed to reconnect to github`, casi siempre es el token: un
`Authorization: Bearer` vacío devuelve 400.

## Decidir antes de construir

Las decisiones de producto se toman con los subagentes de `.claude/agents/`:
`producto`, `viabilidad`, `charla` y `abogado-del-diablo`. Son de sólo lectura:
deliberan, no construyen. Todo lo que salga de esa deliberación y merezca
seguimiento se convierte en una issue de GitHub mediante el servidor MCP
configurado en `~/.claude.json`.

# El dilema

Qué aplicación se construye, por qué es ésta y no otra, y qué queda sin decidir.
El README resume; aquí está el razonamiento, que es lo que se cuenta en la charla.

## Qué hace

Se lanza un dilema: una pregunta con dos posturas defendibles, de esas sobre las que
todo el mundo tiene opinión y nadie tiene razón del todo. Cada asistente elige lado y
escribe una justificación breve. Eso es toda la interacción que se le pide.

Lo que pasa después es la aplicación. El agente **no cuenta votos**. Lee todas las
justificaciones y hace cuatro cosas seguidas:

1. Extrae el argumento que hay detrás de cada frase.
2. Las agrupa en posturas **que nadie definió de antemano**. Las categorías salen de lo
   que la gente escribió, no de una lista preparada antes.
3. Localiza la premisa que la mayoría comparte sin darse cuenta de que la comparte.
4. Escribe el contraargumento que ataca esa premisa.

Se muestra el resultado, se vota otra vez, y se ve si la sala se ha movido.

La tesis que esto demuestra, y que alguien tiene que poder repetir al salir: *cuarenta
personas escribieron una línea cada una, y la máquina no contó los votos: nombró la
creencia que compartían sin haberla dicho, la atacó, y unos cuantos cambiaron de lado.*

## Por qué ésta

Compitió contra cuatro. **El oráculo** (cuarenta conversaciones privadas en paralelo)
desperdicia la simultaneidad y es la única que premia abandonar la charla durante diez
minutos. **El tribunal** (cuatro agentes deliberando sobre un caso real de un asistente)
convierte a treinta y nueve personas en espectadores e incumple la restricción de que
cuarenta la usen a la vez. **El Turing invertido** (la sala vota qué texto es de la
máquina) no tiene agente, tiene una generación, y se rompe justo con cuarenta personas
porque votar exige leer y no se pueden mostrar cuarenta textos. **La sala contra el
agente** (el público intenta romperlo) es la más divertida y la más robusta, y por eso
mismo se lleva la atención y no tiene ningún mecanismo para devolverla.

Los tres evaluadores eligieron el dilema por separado y, lo que importa, **por tres
razones distintas**:

- En remoto, cuando alguien pincha el enlace la aplicación tapa el Meet y del ponente
  sólo queda la voz. La pregunta correcta no es cuánto tiempo consigues que te miren,
  sino **cuánto tardas en devolverlos**. El dilema es la única cuyo bucle termina solo.
- El patrón es **fan-in**: cuarenta líneas entran, una llamada al modelo sale. El pico
  de cuarenta simultáneos cae sobre el camino de escritura, que es trivial, y no sobre
  el del modelo, que es donde duele.
- Es la única en la que lo que aparece en la pantalla común **lo escribe la sala** y la
  máquina lo transforma. Sin caras ni sonido, el único termómetro es el que tú mismo
  has construido.

Es además la única de las cinco que cumple a la vez las cuatro restricciones que el
README ya daba por decididas.

## Cómo se decidió

Cinco candidatas, cuatro subagentes de sólo lectura y dos rondas. En la primera, los
tres evaluadores juzgaron una única propuesta ya elegida y coincidieron; el abogado del
diablo demostró que coincidir sobre un supuesto compartido no es lo mismo que
coincidir, y partió ese supuesto. En la segunda ronda se les pasaron **las cinco
candidatas en frío**, sin recomendación previa y sin ver los veredictos anteriores, y
volvieron a elegir la misma con argumentos independientes. El abogado del diablo atacó
dos veces.

El historial de esa deliberación es material de la charla: es la prueba de que la
aplicación no se eligió por intuición.

Quedaba una duda honesta: el repositorio se llama `el-dilema`, y ese nombre está en la
primera línea del README que los evaluadores leen. La prueba se intentó. Salió mal, y
cómo salió mal vale más que el resultado que buscaba.

## La prueba del anclaje, y por qué no vale

Se escribió una versión neutra del README, fuera del repositorio, sin el nombre y con
«qué aplicación es» de vuelta en lo no decidido. Se relanzó a `producto`, `viabilidad`
y `charla` con esa versión pegada en el prompt, con las cinco candidatas descritas
palabra por palabra igual que en la ronda anterior y en el mismo orden, y con
prohibición explícita de leer ningún fichero del repositorio.

**Los tres volvieron a elegir el dilema.** Y los tres avisaron, por su cuenta, de que
la prueba estaba contaminada.

`charla` lo dijo con todas las letras: el arranque de sesión le inyecta
automáticamente el contenido de `CLAUDE.md` y el estado de git, sin pedirlo. Y
`CLAUDE.md` contiene la sección «Qué es este proyecto», que describe el dilema entero.
No es que supiera el nombre del repositorio: **sabía la respuesta completa antes de
leer el prompt**, por una vía que la prohibición de leer ficheros no podía cerrar,
porque no hacía falta leer nada. `producto` avisó de lo mismo en su versión menor: el
entorno le dice el nombre del directorio y el del remoto, los dos `el-dilema`.

Así que el desenlace es que **la prueba del anclaje no se puede ejecutar desde dentro
de este repositorio**. Neutralizar el README no neutraliza nada. Haría falta una copia
del proyecto en un directorio con otro nombre, con un `CLAUDE.md` también neutralizado,
y lanzar los evaluadores desde ahí.

Hay un matiz que salva algo, y conviene no perderlo: **el contraste en frío de la fase
02 estaba más limpio que este**. Entonces `CLAUDE.md` todavía decía que la aplicación
no estaba decidida, así que lo único que se filtraba era el nombre. La ironía es exacta
y es material de charla: escribir el concepto en `CLAUDE.md` —que era lo correcto— fue
lo que destruyó la posibilidad de hacer la prueba.

Lo que sí se puede leer de esta ronda, porque no depende de la ceguera:

- **Los argumentos volvieron a ser distintos entre sí.** `producto` decidió por la
  verificabilidad personal a escala: cuarenta personas comprobando a la vez, cada una en
  silencio, si el agente entendió lo que ella escribió. `viabilidad` decidió por la
  degradación: es la única candidata que falla a algo que sigue siendo una aplicación
  completa, porque si el agente no responde quedan las frases y el recuento, que es
  justo la comparación que la charla quiere hacer. `charla` decidió por qué mitad del
  público produce el material del que depende la charla, y señaló que escribir una
  línea defendiendo una postura lo hace igual de bien —o mejor— la mitad no técnica.
- **El ganador es estable entre ejecuciones; el segundo puesto no.** `charla` cambió su
  finalista del tribunal a la sala contra el agente entre una ronda y otra, con el mismo
  material. Lo que se repite es la elección, no el orden completo.
- **La estimación sigue sin ser de fiar a esta resolución.** Tres ejecuciones del mismo
  evaluador sobre el mismo trabajo nominal han dado 64, 19 y 16 horas. El número no se
  puede usar para planificar; lo que sí se puede usar es el reparto relativo, que en las
  tres coloca al agente y a la interfaz como las fases que se desbordan.

## Lo que la deliberación cambió

Vale la pena dejar escrito en qué se equivocó la propuesta inicial, porque es la parte
que se enseña.

**La participación baja no era el riesgo.** Ocho justificaciones bien escritas siguen
dando dos o tres posturas y una premisa honesta, a diferencia de una votación, donde
ocho de cuarenta es un fracaso desnudo en pantalla. El problema real de que contesten
pocos es que el ponente no sabe si falló la aplicación o falló la gente.

**Lo que se perdió al pasar a remoto no es ancho de banda, es presión social.** En una
sala, no participar se ve. En casa, con el micro cerrado, no participar es invisible y
gratis.

**El agente, tal y como estaba especificado, no pasa la prueba del `if` disfrazado.**
Agrupar vectores y sacar un contraargumento de un catálogo produce la misma pantalla, y
el público no puede verificar la diferencia porque no ve las cuarenta frases. Lo único
que una tabla de reglas no puede falsificar es **la cita literal**: si el agente cita
textualmente frases del público, ha tenido que leerlas, y eso lo ve todo el mundo
empezando por quien las escribió.

**El riesgo central es un fallo que suena bien.** Como el enunciado del dilema es el
único estímulo del sistema —en remoto no hay pasillo ni conversación lateral—, la
premisa que encuentre el agente puede ser verdadera y circular: el eco del marco del
propio ponente. No falla ruidosamente, falla sonando profundo, y es indistinguible del
éxito sin un contrafactual delante. De ahí que el piloto con personas reales sea la
condición de supervivencia del proyecto y no una mejora opcional.

**La moderación va en la salida, no en la entrada.** Una línea inocua puede ser una
inyección contra el agente que la resume, y entonces la ofensa llega lavada, en
tipografía grande, en la pantalla compartida.

**Y el ataque barato en remoto no es escribir una barbaridad, es el volumen.** Con
fan-in, quien más escribe más pesa en la premisa compartida, y no hay coste social
ninguno en mandar cuarenta líneas desde una consola del navegador.

## Decisiones cerradas

- **La charla es en remoto, por Google Meet.** No hay sala, ni proyector, ni wifi
  compartido: cada asistente en su casa, con su red, con Meet en un portátil.
- **La aplicación es «el dilema»**, en la forma descrita arriba.
- **La vista común vive en dos sitios a la vez**: el ponente la comparte por Meet y
  además cada asistente puede abrirla en su navegador. *Tiene una objeción abierta, ver
  pendientes.*
- **Participar funciona igual desde el móvil y desde el portátil.** *Tiene una objeción
  abierta, ver pendientes.*
- **La lluvia tipo Matrix va completa**, con la animación acoplada a datos reales: cada
  justificación cae al entrar y se coloca según la salida del agrupamiento. *Tiene una
  objeción abierta, ver pendientes.*

## Lo que queda pendiente

Ninguna de estas está decidida. Se nombran como lo que son.

**Bloquean antes de construir.**

- **El dilema concreto.** No existe todavía. Es la pieza de la que depende todo: uno
  tibio no produce posturas que descubrir, uno con respuesta socialmente correcta
  produce cuarenta frases iguales. Quién lo escribe y quién lo valida.
- **Si hay piloto con personas reales.** Diez o quince personas, el enunciado exacto y
  un minuto de plazo, sin escribir una línea de código. Es lo único que distingue un
  hallazgo de un eco.
- **Identidad por dispositivo.** Hacen falta tres cosas distintas que se resuelven con
  lo mismo: deduplicar, emparejar la primera votación con la segunda, y limitar el
  volumen para que nadie inunde el fan-in. Un identificador aleatorio en el navegador,
  sin login ni dato personal, las cubre las tres. Sin él, la frase con la que cierra la
  charla no se puede calcular. Y queda sin resolver que móvil y portátil son dos
  identificadores para la misma persona, que es consecuencia directa de una de las
  decisiones cerradas.
- **El presupuesto de horas de construcción.** No está fijado. Las dos estimaciones
  disponibles difieren en un factor de tres sobre el mismo trabajo nominal.

**Sobre el agente.**

- Si el límite de longitud de la justificación sigue siendo una línea. Se decidió para
  pulgares de pie en una sala; ahora la gente está sentada con teclado.
- Si se añade un segundo campo del tipo «¿qué tendría que ser verdad para que el otro
  lado tuviera razón?», que fabrica materia prima para la premisa en vez de esperar a
  que aparezca.
- Cuál es la línea base contra la que se compara el agente en directo, si es que se
  compara. Contar votos no es la alternativa honesta.
- Si el análisis se ejecuta dos veces con encuadres distintos y se enseñan las dos
  salidas.
- Qué hace el agente por debajo de un umbral mínimo de respuestas, y si se completan
  con datos de un ensayo.

**Sobre la interfaz y el guion.**

- Si la vista común debe existir en un solo sitio. El screen share llega con segundos
  de retraso y comprimido mientras el navegador local va en vivo, así que el ponente
  narra un fotograma que nadie más está mirando.
- Si la lluvia sobrevive al códec de Meet y a competir por CPU con el encoder en la
  máquina del ponente. Se mide antes de construirla, no después.
- Si el texto literal del público se ve en pantalla o sólo se ven los argumentos
  extraídos.
- En qué minuto entra el público, y si la participación se parte en dos tiempos.
- Cómo se coloca y cuánto dura el replay de la deliberación.
- Anonimato o trazabilidad, quién opera la moderación mientras el ponente habla, y si
  la sesión se graba.

**Sobre la plataforma.**

- El stack. La única restricción firme que ha salido de la deliberación es que el
  estado no debe vivir detrás de un pool de conexiones, y que la llamada de análisis no
  cabe en una función con tope de diez segundos.
- El dominio, que es lo único con plazo externo real.

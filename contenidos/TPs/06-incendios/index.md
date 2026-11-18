---
title: "Incendios en una grilla"
subtitle: "Trabajo práctico 06"
---

Este práctico se apoya en tres clases:

- la [07](../../clases/07-1-markov-agentes.qmd): una regla local y aleatoria en una grilla, el frente de contagio y la percolación;
- la [08](../../clases/08-1-difusion.qmd): qué hace el paso de tiempo con una discretización;
- la [09](../../clases/09-1-reaccion-difusion.qmd): un frente con velocidad de referencia y la pregunta "¿física o grilla?".

Es grupal. La pregunta es: **¿qué parte del avance de un incendio simulado en una grilla es del fuego y qué parte es de la grilla, y desde dónde es peligroso que empiece un incendio?**

Las partes 1 y 2 trabajan sobre mapas artificiales, donde cada ingrediente aparece solo. La parte 3 usa un mapa generado con relieve, combustibles, lagos, ríos, calles y un pueblo. La parte 4 audita tres informes sobre ese mapa y cierra el paquete de afirmación. La 5, optativa, agrega pavesas al modelo.

::: callout-important

## Repositorio

La cátedra les enviará el enlace a su repositorio privado de grupo. Acepten la invitación de GitHub y clónenlo desde ese enlace. Todas las personas del grupo y el equipo docente trabajan sobre el mismo repositorio.
:::

## El modelo

El terreno es una grilla de celdas cuadradas de lado `dx`. Cada celda tiene una **tasa** de propagación, en m/min, que depende de su combustible; una celda sin combustible (roca, agua, una calle) tiene tasa cero. Cada celda está sin quemar, ardiendo o apagada.

**El autómata** (`src/incendio/automata.py`) avanza en pasos de tiempo `dt`. Una celda ardiendo $i$ le transmite a su vecina $j$, a distancia $d$, una tasa de ignición

$$
\lambda_{ij} = \frac{\text{tasa}_j \, f_\text{viento} \, f_\text{pendiente}}{d}
\qquad [1/\text{min}],
$$

y las tasas que recibe una celda de todas sus vecinas ardiendo se suman. Las vecinas son las cuatro de los lados (vecindad de **von Neumann**) o las ocho de lados y esquinas (vecindad de **Moore**). Todas las celdas se actualizan a la vez. Una celda arde durante su **tiempo de quemado** (`tiempo_quemado`, en minutos) y después se apaga.

**El camino mínimo** (`src/incendio/camino.py`) es otra manera de calcular el tiempo de llegada del fuego, con las mismas tasas y sin azar. Se explica más abajo.

### Un paso del autómata

`simular`, en `src/incendio/automata.py`, guarda para cada celda si está sin quemar, ardiendo o apagada, y su tiempo de llegada. Los focos arden desde `t = 0`. En cada paso de duración `dt`:

1.  cada celda sin quemar suma las tasas $\lambda_{ij}$ que le transmiten sus vecinas que están ardiendo al empezar el paso;
2.  con esa tasa total se sortea si la celda se enciende durante el paso, con la probabilidad que da `probabilidad_de_ignicion` (parte 1.3), que supone la tasa constante durante todo el paso;
3.  todas las celdas se actualizan a la vez: las que se encendieron registran como tiempo de llegada el final del paso, `t = paso · dt`, y pasan a arder;
4.  las celdas que cumplieron su tiempo de quemado se apagan; el tiempo de quemado se cuenta en pasos enteros, `ceil(tiempo_quemado / dt)`;
5.  la corrida termina cuando ya nada puede encenderse o se llega a `t_max`.

De la regla salen tres propiedades, válidas para cualquier `dt`:

-   los tiempos de llegada son múltiplos de `dt`;
-   una celda que se enciende durante un paso no contagia a nadie en ese mismo paso, sino recién a partir del siguiente; por eso, en cada paso el fuego avanza a lo sumo una vecina;
-   una celda arde un número entero de pasos, aunque `tiempo_quemado / dt` no sea entero.

En tiempo continuo no hay nada de esto: una celda puede encenderse en cualquier instante y empezar a contagiar enseguida. Cuánto cambia el frente por avanzar en pasos finitos, y de qué depende, es la pregunta de la parte 2.

### Caminos mínimos en una grilla

Piensen la grilla como un **grafo**: cada celda es un nodo, unido a cada una de sus vecinas (cuatro u ocho, según la vecindad) por una **arista dirigida**. Pasar de la celda $i$ a su vecina $j$ tarda

$$\tau_{ij} = \frac{1}{\lambda_{ij}},$$

el tiempo medio que tarda $i$ en encender a $j$ (parte 1.3). Como $\lambda_{ij}$ depende de la celda que se enciende, del viento y de la pendiente, ir de $i$ a $j$ no tiene por qué tardar lo mismo que volver. Un **camino** del foco a una celda es una sucesión de aristas, y tarda la suma de sus $\tau$. El tiempo de llegada a la celda es el del camino más rápido:

$$t_j = \min_{\text{caminos del foco a } j} \; \sum_{\text{aristas del camino}} \tau .$$

Es lo que haría un fuego que, en cada celda, encendiera a cada vecina exactamente en el tiempo medio. Probar todos los caminos es imposible: hay demasiados. El **algoritmo de Dijkstra**, el estándar para caminos mínimos en grafos con aristas de duración positiva, lo resuelve en un solo recorrido:

1.  el foco tiene tiempo 0, y todas las demás celdas, infinito;
2.  entre las celdas que todavía no están fijas, se toma la de menor tiempo y se **fija**: ningún otro camino puede llegar antes, porque tendría que pasar por celdas con tiempos mayores y las aristas no tardan tiempos negativos;
3.  para cada vecina $j$ de la celda $i$ recién fijada, si $t_i + \tau_{ij} < t_j$, se cambia $t_j$ por $t_i + \tau_{ij}$;
4.  se repite desde el paso 2 hasta fijar todas las celdas alcanzables.

Las celdas se fijan en el orden en que el fuego las alcanzaría: el algoritmo es un frente que avanza. `camino.grafo` arma la matriz con los $\tau_{ij}$ y `camino.tiempo_minimo` le aplica la implementación de `scipy.sparse.csgraph.dijkstra`.

**Ejemplo.** Una grilla de 3 × 3 con la vecindad de von Neumann, todas las aristas de 1 min y la celda del centro sin combustible. Desde la esquina de arriba a la izquierda, la esquina opuesta se alcanza en 4 min, por arriba o por la izquierda: el algoritmo fija primero las dos vecinas de la esquina (1 min), después las otras dos esquinas (2 min), después las celdas del medio de los bordes de abajo y de la derecha (3 min) y por último la esquina opuesta (4 min).

En un mapa uniforme y sin obstáculos, el tiempo a una celda depende sólo de cuántos pasos de cada tipo tiene el camino más rápido para llegar a ella: de lado y, con la vecindad de Moore, también en diagonal. Contarlos es la parte 1.2.

Con una semilla (parte 2), cada arista tarda un tiempo al azar en lugar de $\tau_{ij}$, y el algoritmo es el mismo.

El repositorio trae:

- `src/incendio/mapas.py`: mapas artificiales (uniforme, dos combustibles, ladera, barreras rectas y diagonales);
- `src/incendio/automata.py`: el autómata, con un hueco;
- `src/incendio/camino.py`: el camino mínimo;
- `src/incendio/frente.py`: la medición de la velocidad del frente, con un hueco;
- `src/incendio/ensamble.py`: semillas independientes y promedios con barra de error;
- `src/incendio/generado.py` y `datos/`: el mapa generado de la parte 3;
- `auditoria/`: los informes de la parte 4;
- `src/incendio/pavesas.py`: las pavesas de la parte 5, optativa.

Los parámetros de las corridas están en `experiments/parametros.py`. En las partes 1 y 2, `DX = 10 m` y `TASA = 10 m/min`.

## Cómo trabajar

Las preguntas de cada parte no se contestan sueltas: siguen el ciclo del [paquete de afirmación](../../la-materia/03-entrega-tps.md).

- Lo marcado **Antes de correr** va a `question.md` antes de ejecutar el experimento, con fecha y commit: qué esperan, por qué, y qué resultado les haría aceptarlo o rechazarlo. Si después cambian de idea, agregan una sección nueva; no reescriben la anterior.
- Las demás preguntas se contestan en `lab.md`, con una entrada por experimento: qué esperaban, qué corrieron, qué vieron y cómo lo interpretan.
- Las partes 2 y 3 cierran con una afirmación parcial en `lab.md`.

## Parte 1 — Un frente con respuesta conocida

Como el frente de Fisher de la clase 09, antes de creerle al autómata hay que probarlo donde se sepa la respuesta. El caso más simple es un mapa plano, sin viento y con el mismo combustible en todas partes.

1.  **Medir la velocidad del frente.** Completen `velocidad_del_frente` en `src/incendio/frente.py`: para cada rumbo, una recta de distancia al foco contra tiempo de llegada. Los tests de `tests/test_frente.py` la prueban con mapas de tiempos construidos a mano.
2.  **El camino mínimo.** **Antes de correr:** en un mapa uniforme, ¿qué forma tienen las curvas de igual tiempo de llegada del camino mínimo con cada vecindad? ¿Cuánto vale la velocidad del frente en los rumbos 0°, 22.5° y 45°, en unidades de `TASA`? Después completen `test_velocidad_del_camino_minimo_en_un_mapa_uniforme` en `tests/test_camino.py`. En `lab.md`: ¿qué forma esperarían para un incendio real en un pastizal plano y uniforme, sin viento?
3.  **El paso del autómata.** Completen `probabilidad_de_ignicion` en `src/incendio/automata.py`: la probabilidad de que una celda que recibe una tasa total $\lambda$, constante, se encienda durante un paso `dt`. `test_tiempo_medio_de_ignicion_de_una_vecina` lo controla. En `lab.md`: ¿qué se supone sobre las vecinas al sumar sus tasas?
4.  **El autómata en el mapa uniforme.** **Antes de correr:** con la misma tasa, ¿el frente del autómata va más rápido, más lento o igual que el del camino mínimo? ¿Qué forma tiene? Corran `uv run python experiments/frente_plano.py` y miren `results/frente-plano.png`. En `lab.md`: ¿qué vieron? ¿Cómo lo explican?

## Parte 2 — ¿Fuego o grilla?

En la clase 09, la pregunta era si un patrón es del modelo o de la grilla. Acá, el modelo tiene una sola escala física por combustible, la tasa. Todo lo demás —`dx`, `dt`, la vecindad, el tiempo de quemado— es una elección. La pregunta es cuánto cambian el frente esas elecciones.

1.  **El paso de tiempo.** **Antes de correr:** ¿qué esperan que pase con la velocidad del frente al achicar `dt`? Lean la documentación de `grafo` y `tiempo_minimo` en `camino.py`: con `semilla`, cada arista tarda un tiempo al azar con distribución exponencial. En `question.md`, argumenten si esa versión debería coincidir con el autómata cuando `dt → 0`, y por qué. Corran `uv run python experiments/convergencia_dt.py`. En `lab.md`: ¿el autómata converge al achicar `dt`? ¿A qué? ¿Qué combinación adimensional de `TASA`, `dt` y `DX` controla la diferencia? Como pista: en las clases 08 y 09, el esquema de difusión dependía de $\Delta t$ y $\Delta x$ sólo a través de $r = D\,\Delta t/\Delta x^2$; busquen el análogo acá y pónganlo a prueba en el punto 2. ¿El `dt` de la parte 1 era adecuado?
2.  **Dos combustibles con el mismo `dt`.** El mapa de `experiments/dos_combustibles.py` tiene un combustible diez veces más lento en la mitad Oeste. **Antes de correr:** ¿el cociente entre las velocidades del frente al Este y al Oeste es el cociente entre las tasas? ¿Depende de `dt`? Córranlo y expliquen el resultado en `lab.md` con lo que encontraron en el punto 1.
3.  **Cuánto arde una celda.** Con tiempo de quemado finito, una celda puede apagarse antes de encender a una vecina. **Antes de correr:** con la vecindad de von Neumann y en el límite `dt → 0`, ¿cuál es el tiempo de quemado mínimo para que el fuego pueda atravesar un mapa muy grande? Dato: en una red cuadrada donde cada enlace está abierto con probabilidad $p$, independientemente de los demás, aparece un camino abierto que la atraviesa si $p > 1/2$; es el umbral de **percolación de enlaces**. Piensen qué probabilidad tiene cada arista de transmitir antes de que su celda se apague. Corran `uv run python experiments/tiempo_quemado.py`. En `lab.md`: ¿coincide con su predicción? ¿Por qué la transición no es abrupta en un mapa de 81 × 81? ¿Qué cambia con la vecindad de Moore? ¿Qué tiempo de quemado tendría sentido físico para una celda de 30 m de pastizal?
4.  **Viento, pendiente y barreras.** Corran `uv run python experiments/viento_pendiente.py`. En `lab.md`: ¿qué forma toma el frente con viento y en una ladera? Con los factores de `automata.py`, ¿cuánto más rápido sube el fuego que lo que baja? Comparen con alguna regla empírica de la propagación en pendiente que encuentren y citen. ¿Por qué una barrera diagonal de una celda no detiene el fuego con la vecindad de Moore? ¿Lo detendría con la de von Neumann?
5.  **Calibración.** En la parte 3, cada combustible va a tener una velocidad de referencia tomada de una tabla, en m/min. Escriban `experiments/calibracion.py`, que genere `results/calibracion.csv`: para la vecindad y el `dt` que elijan, qué tasa hay que poner en el mapa para que el frente del autómata avance a una velocidad de referencia dada, con su barra de error. Usen al menos dos velocidades de referencia, una 25 veces mayor que la otra.
6.  **Afirmación parcial.** En `lab.md`: qué propiedades del frente son del modelo y cuáles de la grilla, con qué evidencia y dentro de qué rango de `dt` y tiempo de quemado.

## Parte 3 — ¿Desde dónde es peligroso?

Ahora el mapa no es artificial. `datos/ladera-1` es un mapa generado de 256 × 256 celdas de 30 m (7.7 km de lado) con relieve, cinco clases de vegetación, lagos, ríos, calles y un pueblo. `src/incendio/generado.py` lo lee:

- `leer(ruta)` devuelve un `MapaGenerado` con la cobertura, la elevación, el viento, tres focos sugeridos por el generador, la **zona del pueblo** (las celdas con combustible a dos celdas o menos de una construcción) y la **velocidad de referencia** de cada celda, en m/min;
- `MapaGenerado.mapa(tasa, tiempo_quemado)` arma el `Mapa` que usa el simulador;
- `llegada(t_llegada, zona)` da el primer tiempo en que el fuego entra en una zona.

La velocidad de referencia sale de una tabla por clase de vegetación, en terreno plano y sin viento, reducida por la humedad del combustible muerto; la fórmula está en la documentación del módulo. Es una velocidad del fuego, no una tasa del autómata: la calibración del punto 2.5 hace la conversión.

El **escenario** de esta parte, con los valores de `experiments/parametros.py`:

- la tasa de cada celda es la que da su velocidad de referencia según la calibración de ustedes;
- una celda arde `TIEMPO_QUEMADO_EN_CRUCES · dx / v` minutos, con `v` su velocidad de referencia: lo que tarda el frente en cruzar tres celdas;
- el viento es el del mapa, y cada corrida se corta a las `HORIZONTE` = 12 h;
- el fuego **llega al pueblo** cuando enciende alguna celda de su zona.

1.  **El mapa.** Corran `uv run python experiments/mapa_generado.py` y miren `results/mapa-generado.png`. Elijan al menos tres focos además de los sugeridos, cada uno con un motivo: a favor o en contra del viento, cuesta arriba o cuesta abajo del pueblo, del otro lado de un río o de una calle, en un combustible rápido o lento. **Antes de correr:** ordenen todos los focos de más a menos peligrosos para el pueblo y estimen, para cada uno, cuánto tarda el fuego en llegar.

2.  **La calibración en otro mapa.** Las celdas son de 30 m, no de 10. ¿Sigue valiendo la calibración del punto 2.5? ¿Qué rango de velocidades de referencia hay en este mapa, y lo cubre su calibración? Si hace falta, extiéndanla.

3.  **Ensambles por foco.** Escriban `experiments/focos.py`, que genere `results/focos.csv`. Para cada foco, un ensamble de corridas independientes debe dar:
    - la probabilidad de llegar al pueblo antes del horizonte, con su barra de error;
    - la distribución del tiempo de llegada, no sólo su promedio;
    - el área quemada.

    Decidan cuántas corridas hacen falta y justifíquenlo. En `lab.md`: ¿qué focos son peligrosos? ¿Coincide con su predicción? Si tuvieran que avisarle al pueblo cuánto tiempo tiene desde que se detecta cada foco, ¿qué número le darían, y por qué ese y no el promedio?

4.  **Sin azar.** Calculen el tiempo de llegada al pueblo desde los mismos focos con el camino mínimo determinista, con la velocidad de referencia como tasa. En `lab.md`: ¿ordena los focos igual que los ensambles? ¿Para qué preguntas serviría como atajo y para cuáles no?

5.  **¿Fuego o grilla, otra vez?** Repitan los ensambles, para al menos los dos focos más dudosos, cambiando una cosa por vez: la otra vecindad (con su calibración), otro `dt`, el tiempo de quemado multiplicado por 2/3 y por 2, y viento de 10 m/s del mismo rumbo. **Antes de correr:** ¿qué esperan que cambie y qué no? En `lab.md`: ¿qué conclusiones del punto 3 sobreviven? Si algún resultado depende de la vecindad, encuentren en el mapa las celdas responsables y digan si el fuego real haría lo mismo.

6.  **Afirmación parcial.** En `lab.md`: desde dónde es peligroso que empiece un incendio para el pueblo, con qué probabilidad, con qué tiempo de aviso, bajo qué supuestos, y qué parte de esa respuesta depende de la grilla. Entre las limitaciones: en un incendio real, una plantación de pinos puede pasar a fuego de copa. ¿Qué hace este modelo cuando el fuego entra en un pinar? ¿Qué le faltaría para representar ese peligro, y con qué capas del mapa generado lo harían?

## Parte 4 — Auditoría y afirmación final

1.  **Auditoría.** Un asistente de IA produjo tres informes sobre el mismo mapa, en `auditoria/`. Audítenlos siguiendo `auditoria/README.md` y registren cada auditoría en `lab.md`. Usen lo que ya tienen de la parte 3: sus ensambles y sus corridas sin azar son controles independientes.
    - El informe A necesita `results/convergencia-dt.csv` de la parte 2.
    - Los informes B y C contestan la misma pregunta con dos cálculos distintos y no dan lo mismo. Decidan cuál tiene razón, o si ninguno, y expliquen de dónde sale la diferencia.
    - Del informe que acepten, comparen su mapa con lo que encontraron en los ensambles: ¿en qué focos coinciden y en cuáles no? ¿Por qué?
2.  **Afirmación final.** Completen en `lab.md` la afirmación vigente, que contesta la pregunta del práctico en sus dos mitades: qué parte del avance es del fuego y qué parte de la grilla, y desde dónde es peligroso que empiece un incendio. Incluyan las limitaciones y las explicaciones alternativas compatibles con los datos. Completen `ai-use.md`.

## Parte 5 — Pavesas (optativa)

Hasta acá, el fuego sólo pasa de una celda a sus vecinas, y una franja sin combustible de una celda de ancho lo detiene. En un incendio real, el viento levanta **pavesas**, fragmentos encendidos que pueden caer a decenas o cientos de metros y encender combustible del otro lado de un río, de una calle o de un bosque que el frente tardaría horas en cruzar. Esta parte es optativa: agrega pavesas al autómata, primero en un mapa artificial donde se puede estimar el resultado con lápiz y papel, y después, si les da el tiempo, en el mapa generado.

`src/incendio/pavesas.py` describe el modelo: cada celda que arde emite pavesas al azar, a una tasa `emision`; cada pavesa vuela viento abajo una distancia al azar con distribución exponencial de media `distancia_media` y, si cae en combustible sin quemar, lo enciende con probabilidad `probabilidad`. La función `lanzar` del mismo módulo, que ya viene resuelta, lanza las pavesas de un paso, y `simular` acepta un argumento `pavesas`. `tests/test_pavesas.py` muestra qué se controló. Las constantes de `experiments/parametros.py` son ilustrativas: no salen de mediciones. El salto medio crece con el viento: `PAVESAS_DISTANCIA_POR_VIENTO` metros por cada m/s.

La **franja** es el mapa uniforme de las partes 1 y 2, de `N_FRANJA` celdas por lado, con tiempo de quemado `TIEMPO_QUEMADO_FRANJA`, viento del Oeste de `VIENTO_FRANJA` m/s y una barrera sin combustible (`con_barrera`) que empieza en la columna del medio. El fuego empieza en toda la columna del borde Oeste, así que llega a la barrera como un frente recto. Usen la vecindad de Moore y `dt = DT`.

1.  **Cuántas pavesas tira una celda.** ¿Cuántas pavesas emite en total una celda que arde durante un tiempo $\tau$ con emisión $e$?
2.  **Estimar antes de correr.** Supongan que todas las celdas al Oeste de la barrera arden, que cada pavesa vuela derecho viento abajo y que los encendidos del otro lado son independientes. Escriban el número medio $\bar N$ de pavesas que encienden algo del otro lado de una barrera de ancho $W$, y la probabilidad de que el fuego la cruce. En `question.md`, calculen esa probabilidad para cada ancho de `ANCHOS_FRANJA` y cada emisión de `EMISIONES_FRANJA`.
3.  **Barrido en la franja.** Escriban `experiments/pavesas.py`, que genere `results/pavesas.csv`: para cada ancho y cada emisión, la probabilidad de que el fuego aparezca del otro lado de la barrera antes de `T_FRANJA`, con `N_SEMILLAS_FRANJA` corridas y su barra de error. Midan también cuánto tarda una corrida. En `lab.md`: ¿la estimación acierta? ¿Dónde se equivoca, en qué sentido, y qué de lo que supusieron no es cierto en el autómata?
4.  **Si les da el tiempo: pavesas en el mapa generado.** En el escenario de la parte 3, tomen la emisión proporcional a la **intensidad** del frente, carga de combustible por velocidad: `emisión = c · carga · v / dx`, con `carga` en kg/m² (`MapaGenerado.carga`) y `c` un coeficiente sin calibrar.
    - Con el tiempo de quemado de la parte 3, ¿cuántas pavesas tira en total una celda de cada clase de vegetación? ¿Depende de su velocidad?
    - **Antes de correr:** con la corrida de la parte 3 que ya midieron, estimen cuánto tardaría en su máquina un barrido de los `c` de `EMISIONES_MAPA` para dos focos con 20 corridas cada uno. Si es razonable, córranlo para un foco detrás del bosque al Oeste del pueblo y uno detrás del río y la calle.
    - En `lab.md`: ¿desde qué `c` cambia la respuesta de la parte 3? ¿Qué barrera cae primero, y cómo lo explican con lo que encontraron en la franja?
5.  **La afirmación, otra vez.** Agreguen a la afirmación vigente de `lab.md` qué parte de la respuesta podría cambiar con un mecanismo, como las pavesas, que el modelo de la parte 3 no tenía y cuyo parámetro nadie calibró.

## Versionar y entregar

Un commit por cada cambio que deja los tests en verde, con un mensaje que diga **por qué**. La forma de entrega está a definir.

## Qué deberían poder explicar al terminar

- Por qué un autómata en una grilla cuadrada puede producir frentes con forma de rombo u octógono, y qué hace el azar con esa forma.
- Por qué la tasa que se le pone a una celda no es la velocidad del frente.
- Qué hace el paso de tiempo en una actualización síncrona, y cómo se controla con una referencia independiente.
- Cómo un tiempo de quemado finito convierte la propagación en un problema de percolación.
- Qué hay que calibrar antes de usar el autómata para comparar focos en un mapa.
- Por qué el peligro de un foco es una probabilidad con barra de error, y qué número de tiempo de aviso tiene sentido dar.
- Cuándo un cálculo determinista hacia atrás reemplaza a muchas corridas hacia adelante, y qué pregunta deja de contestar.
- Si hicieron la parte 5: cómo estimar con lápiz y papel el efecto de un mecanismo nuevo antes de simularlo, y qué pasa con una conclusión cuando ese mecanismo tiene un parámetro sin calibrar.

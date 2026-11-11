---
title: "Dinámica molecular y Metropolis"
subtitle: "Trabajo práctico 05"
---

Este práctico se apoya en la clase
[Dinámica molecular](../../clases/05-1-dinamica-molecular.qmd) y en la clase 06,
de Monte Carlo y Metropolis. Es grupal y **con entrega**. La
pregunta es: **¿coinciden los promedios de los observables a lo largo de una
trayectoria de dinámica molecular con los estimados a partir de configuraciones
de Metropolis para un fluido de Lennard-Jones, y con qué barra de error?**

::: {.callout-important}

## Repositorio

La cátedra les enviará el enlace a su repositorio privado de grupo. Acepten la
invitación de GitHub y clónenlo desde ese enlace. Todas las personas del grupo
y el equipo docente trabajan sobre el mismo repositorio.

:::

## El problema

256 partículas de Lennard-Jones, cortado y desplazado en $r_c=2.5\sigma$, en
una caja periódica con densidad $\rho^*=0.75$. El repositorio trae:

- `src/fluido/lj.py`: el potencial, las fuerzas por suma directa y con listas
  de celdas, y la energía de una partícula;
- `src/fluido/dinamica.py`: la dinámica molecular de la clase, con etapas de
  equilibración y producción;
- `src/fluido/observables.py`: $g(r)$ y desplazamiento cuadrático medio;
- `src/fluido/errores.py` y `src/fluido/metropolis.py`: con huecos.

Los parámetros de las corridas están en `experiments/parametros.py`.

## Cómo trabajar

Las preguntas de cada parte no se contestan sueltas: siguen el ciclo del
[paquete de afirmación](../../la-materia/03-entrega-tps.md).

- Lo marcado **Antes de correr** va a `question.md` antes de ejecutar el
  experimento, con fecha y commit: qué esperan, por qué, y qué resultado les
  haría aceptarlo o rechazarlo. Si después cambian de idea, agregan una sección
  nueva; no reescriben la anterior.
- Las demás preguntas se contestan en `lab.md`, con una entrada por
  experimento: qué esperaban, qué corrieron, qué vieron y cómo lo interpretan.
- La parte 4 cierra `lab.md` con la afirmación sobre la pregunta del práctico.

## Parte 1 — Barras de error

La clase 05 terminó con una pregunta abierta: ¿cuántas mediciones
independientes hay en una trayectoria? La clase 06 dio la herramienta para
contestarla, el análisis por bloques. `error_por_bloques` les va a dar las
barras de error de todo el práctico, así que lo implementan y lo prueban antes
de simular el fluido.

1. **Un caso con respuesta conocida.** Antes de creerle a `error_por_bloques`
   hay que probarlo donde sepamos la respuesta, como en la sección 6.4 de la
   clase 06, donde la cuadratura del pozo permitió comprobar la barra. Para el
   fluido no la sabemos. Para muestras independientes sí, pero ese caso no
   prueba nada: cualquier tamaño de bloque da el error ingenuo. Hace falta una
   serie **correlacionada** cuyo error conozcamos.

   La más simple es una serie con memoria de un paso,
   $x_k=\varphi\,x_{k-1}+\eta_k$, con $\eta_k$ un ruido gaussiano nuevo en
   cada paso. $\varphi$ dice cuánto recuerda cada valor del anterior: con
   $\varphi=0$ las muestras son independientes; con $\varphi=0.9$ cada valor
   conserva el 90 % del anterior. Es como la velocidad de una partícula
   browniana: la fricción le quita una fracción de la velocidad en cada
   intervalo y los choques le dan una patada al azar. En estadística se llama
   **proceso autorregresivo de orden uno**, AR(1); en el código, `proceso_ar1`.

   Lo útil es que todo se calcula: su autocorrelación es $C(k)=\varphi^k$
   (lo controla `test_autocorrelacion_de_un_ar1`), y con la fórmula de
   $\tau_\text{int}$ de la clase 06 sale el error exacto de su promedio.
   Completen `error_por_bloques` en `src/fluido/errores.py` y
   `test_bloques_recuperan_el_error_de_un_proceso_correlacionado` en
   `tests/test_errores.py`: hagan esa cuenta y comprueben que la meseta de los
   bloques la reproduce.

## Parte 2 — Dinámica molecular validada

1. **Antes de correrlo**, anoten en `question.md` cómo esperan que dependa de
   `dt` la fluctuación de la energía total. Después completen
   `test_conservacion_de_energia_sin_termostato` en `tests/test_dinamica.py`.
2. Corran `uv run python experiments/dinamica.py` (alrededor de un minuto) y
   revisen `results/dinamica.png`. En `lab.md`: ¿la energía se conserva en la
   producción? ¿Dónde se usó el termostato?
3. Corran `uv run python experiments/errores.py` y miren
   `results/errores-dinamica.png`. Elijan `BLOQUE["dinamica"]` y justifíquenlo.
   En `lab.md`: ¿cuántas veces más grande que el ingenuo es el error de cada
   promedio? ¿Cuántas mediciones independientes hay, aproximadamente, en la
   corrida?
4. Opcional: comparen el tiempo de `simular` con `calcular_fuerzas=fuerzas` y
   con `fuerzas_celdas` para `n = 6` y `n = 8`.

## Parte 3 — Metropolis

La dinámica molecular promedia a lo largo de una trayectoria, con la energía
total fija. Metropolis genera una cadena de configuraciones a temperatura fija:
no hay velocidades ni tiempo físico. Después de la equilibración, esas
configuraciones aparecen con peso $e^{-U/T}$.

1. **El movimiento.** Completen `barrido` en `src/fluido/metropolis.py`. Un
   barrido recorre las $N$ partículas en orden. A cada una le propone un
   desplazamiento uniforme en el cubo $[-\delta,\delta]^3$. $\delta$ es la
   **amplitud de propuesta**, guardada como `PASO_METROPOLIS`, y se acepta o
   rechaza con el criterio de Metropolis. $\Delta U$ es el cambio de energía de
   esa partícula con todas las demás, que da
   `energia_de_una_particula`. Si se rechaza, la partícula queda donde
   estaba. Las posiciones se mantienen dentro de la caja periódica. Al terminar
   el barrido, el programa registra un estado de la cadena; los promedios de
   Metropolis se calculan sobre esos estados. Barridos consecutivos siguen
   correlacionados, por eso también se usan bloques.
2. **Un caso con respuesta conocida: dos partículas.** Como con
   `error_por_bloques`, antes de usar `barrido` con 256 partículas lo prueban
   donde se conoce la respuesta. Con sólo dos partículas en la caja, lo único
   que importa es la distancia $s$ entre ellas, y su distribución se deduce de
   la de Boltzmann sin pasar por Metropolis, al menos para $s<L/2$. Completen
   `test_dos_particulas_siguen_la_distribucion_de_boltzmann` en
   `tests/test_metropolis.py`: deduzcan esa distribución, apliquen muchos
   barridos a dos partículas y comparen el histograma de las distancias con
   ella. En `lab.md`: ¿por qué la deducción vale sólo para $s<L/2$?
3. **Antes de correr:** ¿qué esperan que pase con la aceptación y con la
   correlación si la amplitud de propuesta $\delta$ es muy chica o muy grande?
   Después elijan `PASO_METROPOLIS` en `experiments/parametros.py` y justifiquen
   la elección en `lab.md`.
4. Corran `uv run python experiments/metropolis.py` (uno o dos minutos), que
   usa la temperatura medida en la dinámica molecular, y
   `uv run python experiments/errores.py metropolis`. Elijan
   `BLOQUE["metropolis"]`.

## Parte 4 — Paquete de afirmación: la comparación

Las partes anteriores dejan dos simulaciones del mismo fluido: 256 partículas,
$\rho^*=0.75$ y el mismo potencial cortado. Difieren en qué se fija:

- la **dinámica molecular** fija la energía total; la temperatura es un
  resultado, el promedio de la temperatura instantánea en la producción;
- **Metropolis** necesita una temperatura: `experiments/metropolis.py` usa la
  que midió la dinámica molecular.

Los comparamos en el estado definido por la temperatura media estimada en la
dinámica molecular. La pregunta del práctico es si sus promedios coinciden
dentro de las barras de error, con ese alcance.
`experiments/comparar.py` compara tres observables:

| Observable | Qué mide                          | Dinámica molecular                     | Metropolis                      |
| ---------- | --------------------------------- | -------------------------------------- | ------------------------------- |
| $U/N$      | energía potencial por partícula   | promedio de la serie `U`               | promedio de la serie `U`        |
| $P$        | presión, por el virial (clase 05) | $\rho T + W/3V$ con la $T$ instantánea | $\rho T + W/3V$ con la $T$ fija |
| $g(r)$     | estructura del líquido            | configuraciones guardadas              | configuraciones guardadas       |

Para $U/N$ y $P$ informa los dos promedios con su barra por bloques y la
diferencia dividida por el error combinado,
$(\bar A_\text{DM}-\bar A_\text{MC})/\sqrt{\sigma_\text{DM}^2+\sigma_\text{MC}^2}$.
Para $g(r)$ hace lo mismo en cada intervalo de $r$.

1. **Antes de correr la comparación**, completen en `question.md` qué esperan
   que coincida entre los dos métodos y qué no, y con qué criterio van a
   decidir si coinciden. Piensen también en observables que no están en la
   tabla, como las fluctuaciones.
2. Elijan los bloques de los promedios y de las configuraciones en
   `experiments/comparar.py` y córranlo. Para $g(r)$, el bloque está medido en
   configuraciones guardadas: elijan una duración al menos tan grande como la
   del bloque escalar correspondiente y dejen al menos 16 bloques. Genera
   `results/comparacion.csv` y `results/comparacion.png`. La temperatura que
   usa Metropolis tiene su propia barra de error, que `comparar.py` informa
   pero no propaga a $U/N$ y $P$: la comparación es **condicional** a esa
   temperatura, y la afirmación tiene que decirlo.
3. **Auditoría.** Un asistente de IA produjo dos informes sobre la misma
   comparación, en `auditoria/`. Audítenlos siguiendo `auditoria/README.md` y
   registren cada auditoría en `lab.md`.
4. Completen `lab.md` y `ai-use.md`. La afirmación tiene que decir qué
   coincide, con qué incertidumbre y qué no se espera que coincida.

Ver [Paquete de afirmación](../../la-materia/03-entrega-tps.md).

## Versionar y entregar

Un commit por cada cambio que deja los tests en verde, con un mensaje que diga
**por qué**. La forma de entrega está a definir.

## Qué deberían poder explicar al terminar

- Qué control tiene cada parte de una simulación de dinámica molecular.
- Por qué la desviación estándar de una serie no es la incertidumbre de su
  promedio, y cómo se estima esta última.
- Cómo se valida Metropolis con un caso de distribución exacta.
- Qué esperan que coincida entre un ensamble microcanónico y uno canónico, y
  qué no.

---
title: "Colapso frío y gravedad aproximada"
subtitle: "Trabajo práctico 04"
---

Este práctico continúa la sección de N cuerpos de la clase
[Tres cuerpos, caos y N cuerpos gravitatorios](../../clases/04-1-n-cuerpos.qmd).
Es grupal y **con entrega**. La pregunta es: **¿cuánto podemos aproximar la
gravedad de muchos cuerpos sin cambiar lo que medimos?**

::: {.callout-important}

## Repositorio

La cátedra les enviará el enlace a su repositorio privado de grupo. Acepten la
invitación de GitHub y clónenlo desde ese enlace. Todas las personas del grupo
y el equipo docente trabajan sobre el mismo repositorio; no creen una copia
desde un template.

:::

## El problema

Usamos unidades con $G=M=R=1$. Una esfera uniforme de $N$ partículas, de masa
total $M$ y radio $R$, empieza en reposo y colapsa por su propia gravedad. La
simulamos con cada método y comparamos el resultado con una referencia exacta.

### La referencia: el colapso de una esfera continua

La simulación no tiene solución exacta. La que tiene solución es una versión
idealizada del mismo sistema: una **esfera continua**, con la misma densidad
uniforme, pero hecha de un medio continuo en lugar de partículas, y sin
suavizado. Lo que se resuelve ahí es **cómo cambia con el tiempo el radio de
cada cáscara delgada de esa esfera**.

Tomemos la cáscara que empieza en el radio $r_0$, y llamemos $r(t)$ a su radio
en el instante $t$. Por el teorema de la cáscara esférica (*shell theorem*),
que se deduce de la ley de Gauss para la gravedad, la cáscara siente sólo la
masa que tiene adentro, como si estuviera toda en el centro:
$M(<r_0)=M\,(r_0/R)^3$. Esa masa no cambia mientras ninguna otra cáscara la
cruce. Entonces $r(t)$ cumple la ecuación de una caída libre radial desde el
reposo,

$$
\ddot r=-\frac{G\,M(<r_0)}{r^2},\qquad r(0)=r_0,\qquad \dot r(0)=0 .
$$

La solución no se puede escribir como una función explícita $r(t)$, pero sí en
forma paramétrica:

$$
\frac{r}{r_0}=\cos^2\eta,\qquad
\frac{t}{t_{ff}}=\frac2\pi\left(\eta+\sin\eta\cos\eta\right),\qquad
0\le\eta\le\frac\pi2 .
$$

El parámetro $\eta$ no tiene otro significado: va de $0$, al comienzo, a
$\pi/2$, cuando la cáscara llega al centro. Para un $t$ dado se obtiene $\eta$
de la segunda ecuación y $r$ de la primera; lo hace `radio_homologo`. La curva
$r(t)$ es un arco de cicloide.

Dos consecuencias:

- $M(<r_0)/r_0^3$ es igual para todas las cáscaras, así que todas siguen la
  **misma** curva $r/r_0$: la esfera se achica sin dejar de ser uniforme, y
  ninguna cáscara alcanza a otra.
- Todas llegan juntas al centro en el **tiempo de caída libre**,
  $$
  t_{ff}=\frac\pi2\sqrt{\frac{R^3}{2GM}} .
  $$
  Ahí la solución deja de valer. En la simulación, después de $t_{ff}$ las
  cáscaras se cruzan y el sistema se relaja hacia un equilibrio.

Además, el campo gravitatorio de la esfera continua en el instante inicial es

$$
\mathbf g(\mathbf r)=\begin{cases}
-GM\,\mathbf r/R^3 & r<R,\\
-GM\,\mathbf r/r^3 & r\ge R .
\end{cases}
$$

Lo calcula `campo_esfera_uniforme`, y es la referencia de las partes 2 y 3.

### Observables

- **Radios lagrangianos.** En cada instante, el radio de la esfera centrada en
  el centro de masa que contiene una fracción fija de la masa: 10 %, 50 % y
  90 %. Los calcula `radios_lagrangianos`. No siguen a partículas fijas: si las
  partículas se reordenan, la esfera que contiene esa fracción incluye a
  otras. En la esfera continua, como las cáscaras no se cruzan, el radio
  lagrangiano de cada fracción es el radio de una misma cáscara: hasta
  $t_{ff}$, dividido por su valor inicial, tiene que seguir la curva $r/r_0$
  de arriba. Esa es la comparación de la parte 1.
- **Energías.** La cinética, $K=\sum_i\frac12m_iv_i^2$, y la potencial,
  $$
  W=-\sum_{i<j}\frac{m_im_j}{\sqrt{r_{ij}^2+\varepsilon^2}},
  $$
  con el mismo suavizado $\varepsilon$ que la fuerza. Las calculan
  `energia_cinetica` y `potencial_directo`. La energía total $E=K+W$ se
  conserva, y su variación relativa controla la integración.
- **Cociente del virial, $2K/|W|$.** Para partículas que interactúan con
  fuerzas $1/r^2$, el teorema del virial dice que, si las posiciones y las
  velocidades se mantienen acotadas, los promedios temporales cumplen
  $2\langle K\rangle+\langle W\rangle=0$: el cociente vale uno. La esfera
  empieza en reposo, con $2K/|W|=0$; cómo evoluciona el cociente dice si el
  sistema llega a un equilibrio.

### Métodos

La suma directa cuesta $N^2$. Probamos dos alternativas de la tabla de la
clase 04:

- **cáscaras esféricas**: por el teorema de la cáscara esférica, cada
  partícula siente sólo la masa más cerca del centro que ella, como si
  estuviera toda en el centro. No hay que elegir un espesor: cada partícula
  hace de cáscara delgada de su propio radio, y la masa interior se cuenta
  partícula por partícula. Costo $N\log N$;
- **malla**: la masa se reparte en una grilla, el potencial se calcula con
  transformadas de Fourier y la fuerza se interpola a las partículas. Costo
  $N+N_c\log N_c$ para $N_c$ celdas.

## Cómo trabajar

Las preguntas de cada parte no se contestan sueltas: siguen el ciclo del
[paquete de afirmación](../../la-materia/03-entrega-tps.md).

- Lo marcado **Antes de correr** va a `question.md` antes de ejecutar el
  experimento, con fecha y commit: qué esperan, por qué, y qué resultado les
  haría aceptarlo o rechazarlo. Si después cambian de idea, agregan una sección
  nueva; no reescriben la anterior.
- Las demás preguntas se contestan en `lab.md`, con una entrada por
  experimento: qué esperaban, qué corrieron, qué vieron y cómo lo interpretan.
- Al final, `lab.md` cierra con la afirmación sobre la pregunta del práctico.

## Parte 1 — La referencia

1. Completen `esfera_fria` en `src/colapso/condiciones.py`: $N$ partículas de
   igual masa, distribuidas de manera uniforme en la esfera de radio 1 y en
   reposo. ¿Cómo sortean la dirección y el radio para que la densidad sea
   uniforme?
2. **Antes de correr:** ¿qué diferencia esperan entre la masa encerrada de
   $N$ partículas y la de la esfera continua, y cuál les haría rechazar el
   sorteo? Después completen `masa_encerrada` en `experiments/inicial.py` y
   córranlo con `uv run python experiments/inicial.py`. El script compara la
   fracción de la masa encerrada de sus partículas con la de la esfera
   continua.
3. **Antes de correr:** ¿hasta cuándo esperan que los radios lagrangianos
   sigan la solución exacta, y qué esperan del cociente $2K/|W|$ después del
   colapso? Después corran `uv run python experiments/directa.py` con 1000
   partículas.
4. En `lab.md`: ¿hasta cuándo siguen los radios lagrangianos la solución
   exacta? ¿A qué valor tiende $2K/|W|$? ¿Por qué no exactamente a uno? Miren
   qué pasa con el 90 % de la masa.

## Parte 2 — Cáscaras esféricas

1. Completen `aceleracion_capas` en `src/colapso/fuerzas.py`: con `np.argsort`
   y `np.cumsum` no hace falta ningún bucle.
2. **Antes de escribirlo**, justifiquen en `question.md` la tolerancia de
   `test_capas_reproducen_el_campo_de_la_esfera_uniforme` con un argumento de
   muestreo: ¿cuánto esperan que difiera una esfera de $N$ partículas de la
   continua? Después completen el test en `tests/test_fuerzas.py`. Una
   tolerancia elegida para que el test pase no es un control.

## Parte 3 — Malla

1. Completen `asignar_cic` y `test_cic_conserva_la_masa_y_reparte_por_volumen`.
2. **Antes de correr:** ¿qué método esperan que se acerque más al campo de la
   esfera continua: suma directa, cáscaras o malla? Después corran
   `uv run python experiments/malla.py`. El script compara la aceleración de
   cada método con el campo de la **esfera continua**, no con la suma directa.
   En `lab.md`: ¿por qué la suma directa tiene el error más grande? ¿Por qué
   hay un tamaño de celda óptimo y de qué depende?
3. **Antes de correr:** ¿qué esperan de las cáscaras y de la malla antes y
   después de $t_{ff}$? Después corran `uv run python experiments/capas.py`,
   que repite el colapso con cáscaras y con malla; la malla, por su costo,
   sólo hasta $1.5\,t_{ff}$. En `lab.md`: ¿qué método reproduce mejor qué
   parte del colapso? ¿Qué física deja afuera cada uno?

## Parte 4 — Auditoría

Las cáscaras son, por lejos, el método más barato. Un asistente de IA las usó
para una nube que **no** es esférica: un elipsoide uniforme frío, de semiejes
1, 0.9 y 0.8, apenas achatado. Produjo tres informes, en `auditoria/`, y los
tres concluyen que la aproximación vale. Audítenlos siguiendo
`auditoria/README.md`.

Cada informe es un experimento: escriban una entrada de `lab.md` por informe,
con el formato de
[auditar un informe](../../la-materia/03-entrega-tps.md#auditar-un-informe).
La entrada dice si la conclusión vale y, si no, **hasta cuándo y para qué
observable** vale la aproximación de cáscaras, con la evidencia de un control
independiente. Las herramientas de las partes 1 a 3 alcanzan; el README de la
auditoría lista las que se agregan para la nube y cuánto tarda cada corrida.

## Cierre — La afirmación

Con la evidencia de las partes 1 a 4, cierren `lab.md` con la afirmación sobre
la pregunta del práctico: **¿cuánto podemos aproximar la gravedad de muchos
cuerpos sin cambiar lo que medimos?** Para cada método: qué observable
reproduce, hasta cuándo, con qué evidencia y qué deja afuera. Usen la forma de
la materia,

> Con la evidencia disponible podemos afirmar **X**, siempre que se cumplan
> **A** y **B**. Todavía no sabemos **C** y el siguiente experimento debería
> distinguir **D** de **E**.

y agreguen las limitaciones y las explicaciones alternativas que siguen siendo
compatibles con los datos.

## Versionar y entregar

Un commit por cada cambio que deja los tests en verde, con un mensaje que diga
**por qué**. Se entrega el **jueves 12 de noviembre**, al comienzo de la
práctica de la clase 07, con un tag sobre el último commit. La entrega es el
paquete de afirmación: `question.md`, `lab.md` y `ai-use.md`, con lo que
delegaron a la IA y cómo lo verificaron. Ver
[Paquete de afirmación](../../la-materia/03-entrega-tps.md).

## Qué deberían poder explicar al terminar

- Qué aproximación hace cada método y qué observable deja de representar.
- Por qué la suma directa, que calcula exactamente la fuerza entre cada par de
  partículas, tampoco da el campo de la esfera continua, y qué le agrega que la
  esfera continua no tiene.
- Qué fija el suavizado y qué escala física deja de resolver.
- Por qué una aproximación que vale al principio puede dejar de valer durante
  la evolución, y cómo se detecta.

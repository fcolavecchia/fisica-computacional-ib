---
title: "Oscilador recargado"
subtitle: "Trabajo práctico 02"
---

Este práctico continúa la clase
[Orden, estabilidad y estructura](../../clases/02-1-integradores.qmd). Vuelve
sobre el oscilador del TP 01, ahora con un repositorio más grande que ya trae
Euler, RK4, el péndulo no lineal y herramientas de análisis. La pregunta es:
**¿cuándo creemos una simulación?**

::: {.callout-important}

## Repositorio

Este práctico es **individual**. La cátedra crea un repositorio privado para
cada estudiante, `fiscom-2026-02-oscilador-recargado-<usuario>`, y les envía
una invitación de GitHub. Acéptenla y clonen el repositorio desde ese enlace.
El equipo docente tiene acceso para seguir y revisar el trabajo; no creen una
copia desde un template.

:::

## Modelos

El repositorio integra sistemas de la forma $y'=f(t,y)$ con
$y=[\text{posición},\ \text{velocidad}]$. Trae dos modelos, en
`src/oscilador/modelos.py`:

- **Oscilador** lineal, amortiguado y forzado, por unidad de masa:
  $$x''+2\gamma x'+\omega_0^2x=F\cos(\Omega t).$$
- **Péndulo** simple sin aproximar:
  $$\theta''=-\frac{g}{\ell}\sin\theta .$$

## Parte 1 — Verlet

1. Completen `paso_verlet` en `src/oscilador/integradores.py`. Pueden adaptar
   la versión de la clase: acá el paso recibe $f(t,y)$ en lugar de $a(x)$.
2. Escriban los tres tests de `tests/test_verlet.py`: orden, energía a tiempos
   largos y reversibilidad. Elijan las tolerancias y justifíquenlas.

> Las propiedades geométricas que van a controlar —energía acotada y
> reversibilidad— corresponden al caso conservativo con aceleración $a(x)$:
> sin amortiguamiento ni forzamiento explícito en el tiempo. La interfaz es
> genérica, pero no todas las dinámicas que admite tienen esas propiedades.
3. Comprueben que cada test falla si introducen a propósito un error en
   `paso_verlet`. Un test que no detecta un Verlet mal implementado no está
   controlando Verlet.

## Parte 2 — Auditoría

Un asistente de IA produjo tres informes a partir de este repositorio. Los tres
corren, dan números razonables y tienen figuras prolijas. Audítenlos siguiendo
`auditoria/README.md` y registren cada auditoría en `lab.md`.

## Parte 3 — De la auditoría a los tests

1. Completen `test_orden_con_forzamiento` en `tests/test_rk4.py`.
2. Si algún control de la auditoría encontró un defecto en el código,
   conviértanlo en un test que falle con ese defecto y corríjanlo en un commit
   separado que deje el test en verde.

Si un test falla, averigüen por qué antes de tocar el código o la tolerancia.

> **Cambiar la tolerancia de un test para que pase no es arreglarlo.** Si
> deciden hacerlo, justifíquenlo en `lab.md` con evidencia.

## Parte 4 — Paquete de afirmación: el péndulo

**Pregunta:** ¿qué integrador conviene para estudiar un péndulo con amplitud
grande durante mil períodos?

No hay una fórmula cerrada para $\theta(t)$, pero sí para el período:
`Pendulo.periodo_exacto` lo calcula con la integral elíptica completa.

1. **Antes de correr nada**, completen `question.md`: la pregunta precisa
   (amplitud, tiempo de integración, qué significa "conviene"), el modelo y
   sus supuestos, las predicciones y los criterios de aceptación.
2. Escriban `experiments/pendulo.py`, que genere en `results/` todo lo que
   citen. Comparen los métodos también a igual costo, como en la clase.
3. Completen `lab.md` con las entradas del experimento, la afirmación vigente,
   las limitaciones y las explicaciones alternativas.
4. Documenten en `ai-use.md` qué delegaron a la IA y cómo lo verificaron.

Ver [Paquete de afirmación](../../la-materia/03-entrega-tps.md).

## Versionar y entregar

Un commit por cada cambio que deja los tests en verde, con un mensaje que diga
**por qué**. La forma de entrega está a definir.

## Qué deberían poder explicar al terminar

- Cómo comprobar que un integrador implementa el método que dice implementar.
- Qué observable sostiene cada afirmación de un informe, y cuál no alcanza.
- Cómo se distingue un problema de modelo, de método y de código.
- Cómo elegir un integrador para un problema sin solución exacta, con
  evidencia y a igual costo.

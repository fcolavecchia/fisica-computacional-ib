---
title: "Oscilador con los dos Euler"
subtitle: "Trabajo práctico 01"
---

Este práctico continúa la clase de [Euler implícito](../../clases/01-1-implicitos.qmd)
con una masa unida a un resorte ideal. Sirve para seguir practicando la mecánica
de trabajo de la materia, ahora implementando tres métodos:

1. Euler explícito;
2. Newton–Raphson para una ecuación escalar;
3. Euler implícito, que usa Newton–Raphson en cada paso.

La pregunta es: **¿qué evidencia necesitamos para confiar en una trayectoria
calculada durante muchos ciclos?**

::: {.callout-important}

## Repositorio

[Abrir el repositorio base](https://github.com/fisica-computacional-ib/fiscom-2026-01-oscilador){.btn .btn-primary}

En esta práctica cada estudiante trabajará individualmente en su
propio repositorio. Abran el repositorio base, elijan **Use this template** y
luego **Create a new repository**. Creen un repositorio **privado** en su cuenta
y clonen esa copia:

```bash
git clone https://github.com/<TU-USUARIO>/fiscom-2026-01-oscilador.git
cd fiscom-2026-01-oscilador
uv sync
```

> No clonen directamente el repositorio de la cátedra: no podrían subir allí sus cambios y compartirían el mismo `origin` con el resto del curso.

:::

## Modelo

Medimos el tiempo en unidades de $1/\omega$, con $\omega=\sqrt{k/m}$. Así,

$$
x''=-x,
\qquad
\frac{dx}{dt}=v,\quad \frac{dv}{dt}=-x,
$$

un período dura $2\pi$ y la energía por unidad de masa es $E=(x^2+v^2)/2$.

Los integradores reciben la aceleración como una función $a(x)$. Con el
resorte, $a(x)=-x$; con el péndulo, $a(\theta)=-\sin\theta$. En Euler
implícito, eliminar $v_{n+1}$ deja una sola ecuación para $z=x_{n+1}$,

$$
G(z)=z-x_n-dt\,v_n-dt^2\,a(z)=0,
$$

que se resuelve con Newton–Raphson partiendo de $z=x_n$.

## Consigna

Antes de correr nada, respondan las predicciones de `question.md`. Después
completen, en `src/oscilador/simulacion.py` y en este orden:

1. `integrar_euler_explicito`;
2. `newton_raphson`;
3. `integrar_euler_implicito`.

Después de cada función, corran los tests y hagan un commit. Al final:

```bash
uv run pytest
uv run python experiments/muchos_ciclos.py
uv run python experiments/convergencia.py
```

Interpreten las figuras generadas en `results/` y escriban una afirmación
breve, su evidencia y al menos una limitación en `lab.md`.

## Qué deberían poder explicar al terminar

- En qué se diferencian un paso de Euler explícito y uno implícito.
- Por qué el paso implícito exige resolver una ecuación, y cómo lo hace
  Newton–Raphson.
- Qué le hace cada Euler a la energía del oscilador, y por qué eso no
  contradice que ambos converjan.
- Por qué un error que tiende a cero con `dt` no alcanza para confiar en una
  trayectoria de muchos ciclos.

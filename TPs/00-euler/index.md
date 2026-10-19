---
title: "Euler y tiro oblicuo"
subtitle: "Actividad inicial"
---

Este ejercicio corto sirve para aprender la mecánica de trabajo de la materia
con un sistema conocido. El objetivo no es estudiar un proyectil en detalle,
sino recorrer una vez el ciclo completo:

1. escribir las ecuaciones de Newton como un sistema de primer orden;
2. aproximarlas con Euler explícito;
3. contrastar el resultado numérico con una solución conocida;
4. registrar qué se esperaba, qué se observó y qué limita la conclusión.

::: {.callout-important}

## Repositorio

[Abrir el repositorio base](https://github.com/fisica-computacional-ib/fiscom-2026-00-euler){.btn .btn-primary}

En esta práctica cada estudiante trabajará individualmente en su
propio repositorio. Abran el repositorio base, elijan **Use this template** y
luego **Create a new repository**. Creen un repositorio **privado** en su cuenta
y clonen esa copia:

```bash
git clone https://github.com/<TU-USUARIO>/fiscom-2026-00-euler.git
cd fiscom-2026-00-euler
uv sync
```

No clonen directamente el repositorio de la cátedra: no podrían subir allí sus
cambios y compartirían el mismo `origin` con el resto del curso.

:::

## Consigna

Completen `integrar_euler` en `src/tiro_oblicuo/simulacion.py`. Antes de correr
el experimento, respondan las predicciones de `question.md`. Después:

```bash
uv run pytest
uv run python experiments/trayectoria.py
```

Interpreten la figura generada en `results/trayectoria.png` y escriban una
afirmación breve, su evidencia y al menos una limitación en `lab.md`.

## Qué deberían poder explicar al terminar

- Cómo se pasa de $\vec F=m\vec a$ a cuatro ecuaciones de primer orden.
- Qué aproximación hace un paso de Euler explícito.
- Por qué comparar contra una solución exacta es un test físico y numérico.
- Por qué reducir `dt` mejora el resultado, pero no corrige los supuestos del
  modelo.

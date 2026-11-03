---
title: "Paquete de afirmación"
---

Lo que se entrega en cada trabajo práctico no es un notebook terminado. Es una
**afirmación computacional auditable**: una conclusión acotada, la evidencia
que la sostiene y todo lo necesario para que otra persona la regenere y la
cuestione.

## Estructura del repositorio

Los repositorios de trabajo de la materia tienen esta forma:

```text
README.md          cómo reproducir los resultados
question.md        pregunta, hipótesis y predicciones previas
src/               código reutilizable
tests/             evaluadores físicos y numéricos
experiments/       scripts que generan resultados
results/           resultados regenerables
lab.md             afirmaciones, evidencia y limitaciones
ai-use.md          delegaciones y verificaciones
pyproject.toml     dependencias y comandos
```

En la actividad inicial y el TP 01 cada estudiante crea una copia privada del
template. Desde el TP 02 la cátedra crea un repositorio privado por
estudiante o por grupo, según el práctico: los estudiantes tienen permiso para
trabajar y los docentes tienen acceso desde el comienzo. Cada integrante
acepta su invitación antes de clonarlo.

## `question.md`: antes de correr

Se escribe **antes** de ejecutar el experimento que contesta la pregunta. Si
después cambia, se agrega una sección nueva con fecha; no se reescribe la
anterior.

```markdown
## Pregunta

Una sola, contestable con los recursos de la materia.

## Modelo y supuestos

Qué representa el modelo, qué deja afuera, qué escalas importan.

## Predicciones

Qué esperamos ver y por qué, antes de mirar. Fecha y commit.

## Observables y criterios de aceptación

Qué vamos a medir y qué resultado nos haría aceptar o rechazar la hipótesis.
```

## `tests/`: evaluadores

Un test es un **evaluador ejecutable de una afirmación**. Puede ser una prueba
unitaria, pero en esta materia casi siempre es algo más físico:

- una ley de conservación;
- un orden de convergencia;
- una solución analítica o un régimen límite;
- un análisis dimensional;
- un residuo;
- la comparación entre dos métodos independientes;
- la recuperación de un parámetro conocido en datos sintéticos.

Cada test explica en su docstring **qué afirmación física falla si el test
falla**. Todos se corren con:

```bash
uv run pytest
```

## `experiments/` y `results/`: reproducibilidad

- Cada resultado de `results/` sale de un script de `experiments/` que se corre
  con `uv run python experiments/nombre.py`.
- Nada en `results/` se edita a mano.
- Los números aleatorios usan semillas fijas y declaradas.
- Un resultado que sólo existe después de ejecutar celdas de un notebook en un
  orden que nadie anotó **no es evidencia**. Los notebooks sirven para explorar
  y explicar.

## `lab.md`: el cuaderno

`lab.md` es un cuaderno de laboratorio: **cada experimento es una entrada**,
escrita el día que se corrió. Un experimento es cualquier corrida que pone algo
a prueba: un test, un script propio o la auditoría de un informe. Las entradas
no se reescriben: si una resulta equivocada, se agrega otra que la corrige.

```markdown
## 2026-10-22 — ¿Converge el ángulo al agrandar el dominio?

- **Esperábamos:** error proporcional a 1/R.
- **Criterio:** aceptamos si la pendiente queda entre −1.1 y −0.9.
- **Corrimos:** `uv run python experiments/convergencia_dominio.py` (commit a1b2c3d).
- **Vimos:** `results/convergencia_dominio.png`: pendiente −2, no −1.
- **Interpretación:** la condición inicial conserva el momento angular; el error
  que queda viene de la salida.
- **Qué sigue:** confirmar con un potencial de corto alcance.
```

**Esperábamos** y **Criterio** se escriben antes de correr.

### Auditar un informe

Auditar un informe también es un experimento. La entrada contesta tres
preguntas: **¿la conclusión vale?**, **si no vale, por qué** y **qué hay que
corregir para obtener un resultado físicamente correcto**.

```markdown
## AAAA-MM-DD — Informe X: pregunta del informe

- **Esperábamos:** si le creemos a la conclusión y qué tendría que ser cierto
  para que valga.
- **Criterio:** un control independiente que podría refutarla, y qué resultado
  la refutaría.
- **Corrimos:** comando y commit.
- **Vimos:** archivo de results/ y números.
- **Veredicto:** la conclusión vale, no vale o vale en parte. Si falla, la
  fuente (modelo, método o código) y por qué.
- **Corrección:** qué cambiamos y qué da al volver a correr (comando, commit,
  results/).
- **Afirmación:** la conclusión que sí sostiene la evidencia.
```

La corrección depende de la fuente: otras ecuaciones (modelo), otro integrador
u otro paso (método), un test que falla y el arreglo en commits separados
(código), o **la afirmación misma**, cuando los números están bien calculados
pero no sostienen lo que el informe concluye. "La conclusión vale" es un
veredicto posible si el control no la refuta.

### Afirmación vigente

Al final del cuaderno va la **afirmación vigente** sobre la pregunta de
`question.md`, con la forma de cierre de cada clase:

> Con la evidencia disponible podemos afirmar **X**, siempre que se cumplan
> **A** y **B**. Todavía no sabemos **C** y el siguiente experimento debería
> distinguir **D** de **E**.

Y dos listas que no pueden faltar: **limitaciones** y **explicaciones
alternativas que siguen siendo compatibles** con los datos.

## `ai-use.md`

Formato y reglas en [Trabajo con IA](02-trabajo-con-ia.md).

## Entrega

Una entrega es un **tag de git** sobre un commit que cumple dos condiciones:

- `uv run pytest` termina sin fallas inesperadas. Un test que documenta una
  falla conocida se marca con `@pytest.mark.xfail(reason="...")` y se explica
  en `lab.md`.
- Cada resultado citado en `lab.md` se regenera con su script.

Lo que no está en el tag no se revisa.

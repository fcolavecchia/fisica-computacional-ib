---
title: "Evaluación"
---

# Evaluación

La evaluación combina **productos grupales** y **comprensión individual**.
Premia la precisión de la pregunta, la calidad de los controles, la honestidad
sobre las incertidumbres y la capacidad de encontrar y corregir fallas.

No se califica ganar una competencia, producir la figura más atractiva ni
escribir más líneas de código. **Tampoco se penaliza haber usado IA** para una
parte sustancial del trabajo, si pueden verificarla, explicarla y adaptarla.

## Qué se evalúa

| Evidencia | Modalidad | Peso orientativo |
|---|---|---:|
| Trabajo práctico 1: colisiones | grupal | 20% |
| Trabajo práctico 2: Schrödinger | grupal | 20% |
| Trabajo práctico 3: fluidos | grupal | 20% |
| Trabajo práctico final | grupal, con componentes asignados | 20% |
| Defensas y perturbaciones | individual | 20% |

La nota de cada trabajo práctico combina el paquete entregado y la presentación
del grupo.

Las clases 1 y 2 son **formativas**: se pide completarlas, pero los problemas de
instalación o de git del comienzo no pesan en la nota.

## Rúbrica

Todos los trabajos prácticos se evalúan con la misma rúbrica. El código cuenta sólo
en la medida en que afecta la claridad, la corrección y la reproducibilidad.

### Modelo

¿La pregunta y los supuestos están definidos con precisión?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | La pregunta no se contesta con un cálculo, o los supuestos no aparecen. |
| Suficiente | Pregunta contestable y supuestos listados. |
| Bueno | Escalas y parámetros adimensionales identificados; se dice qué deja afuera el modelo. |
| Muy bueno | Los regímenes límite se anticipan y se usan para diseñar controles o experimentos. |

### Evidencia

¿Los experimentos realmente discriminan la afirmación?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | Figuras que "se ven bien"; ningún resultado posible habría refutado la afirmación. |
| Suficiente | Experimentos vinculados con la afirmación, con predicción registrada antes de correrlos. |
| Bueno | Al menos un experimento distingue la hipótesis de una alternativa concreta. |
| Muy bueno | Las alternativas se descartan con evidencia y la conclusión resiste las preguntas de la presentación. |

### Verificación

¿Hay controles físicos, numéricos y de implementación independientes?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | Tests que sólo prueban que el código corre. |
| Suficiente | Controles básicos: una conservación, un límite analítico. |
| Bueno | Controles independientes de los tres tipos; cada test dice qué afirmación cae si falla. |
| Muy bueno | Tests que nacieron de fallas reales encontradas y corregidas, con la falla documentada. |

### Incertidumbre

¿Se cuantifican sensibilidad, error y límites?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | Números sin error ni rango de validez. |
| Suficiente | Error numérico o estadístico estimado, según corresponda. |
| Bueno | Errores numérico y estadístico separados; sensibilidad a los parámetros relevantes. |
| Muy bueno | Límites de validez establecidos con evidencia: se muestra dónde deja de valer la conclusión. |

### Reproducibilidad

¿Otra persona puede regenerar los resultados?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | Los resultados no se regeneran desde el tag entregado. |
| Suficiente | `uv run pytest` pasa y los resultados principales salen de un comando. |
| Bueno | Todo resultado citado tiene script, semilla y commit. |
| Muy bueno | El script de la cátedra regenera todos los resultados citados desde el tag, sin intervención. |

### Comunicación

¿La conclusión distingue lo sabido de lo supuesto?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | La conclusión dice más que la evidencia. |
| Suficiente | Afirmación acotada, con sus condiciones. |
| Bueno | Separa lo sabido, lo supuesto y lo que falta; limitaciones explícitas. |
| Muy bueno | Lista alternativas compatibles con los datos y el experimento que las distinguiría; en la presentación responde las objeciones con evidencia. |

### Uso de IA

¿La delegación fue explícita y los resultados fueron verificados?

| Nivel | Qué se ve |
|---|---|
| Insuficiente | Sin registro, o resultados generados aceptados sin control. |
| Suficiente | Registro con delegaciones y verificaciones. |
| Bueno | Cada delegación relevante tiene un control independiente asociado. |
| Muy bueno | El registro documenta errores de la IA, cómo se detectaron y cómo se corrigieron. |

## Presentación del grupo

Quién presenta se sortea en el momento y **la nota es del grupo**. Se evalúa que
cualquiera de sus integrantes pueda explicar y defender la afirmación entregada:
la evidencia que la sostiene, sus límites y cómo se detectaron las fallas.
Detalle en [Grupos y presentaciones](grupos-y-revision.md).

## Defensa individual

Es la **única instancia de evaluación individual**. La defensa se hace **con IA disponible**, sobre el trabajo del grupo. La cátedra
introduce una **modificación no anticipada** del problema y se observa cómo
trabajan en tiempo real:

1. **Predicción:** qué esperan que cambie, antes de tocar nada.
2. **Delegación:** qué le piden a la herramienta y con qué criterio de
   aceptación.
3. **Verificación:** cómo comprueban que el resultado es correcto.
4. **Interpretación:** qué significa físicamente y dónde deja de valer.

Se evalúa el juicio científico, no la velocidad ni la sintaxis. Decir con
precisión qué no saben y cómo lo averiguarían cuenta a favor.

## A confirmar

- Régimen de aprobación y recuperatorios.
- Pesos definitivos.
- Cantidad y momento de las defensas; la clase 15 está reservada para esto.
- Peso de la presentación dentro de la nota de cada trabajo práctico.
- Duración de las presentaciones, y si van en la clase de cierre o al comienzo
  de la siguiente.
- Qué pasa si la persona sorteada está ausente.

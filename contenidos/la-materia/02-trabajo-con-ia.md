---
title: "Trabajo con IA"
---

En esta materia la IA no es una excepción tolerada ni un tema aparte. Las
actividades, las entregas y la evaluación están diseñadas **suponiendo que la
IA está disponible**. Por eso las consignas piden cosas que una respuesta
plausible no alcanza a cubrir: evidencia, controles independientes e
interpretación.

## Reglas

1. **El uso está permitido y se espera** durante todo el curso, incluidas las
   defensas.
2. **La responsabilidad es de ustedes.** Cada afirmación entregada es del grupo,
   la haya redactado quien la haya redactado. "Lo dijo el modelo" no es un
   argumento.
3. **El código generado es no confiable** hasta que supera controles físicos y
   numéricos independientes.
4. **Se declara lo relevante**: qué se delegó, qué se usó y cómo se verificó.
   No hace falta guardar conversaciones completas.
5. **Las referencias se verifican en la fuente original.** Un modelo puede
   inventar una cita, un teorema o una fórmula con total seguridad.
6. **No se suben a servicios externos** datos personales ni material que se les
   haya entregado como privado.
7. **Ninguna consigna obligatoria requiere una suscripción paga.** La cátedra
   asegura una herramienta común; pueden usar otras si quieren.

## Qué quiere decir "control independiente"

Un control es independiente cuando **no hereda los supuestos del código que
controla**. Pedirle al mismo modelo, en la misma conversación, el código y el
test que lo aprueba no es independiente: los dos pueden compartir el mismo
error.

Controles que sí suelen ser independientes:

| Control | Ejemplo |
|---|---|
| Solución analítica o régimen límite | Rutherford para Coulomb; oscilador armónico para Schrödinger |
| Ley de conservación | Energía y momento angular en una fuerza central |
| Orden de convergencia | El error de RK4 cae 16 veces al dividir el paso por 2 |
| Simetría | Paridad de los autoestados en un potencial simétrico |
| Método alternativo | Ángulo por integración de trayectorias contra ángulo por cuadratura |
| Análisis dimensional | Las unidades de cada término coinciden |
| Datos sintéticos | Recuperar un parámetro conocido antes de estimar uno desconocido |

## Hábitos que sirven con cualquier modelo

Las herramientas cambian cada pocos meses. Estos hábitos no:

- **Descomponer** el problema antes de pedir código.
- **Escribir el criterio de aceptación** antes de ver el resultado.
- **Pedir alternativas** y compararlas, en lugar de aceptar la primera.
- **Pedir que el modelo ejecute** el código y muestre la salida, no que la
  describa.
- **Pedir crítica**: "¿qué supuesto de este código fallaría si cambio X?".
- **Leer los diffs** antes de aceptarlos.
- **Desconfiar de lo que funciona a la primera**, sobre todo si coincide con lo
  que esperaban.

No se evalúa la longitud ni la sofisticación de los *prompts*.

## Registro de uso: `ai-use.md`

Cada repositorio de grupo tiene un `ai-use.md`. Una entrada por delegación
relevante, no por cada consulta:

| Fecha | ¿Qué se delegó? | ¿Qué se usó? | ¿Cómo se verificó? | ¿Qué falla se encontró? |
|---|---|---|---|---|
| 2026-10-20 | Implementación inicial del integrador | Estructura del código, con dos cambios | Orden de convergencia y conservación de energía | El evento de cruce se detectaba un paso tarde |
| 2026-10-22 | Derivación del ángulo por cuadratura | Nada: la fórmula tenía un factor 2 de más | Comparación con Rutherford | Integrando mal definido cerca del punto de retorno |

Una entrada honesta que dice "no sirvió" vale tanto como una que dice "funcionó".

## En la defensa

La defensa individual se hace **con IA disponible**. Se introduce una
modificación no anticipada y se evalúa cómo predicen, delegan, prueban e
interpretan en tiempo real. Ver [Evaluación](04-evaluacion.md).

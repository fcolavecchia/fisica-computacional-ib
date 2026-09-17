---
title: "Grupos y presentaciones"
---

## Grupos

- Los equipos son de **dos o tres integrantes** y se mantienen durante la
  cursada, salvo que haya una razón para reorganizarlos.
- Cada integrante participa de todas las dimensiones del trabajo: formular el
  modelo, programar, verificar, registrar e interpretar. Pueden repartirse el
  trabajo de cada semana, pero no hay "dueños" permanentes de una parte del
  práctico.
- Conviene acordar al comienzo de cada bloque quién impulsa cada aspecto y
  dejarlo asentado en `lab.md`. En un equipo pequeño una misma persona puede
  asumir más de un aspecto; el objetivo es que el equipo sepa qué está haciendo
  y por qué.

| Rol | Responsabilidad |
|---|---|
| Aspecto | Qué hay que cuidar |
|---|---|
| **Modelo** | Explicitar supuestos, escalas y predicciones en `question.md` |
| **Cómputo** | Organizar el código y el uso de las herramientas |
| **Verificación** | Diseñar tests y buscar contraejemplos |
| **Registro** | Mantener `lab.md`, `ai-use.md` y la reproducibilidad |

**La verificación nunca queda vacante.** Si una persona no está, el resto del
equipo se hace cargo de que las pruebas y sus resultados queden registrados.

## La autonomía crece en el juicio, no en el código

| | Práctico 1 | Práctico 2 | Práctico 3 | Final |
|---|---|---|---|---|
| Pregunta | dada | dada | dada | extensión propia |
| Código de partida | plantilla con huecos acotados | plantilla con huecos acotados | solver completo | plantilla con huecos acotados |
| Evaluadores | lista de la cátedra | parte de la lista | los proponen | los proponen |
| Interpretación | guiada | parcialmente guiada | libre | libre |

## Entrega

Cada trabajo práctico se entrega **a la cátedra** como un tag de git. La
cátedra lo revisa en dos pasos:

1. **Un script** clona el tag, instala el entorno, corre los tests y regenera
   los resultados citados en `lab.md`.
2. **Lectura docente** con la [rúbrica](04-evaluacion.md).

## Sprints de investigación

En todas las clases, algunos equipos compartirán brevemente dónde están con su
investigación. Son **sprints de investigación**: una pausa para hacer visible el
proceso, recibir una pregunta útil y volver al trabajo con el próximo paso más
claro. No son una exposición preparada ni una evaluación oral.

- La cátedra indica qué equipos hacen sprint ese día y elige **al azar** a uno
  de sus integrantes para contarlo. Así, todos seguimos de cerca el trabajo
  común y cualquiera puede ponerse al día si hace falta.
- Cada sprint dura pocos minutos y puede apoyarse en una figura, una corrida, un
  test o el cuaderno de laboratorio; no hace falta preparar diapositivas.
- La persona que habla representa al equipo. Las preguntas apuntan a entender el
  estado de la investigación y a destrabar el siguiente paso, no a tomar una
  lección individual.

En un sprint conviene contar, de manera simple:

1. Qué pregunta están tratando de responder y qué hicieron desde la clase
   anterior.
2. Qué resultado, decisión o problema es hoy el más importante.
3. Cómo lo verificaron —o cómo planean verificarlo—.
4. Qué van a hacer después y qué ayuda o decisión necesitan del equipo docente.

Al final de cada práctico, el equipo deja en el repositorio una afirmación
respaldada por evidencia, sus límites y el registro del proceso. Los sprints
sirven para llegar a esa entrega con mejores preguntas y mejores pruebas.

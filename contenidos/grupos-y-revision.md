---
title: "Grupos y presentaciones"
---

# Grupos y presentaciones

## Grupos

- **Cuatro integrantes**, fijos durante la cursada.
- **Roles que rotan** en cada trabajo práctico. Con cuatro trabajos prácticos, cada
  persona pasa por los cuatro roles.
- Los roles **no son tareas exclusivas**: sirven para que todos practiquen todas
  las dimensiones del trabajo.

| Rol | Responsabilidad |
|---|---|
| **Modelo** | Explicita supuestos, escalas y predicciones en `question.md` |
| **Cómputo** | Conduce la interacción con las herramientas y organiza el código |
| **Verificación** | Diseña tests y busca contraejemplos |
| **Registro** | Mantiene `lab.md`, `ai-use.md` y la reproducibilidad |

**El rol de verificación nunca queda vacante.** Si falta quien lo tiene, otra
persona lo asume ese día y se anota en `lab.md`.

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
2. **Lectura docente** con la [rúbrica](evaluacion.md).

## Presentación

Al cerrar cada práctico, cada grupo presenta su trabajo en clase.

- **Quién presenta se sortea en el momento**, entre quienes todavía no
  presentaron. A lo largo de la cursada, cada estudiante presenta una vez.
- **La nota es del grupo**, no de quien presenta. Cualquiera de los cuatro
  tiene que poder explicar y defender todo lo entregado.
- **La cátedra pregunta para poner a prueba la afirmación**, no para repasar el
  código.

Qué se presenta:

1. **La afirmación**, con la forma de cierre de las clases.
2. **El experimento que más discriminó** entre la hipótesis y una alternativa.
3. **Una falla encontrada** en el modelo, el método, el código o lo que produjo
   la IA, y cómo se detectó.
4. **Los límites:** dónde deja de valer la conclusión.

---
title: "Organización"
subtitle: "Física Computacional 2026"
---

Instituto Balseiro

## Cátedra

- Flavio Colavecchia (flavio.colavecchia@ib.edu.ar)
- Lautaro Saba (lautaro.saba@ib.edu.ar)
- Juan Camilo Zapata Ceballos (camilo.zapata@ib.edu.ar)

> Este curso se basa en el trabajo de P. Cornaglia y otros docentes de la
> materia a lo largo de los años.

## Para qué es esta materia

La materia busca que puedan **producir conocimiento computacional** sobre un
problema físico con las herramientas de hoy, incluidos los modelos de lenguaje
y los agentes de programación.

Al terminar, se espera que puedan hacer esto:

> Dada una pregunta física y acceso a modelos de lenguaje, bibliografía, código
> y cómputo, formular un modelo, diseñar experimentos, producir evidencia
> reproducible y explicar qué parte de la conclusión está justificada.

Hoy un modelo de lenguaje escribe en minutos un integrador, un solver o un
Monte Carlo que parece correcto. Por eso el cuello de botella ya no es escribir
la primera versión del código. Lo difícil es otra cosa:

- formular con precisión la pregunta;
- explicitar supuestos y escalas;
- decidir qué cálculo constituye evidencia;
- diseñar controles independientes de la solución generada;
- reconocer resultados plausibles pero incorrectos;
- distinguir una falla del modelo, del método numérico o del código;
- estimar incertidumbres y límites de validez;
- hacerse responsable de lo que se entrega.

Eso es lo que se practica y se evalúa.

## Qué significa que la IA esté integrada

- **Pueden y se espera que usen IA** en clase, en las entregas y en las
  defensas.
- **No se evalúa** escribir código desde cero ni memorizar sintaxis.
- **Sí se evalúa** especificar, delegar, inspeccionar, probar, interpretar y
  defender.
- **El código generado no es confiable** hasta que supera controles físicos y
  numéricos independientes.
- **Se registra** qué se delegó y cómo se verificó.

Las reglas completas están en [Trabajo con IA](02-trabajo-con-ia.md).

## Cómo es una clase

Las clases son talleres de cuatro horas. La teoría aparece en bloques breves,
cuando hace falta para tomar una decisión.

| Momento                | Duración | Qué se hace                                                                           |
| ---------------------- | -------: | ------------------------------------------------------------------------------------- |
| Pregunta y predicción  |   25 min | Se presenta el fenómeno y cada grupo anota qué espera, **antes** de consultar a la IA |
| Teoría inicial         |   35 min | Lo necesario para formular el primer experimento                                      |
| Especificación         |   20 min | Hipótesis, observables y criterios de aceptación                                      |
| Construcción           |   70 min | Trabajo en grupo asistido por IA                                                      |
| Pausa                  |   15 min | —                                                                                     |
| Verificación y crítica |   45 min | Tests, comparaciones, ataque a los propios resultados                                 |
| Síntesis               |   30 min | Commit reproducible y conclusión provisional                                          |

Cada clase termina con una afirmación de esta forma:

> Con la evidencia disponible podemos afirmar **X**, siempre que se cumplan
> **A** y **B**. Todavía no sabemos **C** y el siguiente experimento debería
> distinguir **D** de **E**.

## Recorrido

| Clase | Pregunta o actividad                                    | Producto                              |
| ----: | ------------------------------------------------------- | ------------------------------------- |
|     1 | ¿Cuándo creemos una simulación?                         | Diagnóstico, no evaluado              |
|     2 | Leer, modificar, probar y versionar un oscilador con IA | Primer paquete de afirmación          |
|     3 | Formular una colisión clásica y recuperar Rutherford    | Modelo y predicciones                 |
|     4 | ¿Podemos confiar en las trayectorias?                   | Suite de evaluadores                  |
|     5 | ¿Qué potencial produjo estos ángulos?                   | Hipótesis discriminantes              |
|     6 | Caja negra y cierre del práctico                        | **Trabajo práctico 1: colisiones**    |
|     7 | Construir un Hamiltoniano discretizado                  | Solver cuántico inicial               |
|     8 | ¿Los estados calculados son físicos?                    | Validación del solver                 |
|     9 | ¿Qué potencial produjo este espectro?                   | **Trabajo práctico 2: cuántica**      |
|    10 | ¿Qué debe respetar una simulación de fluidos?           | Ejecución del solver dado             |
|    11 | El vórtice de Taylor–Green como referencia              | Auditoría del solver                  |
|    12 | ¿La disipación es física o numérica?                    | **Trabajo práctico 3: fluidos**       |
|    13 | ¿Qué significa una barra de error Monte Carlo?          | Monte Carlo variacional del hidrógeno |
|    14 | Extender una conclusión y ponerla a prueba              | **Trabajo práctico final**            |
|    15 | Defensa con IA y síntesis                               | Evaluación individual                 |

Las fechas se confirman al comenzar la cursada. El material de cada
trabajo práctico está en [`TPs/`](../TPs/README.md).

## Cómo se trabaja

- **Grupos de cuatro** con roles que rotan en cada trabajo práctico. Ver
  [Grupos y presentaciones](05-grupos-y-revision.md).
- **La unidad de trabajo es una afirmación auditable**, no un notebook
  terminado. Ver [Paquete de afirmación](03-entrega-tps.md).
- **Cada trabajo práctico se entrega a la cátedra y se presenta en clase.** Quien
  presenta sale sorteado y la nota es del grupo.
- **Todo resultado entregado se puede regenerar** con un comando desde el
  repositorio.

## Evaluación

La nota combina cuatro trabajos prácticos grupales y una defensa individual. No se
califica la figura más atractiva, la cantidad de código ni ganar una
competencia. Tampoco se penaliza haber usado IA si pueden verificar, explicar y
adaptar lo que entregan. Detalle y rúbrica en [Evaluación](04-evaluacion.md).

## Herramientas

| Herramienta               | Para qué                                                  |
| ------------------------- | --------------------------------------------------------- |
| Terminal                  | Navegar y ejecutar comandos preparados                    |
| `uv`                      | Instalar Python y las bibliotecas de cada proyecto        |
| Python, NumPy, Matplotlib | Leer y modificar funciones, trabajar con arrays, graficar |
| SciPy                     | Integración, álgebra dispersa, ajustes                    |
| `pytest`                  | Ejecutar y escribir evaluadores                           |
| Git y GitHub              | Versionar, compartir y entregar                           |
| Asistente de IA           | Explorar, editar, ejecutar y criticar                     |

Todo lo obligatorio corre en una laptop sin GPU. La cátedra asegura el acceso a
una herramienta de IA común. Instrucciones en [Instalación](06-instalacion.md).

## Material de consulta

Los notebooks de años anteriores son una **biblioteca**: se consultan cuando
una derivación, un ejemplo o un método desbloquea un trabajo práctico. No son la
secuencia de la materia.

## Bibliografía

- G. Abramson, [_Notas de Física Computacional_](https://drive.google.com/file/d/1Zv_vW3wvVkFlZEIgmR6r08vysiWJYUvv/view?usp=sharing)
  (2022), Instituto Balseiro. Buena introducción en castellano a varios temas.
- W. H. Press _et al._, _Numerical Recipes_, 3.ª ed., Cambridge University
  Press, 2007.
- J. Thijssen, _Computational Physics_, 2.ª ed., Cambridge University Press, 2007.
- W. Krauth, _Statistical Mechanics: Algorithms and Computations_, Oxford
  University Press, 2006.
- R. H. Landau, M. J. Páez y C. C. Bordeianu, _Computational Physics: Problem
  Solving with Python_, Wiley, 2015.
- S. E. Koonin y D. C. Meredith, _Computational Physics_, Addison-Wesley, 1990.
- D. C. Rapaport, _The Art of Molecular Dynamics Simulation_, Cambridge
  University Press, 2004.

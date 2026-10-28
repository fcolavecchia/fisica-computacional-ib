---
title: "Dispersión"
subtitle: "Trabajo práctico 03"
---

Este práctico continúa la clase
[Colisiones y dispersión de Rutherford](../../clases/03-1-colisiones.qmd). Usa
un repositorio que ya integra trayectorias en potenciales centrales y estima
secciones eficaces. La pregunta es: **¿cuándo confiamos en un cálculo de
dispersión?**

::: {.callout-important}

## Repositorio

La cátedra les enviará el enlace a su repositorio privado de grupo. Acepten la
invitación de GitHub y clónenlo desde ese enlace. Todas las personas del grupo
y el equipo docente trabajan sobre el mismo repositorio; no creen una copia
desde un template.

:::

## El problema

Una partícula de masa reducida $\mu=1$ llega desde lejos con energía $E$ y
parámetro de impacto $b$, y se desvía un ángulo $\chi(b)$ en un potencial
central $V(r)$. El repositorio trae cuatro potenciales, en
`src/dispersion/potenciales.py`:

| Potencial | $V(r)$ | Alcance |
|---|---|---|
| Coulomb | $k/r$ | largo |
| Yukawa | $k\,e^{-r/a}/r$ | $a$ |
| Esfera blanda | $\varepsilon(\sigma/r)^{12}$ | $\sigma$ |
| Lennard-Jones | $4\varepsilon[(\sigma/r)^{12}-(\sigma/r)^6]$ | $\sigma$, con un pozo |

El ángulo se puede calcular de dos maneras independientes: integrando la
trayectoria (`trayectoria.py`) o con la cuadratura de la órbita
(`deflexion.py`),

$$
\chi(b)=\pi-2b\int_{r_{\min}}^{\infty}\frac{dr}{r^2\sqrt{1-V(r)/E-b^2/r^2}} .
$$

## Datos de cuentas

En los CSV, `luminosidad` es un factor de normalización experimental conocido
que convierte una sección eficaz en cuentas esperadas:

$$
N=L\,\sigma.
$$

En este TP se toma como dato; no es un parámetro a ajustar.

## Parte 1 — Cuadratura

1. Completen `angulo_cuadratura` en `src/dispersion/deflexion.py`. El
   integrando diverge en el punto de retorno $r_{\min}$, que ya calcula
   `punto_de_retorno`: decidan cómo tratar esa singularidad.
2. Escriban los dos tests de `tests/test_deflexion.py` que faltan: contra
   Rutherford y contra la trayectoria. Elijan las tolerancias y justifíquenlas.
3. Comprueben que cada test falla si introducen a propósito un error en
   `angulo_cuadratura`, por ejemplo un factor 2 o un límite corrido.

## Parte 2 — Auditoría

Un asistente de IA produjo cuatro informes a partir de este repositorio. Los
cuatro corren, dan números razonables y tienen figuras prolijas. Audítenlos
siguiendo `auditoria/README.md` y registren cada auditoría en `lab.md`.

## Parte 3 — De la auditoría a los tests

1. Completen `test_histograma_contra_una_seccion_eficaz_conocida` en
   `tests/test_seccion_eficaz.py`.
2. Si algún control de la auditoría encontró un defecto en el código,
   conviértanlo en un test que falle con ese defecto y corríjanlo en un commit
   separado que deje el test en verde.

> **Cambiar la tolerancia de un test para que pase no es arreglarlo.** Si
> deciden hacerlo, justifíquenlo en `lab.md` con evidencia.

## Versionar y entregar

Un commit por cada cambio que deja los tests en verde, con un mensaje que diga
**por qué**. La forma de entrega está a definir.

## Qué deberían poder explicar al terminar

- Cómo comprobar que dos métodos independientes calculan el mismo ángulo.
- Qué observable sostiene cada afirmación de un informe, y cuál no alcanza.
- Cómo se distingue un problema de modelo, de método y de código.

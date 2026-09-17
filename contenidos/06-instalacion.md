---
title: "Instalación"
---

Todo lo obligatorio corre en una laptop, sin GPU, con Windows, macOS o Linux.
Hacen falta tres cosas: una terminal, `uv` y `git`.

## 1. Abrir una terminal

- **Windows:** PowerShell.
- **macOS:** Terminal.
- **Linux:** la terminal de la distribución.

Si usan Visual Studio Code, sirve la terminal integrada: **Terminal → New
Terminal**.

## 2. Instalar `uv`

`uv` instala la versión de Python y las bibliotecas que necesita cada proyecto.
Primero comprueben si ya está:

```bash
uv --version
```

Si no aparece un número de versión, instálenlo.

En macOS o Linux:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

En Windows, desde PowerShell:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Son las instrucciones oficiales de
[uv](https://docs.astral.sh/uv/getting-started/installation/). Al terminar,
**cierren y vuelvan a abrir la terminal** y repitan `uv --version`.

No hace falta instalar Python aparte ni activar entornos: todos los comandos de
la materia empiezan con `uv run`.

## 3. Instalar y configurar `git`

```bash
git --version
```

Si no está instalado:

- **Windows:** [git-scm.com/download/win](https://git-scm.com/download/win).
- **macOS:** `xcode-select --install`.
- **Linux:** el gestor de paquetes, por ejemplo `sudo apt install git`.

Después, una sola vez, con su nombre y su correo:

```bash
git config --global user.name "Nombre Apellido"
git config --global user.email "correo@ib.edu.ar"
```

## 4. Comprobar que todo funciona

Dentro del repositorio de la primera clase:

```bash
uv sync
uv run python -m oscilador.chequeo
```

La primera ejecución tarda unos minutos porque descarga Python y las
bibliotecas. El chequeo informa qué encontró y genera
`results/chequeo.png`. Si termina con `Entorno listo`, está todo.

## Si algo no funciona

### `uv` no se reconoce

Cierren la terminal y abran una nueva. Si sigue igual, reinstalen `uv` y copien
el mensaje completo de error.

### No aparece `pyproject.toml`

La terminal está en otra carpeta. Con `ls` (macOS, Linux) o `dir` (Windows)
comprueben que ven `README.md` y `pyproject.toml`. Si no, cambien de carpeta
con `cd`.

### `uv sync` falla al descargar

Casi siempre es la red o un proxy. Prueben otra red y guarden el mensaje de
error completo.

### Un test termina con `NotImplementedError`

No es un problema de instalación: es un hueco de la plantilla que les toca
completar.

### Nada de lo anterior

Escriban a la cátedra **antes de la clase**, con el sistema operativo, el
comando que corrieron y el mensaje completo. Mientras se resuelve, trabajen en
la computadora de otra persona del grupo: nadie pierde una clase por la
instalación.

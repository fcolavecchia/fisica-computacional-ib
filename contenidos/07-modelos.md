---
title: "Modelos de IA en VS Code"
---

En la materia pueden usar un asistente de IA para leer el repositorio, proponer
cambios y ayudar a interpretar errores. Todos los programas y pruebas también se
pueden ejecutar a mano: el asistente es una herramienta de trabajo, no un
requisito para que el código funcione.

Hay dos alternativas gratuitas. Para estudiantes, recomendamos **GitHub Copilot
Student**. **Codex con ChatGPT Free** sirve como alternativa, aunque su cuota es
más limitada.

## Opción recomendada: GitHub Copilot Student

GitHub Copilot funciona dentro de VS Code. En modo agente puede buscar y leer
archivos del repositorio, modificarlos, proponer comandos de terminal y revisar
el resultado de las pruebas. Los cambios siempre deben ser revisados por quien
los entrega.

### 1. Crear o preparar la cuenta de GitHub

Si ya tienen una cuenta personal de GitHub, no creen otra: agreguen y verifiquen
su dirección `@ib.edu.ar` en
[Settings → Emails](https://github.com/settings/emails).

Si todavía no tienen cuenta:

1. Entren en [github.com/signup](https://github.com/signup).
2. Creen una cuenta personal. Pueden registrarse con la dirección institucional
   o agregarla después.
3. Verifiquen la dirección de correo desde el mensaje que envía GitHub.
4. Activen autenticación en dos pasos y guarden los códigos de recuperación.

El beneficio estudiantil queda asociado a la cuenta personal de GitHub, no a
una cuenta separada del Instituto.

### 2. Solicitar GitHub Education

1. Abran [GitHub Education Benefits](https://github.com/settings/education/benefits).
2. Seleccionen **Start an application**.
3. Indiquen que son estudiantes y seleccionen el Instituto Balseiro.
4. Usen la dirección `@ib.edu.ar` cuando sea solicitada.
5. Si GitHub pide documentación adicional, adjunten una constancia vigente que
   muestre nombre, institución y fecha. No suban documentación al repositorio.
6. Envíen la solicitud y esperen la aprobación. Puede demorar varios días.

Si el dominio o la institución no aparecen, sigan el enlace de soporte que
muestra el formulario. La aprobación la realiza GitHub, no la cátedra.

### 3. Activar Copilot Student

La aprobación de GitHub Education y la activación de Copilot son pasos
separados:

1. Vuelvan a [GitHub Education Benefits](https://github.com/settings/education/benefits).
2. Busquen los recursos gratuitos para estudiantes y activen **Copilot Student**.
3. Comprueben en la configuración de Copilot que el beneficio está activo. La
   interfaz puede mostrarlo como **Copilot Pro** aunque el acceso provenga del
   beneficio estudiantil.
4. Mantengan desactivado el uso adicional pago o dejen su presupuesto en
   `USD 0` si no quieren cargos fuera de la cuota incluida.

Si sólo aparecen opciones pagas inmediatamente después de la aprobación, no
completen una compra: esperen unos días y vuelvan a intentar la activación.

### 4. Instalar Copilot en VS Code

1. Instalen [Visual Studio Code](https://code.visualstudio.com/) si todavía no lo
   tienen.
2. Abran **Extensions** en VS Code.
3. Busquen **GitHub Copilot**, publicado por GitHub, e instálenlo.
4. Inicien sesión con la misma cuenta de GitHub que tiene activo el beneficio.
5. Abran el repositorio de la materia y luego el panel **Chat**.
6. Seleccionen el modo **Agent** para permitir que Copilot trabaje con varios
   archivos. Mantengan los permisos manuales para revisar los comandos antes de
   ejecutarlos.

El nombre que aparece junto a una tarea, por ejemplo `GPT-6 Luna`, identifica el
modelo elegido por Copilot. No significa que se esté usando una suscripción de
ChatGPT. Para comprobar el origen del uso, abran el panel de estado de Copilot:
la tarea debe descontar créditos de la cuota de GitHub.

Documentación oficial: [Copilot gratuito para estudiantes](https://docs.github.com/en/copilot/how-tos/copilot-on-github/set-up-copilot/enable-copilot/set-up-for-students).

## Alternativa: Codex con ChatGPT Free

Codex también puede trabajar como agente dentro de VS Code: lee y modifica
archivos y puede proponer o ejecutar comandos. Actualmente está incluido en el
plan gratuito de ChatGPT, con límites de uso menores que los planes pagos. No
hace falta crear una clave de API ni activar facturación.

### 1. Crear una cuenta gratuita de ChatGPT

1. Entren en [chatgpt.com](https://chatgpt.com/).
2. Creen una cuenta o inicien sesión en una existente.
3. No es necesario contratar ChatGPT Plus ni agregar un medio de pago.

### 2. Instalar Codex en VS Code

1. Abran **Extensions** en VS Code.
2. Busquen **Codex – OpenAI's coding agent** y comprueben que el publicador sea
   **OpenAI**.
3. Instalen la extensión y abran su panel lateral.
4. Seleccionen **Sign in with ChatGPT** y autoricen la cuenta gratuita en el
   navegador.
5. Abran la carpeta del repositorio antes de iniciar una tarea.

No seleccionen la opción de clave de API: una clave usa la facturación separada
de la API y no la cuota gratuita de ChatGPT.

La disponibilidad y los límites pueden cambiar. Se pueden consultar desde el
panel de uso o mediante `/status` en una sesión de Codex. Documentación oficial:
[precios y disponibilidad de Codex](https://learn.chatgpt.com/docs/pricing).

## Primer uso

Antes de pedir cambios, hagan una consulta de reconocimiento, por ejemplo:

> Leé `AGENTS.md` y el `README` del repositorio. Explicame cómo ejecutar los
> códigos y las pruebas, pero no modifiques archivos ni ejecutes comandos.

Después pueden pedir una tarea concreta. Revisen siempre el diff y sepan repetir
manualmente los comandos importantes. El uso del asistente debe registrarse como
se explica en [Trabajo con IA](02-trabajo-con-ia.md).

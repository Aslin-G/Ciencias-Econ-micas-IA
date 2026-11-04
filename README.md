# IA Lab · Evaluación de inteligencia artificial

Evaluación interactiva, gamificada y tipo ICFES sobre el uso de herramientas de inteligencia artificial generativa (ChatGPT, Claude, Gemini, DeepSeek, agentes y más), con ejemplos prácticos de analítica para la toma de decisiones y de turismo.

**Aplicación publicada:** https://aslin-g.github.io/Ciencias-Econ-micas-IA/

## Qué incluye

- **8 módulos · 24 preguntas:** fundamentos (tokens, contexto, temperatura), el arte del prompt, la herramienta correcta, decidir con datos, IA en acción para el turismo, agentes con aprobación humana, privacidad y datos, y verificación.
- **Preguntas interactivas:** arrastrar y relacionar, arrastrar y clasificar, completar el prompt, ordenar pasos, verdadero o falso y selección única con situación (estilo ICFES). En el celular se responde tocando la tarjeta y luego su lugar.
- **Sin límite de tiempo.** El estudiante entra con su nombre completo y su correo y puede salir y volver para continuar.
- **Al entregar:** nota sobre 5.0, medalla, porcentaje por módulo y revisión de cada pregunta con la respuesta correcta y su explicación.
- **Gamificación:** XP, niveles y logros (no cambian la nota).
- **Repasa conceptos:** tokenizador, temperatura, ventana de contexto, analizador de prompts y glosario.
- **Nova:** tutora con IA (DeepSeek) que explica conceptos sin responder la evaluación.
- **Registro en Google Sheets:** cada interacción, el avance, el resultado y cada respuesta, identificados por correo y nombre.

Sin configurar nada, la aplicación funciona de inmediato en modo local: guarda el avance en el navegador del estudiante y Nova responde con una guía local, sin IA en línea.

## Estructura del repositorio

| Archivo | Para qué sirve |
| --- | --- |
| `index.html` | La aplicación completa en un solo archivo (es lo que publica GitHub Pages). |
| `apps-script/Code.gs` | El código de Google Apps Script que guarda todo en la hoja de cálculo y conecta el chat con DeepSeek. |
| `.nojekyll` | Indica a GitHub Pages que publique los archivos tal como están. |

## 1. Publicar con GitHub Pages

1. En el repositorio, entra a **Settings → Pages**.
2. En **Build and deployment**, elige **Deploy from a branch**, rama **main** y carpeta **/ (root)**. Pulsa **Save**.
3. En uno o dos minutos la aplicación queda en https://aslin-g.github.io/Ciencias-Econ-micas-IA/

## 2. Conectar la hoja de cálculo y el chat (opcional, recomendado)

1. Crea una hoja de cálculo en Google Drive y ábrela.
2. Entra a **Extensiones → Apps Script**, borra lo que haya y pega todo el contenido de `apps-script/Code.gs`. Guarda.
3. Ve a **Configuración del proyecto (⚙️) → Propiedades de la secuencia de comandos** y agrega:
   - `DEEPSEEK_API_KEY` = tu clave de DeepSeek (https://platform.deepseek.com/api_keys).
   - Opcionales: `DEEPSEEK_MODEL` (por defecto `deepseek-flash`), `CHAT_LIMIT` (mensajes por estudiante por hora; por defecto 40) y `COURSE_CODE` (un código de tu curso).
4. En el editor, elige la función **`setup`** y pulsa **Ejecutar**. Acepta los permisos (si aparece «Google no ha verificado esta app»: *Configuración avanzada → Ir al proyecto*). Se crean las hojas y el panel.
5. Elige **`probarDeepSeek`** y ejecútala: en el registro de ejecución debe aparecer `"ok": true`.
6. Pulsa **Implementar → Nueva implementación → Aplicación web**, con «Ejecutar como: **Yo**» y «Quién tiene acceso: **Cualquier usuario**». Copia la URL que termina en **`/exec`**.
7. En GitHub abre `index.html`, pulsa el lápiz ✏️ para editarlo, busca `apiUrl: ''` (cerca de la línea 1007) y pega la URL entre las comillas:

   ```js
   apiUrl: 'https://script.google.com/macros/s/.../exec',
   ```

   Si definiste `COURSE_CODE`, escribe el mismo valor en `courseCode`. Pulsa **Commit changes**; GitHub Pages se actualiza solo.
8. Prueba con tu propio correo: en la barra de la evaluación debe verse «● Sincronizado con la hoja».

Si más adelante cambias el script, publica una versión nueva desde **Implementar → Gestionar implementaciones → editar → Nueva versión**; así la URL no cambia.

### Hojas que se crean

- **Panel:** estudiantes, entregas, nota promedio, aprobados, promedio por módulo y análisis por pregunta (de la más difícil a la más fácil).
- **Estudiantes:** una fila por correo, con estado, nota, XP y logros.
- **Interacciones:** cada acción del estudiante (respuestas, módulos abiertos, conceptos, logros…).
- **Progreso:** el avance guardado, para continuar en otro equipo.
- **Resultados:** nota, logro y porcentaje por módulo de cada entrega.
- **Respuestas:** una fila por pregunta y estudiante, con su respuesta, la correcta y el puntaje.
- **Chat:** las conversaciones con Nova y los tokens usados.

### Permitir que un estudiante presente de nuevo

En Apps Script, abre la función `permitirNuevoIntento`, cambia el correo y ejecútala.

## 3. Ajustes en `index.html`

En el bloque `CONFIG`, al inicio del código:

- `chatInExam`: `true` para que Nova esté disponible durante la evaluación; `false` para apagarla.
- `showAnswersAfterSubmit`: `true` para mostrar las respuestas correctas y las explicaciones al entregar; `false` para mostrar solo la nota.

## Importante

- **Nunca pegues la clave de DeepSeek en `index.html`:** el repositorio es público y cualquiera podría usarla. La clave va solo en las propiedades del script.
- **Las respuestas correctas están dentro de `index.html`.** Alguien con conocimientos técnicos podría encontrarlas en el código; tenlo en cuenta si la evaluación tiene un peso alto en la nota.
- **La URL del script es pública:** cualquiera que la tenga podría enviar registros o usar el chat dentro del límite por hora. `COURSE_CODE` lo dificulta, pero ese código también queda visible en `index.html`.
- **Un intento por correo:** con la hoja conectada, el límite se controla desde la hoja. Sin conexión, solo se controla en el navegador del estudiante.
- **Datos de las herramientas:** los nombres y versiones provienen de las presentaciones del curso (octubre de 2026). Verifícalos antes de usarlos, porque cambian con frecuencia.

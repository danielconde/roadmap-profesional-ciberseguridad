# Semana 6 – Automatización y Scripting aplicado a Ciberseguridad 🤖🔐

## 0. Introducción a la semana

Hasta ahora has trabajado mucho de forma "manual":

- Revisando procesos y logs.
- Lanzando comandos en Linux y Windows.
- Explorando redes y cloud.

Eso está muy bien, pero en ciberseguridad profesional hay un punto clave:

> Si haces lo mismo muchas veces a mano, necesitas automatizarlo.

En un entorno real, la diferencia entre "ser productivo" y "ahogarte" suele estar en cómo gestionas la repetición. La automatización no es solo "hacer scripts", es construir hábitos:

- Pasos repetibles.
- Resultados comparables.
- Menos fricción para validar hipótesis.
- Menos dependencia de memoria o improvisación.

La automatización te permite:

- Ahorrar tiempo en tareas repetitivas.
- Reducir errores humanos (especialmente cuando estás cansado o con prisa).
- Estandarizar análisis (siempre haces los mismos pasos, igual).
- Prepararte para entornos reales donde se manejan **miles de eventos** al día.

Además, automatizar te cambia la mentalidad:

- Pasas de "buscar a ojo" a "definir patrones".
- Pasas de "hacerlo una vez" a "hacerlo siempre igual".
- Pasas de "me suena" a "lo puedo demostrar con salida reproducible".

En esta Semana 6 vas a:

- Entender el papel de la automatización en ciberseguridad.
- Ver conceptos básicos de Python, PowerShell y Bash (sin entrar en programación avanzada).
- Aplicar estos lenguajes a tareas muy concretas de seguridad (leer logs, buscar patrones, hacer comprobaciones).
- Ver cómo encaja la IA / vibecoding para generar scripts, pero siempre:
  - Entendiéndolos mínimamente.
  - Probándolos en tu laboratorio.

No se trata de que te conviertas en programador, sino de que veas la programación como una **herramienta de trabajo para un analista / pentester / ingeniero**. Igual que usas nmap o Wireshark, aquí vas a usar scripts como "herramientas hechas a medida".

**Prompt IA:**  
"Actúa como instructor de ciberseguridad orientado a SOC y automatización. Explica por qué automatizar es clave y cómo cambia la forma de analizar incidentes. Incluye 8 ejemplos de tareas repetitivas típicas (SOC, pentest, hardening) y cómo se podrían automatizar con Python, PowerShell o Bash. Termina con un consejo sobre cómo evitar automatizar mal."

---

## 1. Objetivos de aprendizaje de la Semana 6

Al finalizar esta semana deberías ser capaz de:

1. Explicar por qué la automatización es importante en un SOC, en pentesting y en ingeniería de seguridad.
2. Entender los conceptos básicos de scripting con Python, PowerShell y Bash (variables, leer ficheros, bucles sencillos).
3. Crear scripts muy simples que:
   - Lean un fichero de log.
   - Busquen líneas con ciertos patrones (por ejemplo, errores o palabras clave).
   - Muestren resultados o cuenten eventos.
4. Usar IA / vibecoding para generar un script inicial y luego:
   - Revisarlo.
   - Ajustarlo.
   - Probarlo en tu laboratorio.
5. Diseñar un pequeño "entorno de pruebas para scripts" dentro de tu lab (sin tocar sistemas reales).
6. Documentar tus scripts y su propósito en tu repositorio.

Estos objetivos tienen un enfoque intencionalmente práctico. No buscamos "programar bonito", buscamos:

- Que el script funcione.
- Que sea entendible para ti dentro de 2 semanas.
- Que sea seguro para ejecutar en un laboratorio.
- Que produzca salida útil para análisis o para documentación.

**Prompt IA:**  
"Actúa como evaluador de aprendizaje. Genera 12 preguntas de repaso alineadas a los 6 objetivos de la Semana 6. Para cada pregunta incluye: respuesta esperada, explicación breve y un error típico de principiante. Añade 2 mini-casos: análisis de auth.log y revisión de eventos de Windows con PowerShell."

---

## 2. ¿Por qué automatizar en ciberseguridad?

Imagina:

- Estás en un SOC con miles de eventos diarios.
- O en un pentest con decenas de hosts y servicios que revisar.
- O gestionando reglas de firewall, detectores, listas de IPs, etc.

Hacerlo todo a mano:

- Es lento.
- Es fácil que cometas errores.
- Es difícil repetir el mismo análisis semanas después.
- Es difícil explicar a otra persona exactamente qué hiciste si no está estandarizado.

La automatización aplicada a seguridad se usa, por ejemplo, para:

- Buscar patrones sospechosos en logs (intentos de login fallidos, IPs concretas, comandos raros).
- Comparar configuraciones (antes y después de aplicar parches).
- Generar informes a partir de datos (resúmenes, tablas, estadísticas).
- Orquestar acciones (bloquear IPs, aislar equipos, enviar notificaciones) en entornos más avanzados (SOAR, playbooks).

A tu nivel ahora mismo, la idea es:

> Empezar con scripts pequeños que hagan "una sola cosa útil", pero bien.

Un criterio útil: automatiza lo que cumpla al menos una de estas condiciones:

- Lo haces a menudo.
- Te equivocas con facilidad si lo haces a mano.
- Necesitas un resultado consistente para comparar.
- Te consume tiempo que podrías dedicar a pensar (no a copiar y pegar).

Ejemplos "muy SOC":

- Contar intentos de login fallidos y ver top IPs.
- Extraer errores de un log de aplicación y agrupar por tipo.
- Filtrar eventos de Windows por EventID y exportarlos.
- Validar si un host tiene ciertos servicios activos y registrar estado.

**Prompt IA:**  
"Actúa como analista SOC. Describe 10 tareas repetitivas de triage y threat hunting que se pueden automatizar con scripts simples. Para cada tarea, indica: input (qué datos), lógica (qué filtrar/contar) y output (qué debería mostrar). Mantén el nivel para un principiante."

---

## 3. Python para seguridad: visión básica

### 3.1. ¿Por qué Python?

Python es:

- Relativamente fácil de leer.
- Muy popular en ciberseguridad.
- Potente para:
  - Analizar logs (ficheros de texto).
  - Hacer peticiones HTTP.
  - Interactuar con APIs.
  - Manipular datos.

No necesitas saber clases, patrones de diseño, etc.  
Con entender:

- Variables.
- Lectura de ficheros.
- Condiciones.
- Bucles.
- Funciones simples.

...ya puedes hacer cosas muy útiles.

Además, Python te da una ventaja clara: cuando el problema crece (por ejemplo, agrupar por IP, extraer campos, deduplicar), te permite evolucionar el script sin convertirlo en un "monstruo". Eso lo hace ideal para pasar de scripts "de una tarde" a herramientas pequeñas que reutilizas.

### 3.2. Estructura mínima de un script sencillo

Ejemplo conceptual (no necesitas ejecutarlo aún, solo entender la forma):

# script_ejemplo.py

# Abrimos un fichero de log en modo lectura
with open("log_ejemplo.txt", "r", encoding="utf-8") as f:
    for linea in f:
        if "ERROR" in linea:
            print(linea.strip())

Qué hace:

- Abre un fichero.
- Recorre cada línea.
- Si la línea contiene la palabra "ERROR", la muestra.

Aplicado a seguridad:

- Podrías buscar "Failed password" en auth.log.
- Podrías buscar ciertos códigos de estado HTTP.
- Podrías filtrar por IP concreta.

Consejo práctico: en seguridad, muchos logs vienen "sucios" o con caracteres raros. Por eso es habitual ver:

- errors="ignore"

Así evitas que el script falle por una línea con una codificación inesperada. En un laboratorio no suele ser crítico, pero en producción es un clásico.

**Prompt IA:**  
"Actúa como instructor. Explica Python para análisis de logs a un alumno de ciberseguridad. Incluye una explicación de variables, bucles, condicionales y lectura de ficheros. Luego propone 5 mini-scripts útiles para SOC: contar patrones, extraer IPs, deduplicar, agrupar por usuario, y exportar resultados a un fichero."

---

## 4. PowerShell para seguridad (Windows)

### 4.1. ¿Por qué PowerShell?

PowerShell es:

- Un shell avanzado de Windows.
- Muy integrado con el sistema (registro, servicios, eventos).
- Capaz de:
  - Leer logs.
  - Consultar procesos.
  - Revisar servicios.
  - Trabajar con WMI, etc.

Es el "cuchillo suizo" para automatización en entornos Windows.

En un SOC, muchas tareas de investigación en Windows se benefician de PowerShell porque:

- Puedes filtrar eventos rápido.
- Puedes automatizar extracción de datos (procesos, conexiones, servicios).
- Puedes convertir resultados en objetos y seleccionar campos (no solo texto).
- Puedes exportar información para documentación.

### 4.2. Ejemplo básico de PowerShell

Ejemplo conceptual:

# Listar los 20 últimos eventos del log de seguridad
Get-EventLog -LogName Security -Newest 20

# Filtrar procesos que contengan 'powershell'
Get-Process | Where-Object { $_.ProcessName -like "*powershell*" }

Aplicado a seguridad:

- Buscar eventos de inicio de sesión concretos.
- Ver procesos sospechosos.
- Revisar qué scripts se ejecutan.

Nota práctica: hay dos mundos de logs en Windows:

- Get-EventLog (más sencillo, pero limitado en algunos escenarios)
- Get-WinEvent (más moderno y flexible)

Para este curso, empezar simple está bien. Lo importante es entender el concepto: "consultar, filtrar, mostrar".

**Prompt IA:**  
"Actúa como instructor. Explica PowerShell para tareas de seguridad: procesos, servicios, red y eventos. Incluye ejemplos básicos de filtrado con Where-Object y selección de campos con Select-Object. Termina con 6 ideas de scripts simples de Blue Team para Windows."

---

## 5. Bash para seguridad (Linux)

### 5.1. ¿Qué es Bash?

Bash es:

- El shell más común en sistemas Linux.
- Un lenguaje de scripting que te permite encadenar comandos.

Es ideal para:

- Automatizar tareas en servidores Linux.
- Procesar logs con herramientas como grep, awk, cut, etc.
- Crear scripts que se ejecuten con cron (tareas programadas).

Bash es muy práctico cuando:

- El input es texto.
- La lógica es sencilla (filtrar, contar, extraer).
- Quieres resultados rápidos sin escribir un programa completo.

### 5.2. Ejemplo básico de script Bash

#!/bin/bash

# Script que cuenta cuántas veces aparece "Failed password" en auth.log

ARCHIVO="/var/log/auth.log"

grep "Failed password" "$ARCHIVO" | wc -l

Aplicado a seguridad:

- Puedes contar intentos de login fallidos.
- Buscar ciertos patrones de error de servicios.
- Generar pequeños resúmenes.

Un detalle importante: Bash es potente pero puede volverse difícil de mantener cuando empiezas a hacer parsing complejo. Por eso, una buena práctica es:

- Bash para scripts simples y rápidos.
- Python para parsing más estructurado y evolutivo.

**Prompt IA:**  
"Actúa como instructor. Explica Bash para análisis de logs de Linux usando grep, wc, tail y pipes. Propón 5 scripts cortos que ayuden a un analista SOC: contar fallos de SSH, top IPs, detectar palabras clave, revisar últimas líneas y generar un resumen en un fichero."

---

## 6. IA y vibecoding: cómo usar ChatGPT/IA para scripts de forma responsable

En este curso, la IA no es "magia", es un **asistente**.

Flujo de trabajo recomendado:

1. Tienes un objetivo claro:
   - "Quiero un script que lea este log y cuente intentos de login fallidos por IP."
2. Pides a la IA un script inicial (en Python, Bash o PowerShell).
3. **Lees** el script:
   - Qué hace línea a línea.
   - Si usa comandos que reconoces.
   - Si accede a rutas peligrosas sin sentido.
4. Lo ejecutas en tu **laboratorio**, nunca en producción.
5. Validas resultados:
   - Si da el conteo esperado.
   - Si se comporta como esperas.

La clave:

> La IA escribe rápido, pero el criterio y la responsabilidad son tuyos.

En seguridad esto es especialmente importante por dos razones:

- Un script puede borrar o modificar cosas si está mal.
- Un script puede darte "falsos resultados" y llevarte a conclusiones incorrectas.

Buenas prácticas cuando uses IA para scripting:

- Pide scripts con "modo seguro": solo lectura, no modificar, no borrar.
- Pide que el script imprima antes de actuar.
- Pide que incluya comentarios explicativos.
- Prueba con ficheros de copia, nunca con los originales.
- Haz pequeñas pruebas controladas para verificar.

**Prompt IA:**  
"Actúa como mentor de uso responsable de IA para scripting en ciberseguridad. Define un checklist de revisión de scripts generados por IA (seguridad, lógica, inputs, outputs, errores). Incluye 10 señales de alerta (red flags) que indicarían que no debes ejecutar un script sin revisarlo."

---

## 7. Entorno de pruebas para scripts dentro de tu laboratorio

Aunque ya tienes el lab montado, es buena idea que:

- Tengas una **carpeta de trabajo para scripts** en cada VM.
- Tengas algunos **ficheros de log de prueba** (copias, no los originales).
- Nunca empieces a automatizar tocando ficheros críticos sin copia.

Propuesta:

- En Ubuntu:  
  Carpeta "/home/tuusuario/scripts_seguridad/"
- En Kali:  
  Carpeta "/home/kali/scripts/"
- En Windows:  
  Carpeta "C:\ScriptsSeguridad\"

Usa esa estructura para:

- Guardar scripts.
- Guardar logs de prueba (copias de "/var/log/auth.log", por ejemplo).
- Hacer experimentos sin miedo.

Un punto adicional útil para tu repositorio: crea un estándar de nombres:

- "logins_ssh_contar.sh"
- "logins_ssh_filtrar.py"
- "eventos_seguridad_listar.ps1"

Eso te permite crecer sin perderte y, si algún día compartes el repo, se entiende rápido.

**Prompt IA:**  
"Actúa como arquitecto de laboratorio personal. Propón una estructura de carpetas para scripts y datos de prueba en Linux y Windows. Añade recomendaciones de naming, cómo guardar ejemplos de salida y cómo documentar propósito, uso y limitaciones en Markdown."

---

## 8. Laboratorio de la Semana 6

Vamos a hacer varios ejercicios sencillos, uno por lenguaje, pensados para seguridad.

Idea clave: cada ejercicio te deja algo reutilizable. No son ejercicios "para aprobar", son piezas que podrías usar como base para automatizar tareas reales.

**Prompt IA:**  
"Actúa como instructor. Convierte los ejercicios 8.1 a 8.4 en una guía paso a paso aún más detallada, incluyendo qué comprobar si algo falla, qué salida esperar, y cómo validar que el script funciona. Mantén el enfoque en laboratorio y en seguridad."

### 8.1. Ejercicio 1 – Script Bash para contar intentos de login fallidos

En tu Ubuntu Server:

1. Crea la carpeta de scripts:

   mkdir -p ~/scripts_seguridad
   cd ~/scripts_seguridad

2. Haz una copia del log de autenticación (para no trabajar directamente sobre el original):

   sudo cp /var/log/auth.log ./auth_log_copia.log
   sudo chown "$USER":"$USER" auth_log_copia.log

   Nota: si el fichero es grande, no pasa nada. Aquí estás aprendiendo a procesarlo. En entornos reales, parte de la habilidad está en filtrar y no cargar todo a ojo.

3. Crea un script "contar_fallos_ssh.sh":

   nano contar_fallos_ssh.sh

   Contenido sugerido:

   #!/bin/bash

   ARCHIVO="auth_log_copia.log"

   echo "Número de intentos de login fallidos (Failed password):"
   grep "Failed password" "$ARCHIVO" | wc -l

4. Hazlo ejecutable y ejecútalo:

   chmod +x contar_fallos_ssh.sh
   ./contar_fallos_ssh.sh

5. Prueba a hacer algunos logins fallidos desde otra máquina (Kali) y copia de nuevo el log para ver cómo aumenta el número.

Qué estás aprendiendo aquí:

- A separar datos reales del sistema (auth.log) de datos de prueba (copia).
- A construir un script que da un dato medible.
- A validar que el dato cambia cuando generas eventos.

Documenta en:

"03-Automatizacion-Scripting-Semana_6_Bash_Logins.md"

Incluye:

- Script final.
- Ejemplo de salida.
- Reflexión: cómo podrías mejorar este script (contar por IP, por ejemplo).
- Problemas encontrados y cómo los resolviste (si grep no encuentra nada, si no hay intentos fallidos, etc.).

---

### 8.2. Ejercicio 2 – Script Python sencillo para filtrar líneas de log

En Ubuntu o Kali:

1. En la carpeta de scripts ("~/scripts_seguridad" o similar), crea un fichero "filtrar_errores.py".

2. Copia dentro algo como:

   # filtrar_errores.py

   RUTA_LOG = "auth_log_copia.log"
   PATRON = "Failed password"

   contador = 0

   with open(RUTA_LOG, "r", encoding="utf-8", errors="ignore") as f:
       for linea in f:
           if PATRON in linea:
               print(linea.strip())
               contador += 1

   print(f"\nTotal de líneas que contienen '{PATRON}': {contador}")

3. Ejecuta el script:

   python3 filtrar_errores.py

Observa:

- Cómo imprime cada línea que contiene el patrón.
- Cómo cuenta cuántas hay.
- Qué diferencia hay entre "contar" (Bash) y "mostrar detalle" (Python).

Qué estás entrenando:

- Lectura de ficheros línea a línea (muy típico en logs).
- Búsqueda de patrones.
- Salida reproducible que puedes pegar en un informe.

Documenta en:

"03-Automatizacion-Scripting-Semana_6_Python_Log.md"

Incluye:

- Código del script (o versión final).
- Resultado.
- Ideas de mejora:
  - Guardar salida en un fichero.
  - Contar por IP.
  - Aceptar parámetros por línea de comandos.

---

### 8.3. Ejercicio 3 – PowerShell para listar eventos de seguridad en Windows

En tu máquina Windows:

1. Crea la carpeta "C:\ScriptsSeguridad\".
2. Abre PowerShell como usuario normal (no hace falta admin para leer muchos eventos).
3. Crea un script "ListarEventosSeguridad.ps1" en esa carpeta.

   Contenido mínimo:

   # ListarEventosSeguridad.ps1

   Write-Host "Últimos 30 eventos del log de Seguridad:"
   Get-EventLog -LogName Security -Newest 30 |
       Select-Object TimeGenerated, EntryType, Source, EventID, Message

4. Ejecuta el script desde PowerShell:

   cd C:\ScriptsSeguridad
   .\ListarEventosSeguridad.ps1

5. Observa:

   - Qué eventos salen.
   - Qué EventID ves con más frecuencia.
   - Si aparece algo relacionado con inicios de sesión (según auditoría y configuración).

Qué estás aprendiendo:

- A consultar el "registro de evidencias" principal de Windows.
- A extraer campos concretos para no imprimir todo sin control.
- A crear una base para futuros filtros (por EventID, por usuario, por rango de tiempo).

Documenta en:

"03-Automatizacion-Scripting-Semana_6_PowerShell_Eventos.md"

Incluye:

- Script utilizado.
- Ejemplo de salida.
- Nota: si ves algún EventID repetitivo, anótalo (más adelante puede servir para buscar patrones).

---

### 8.4. Ejercicio 4 – Usar IA/vibecoding para mejorar un script

Elige uno de los scripts anteriores (Bash, Python o PowerShell) y:

1. Plantea una mejora concreta. Ejemplos:
   - Que el script de Bash muestre las IP de los intentos fallidos y cuántos intentos lleva cada IP.
   - Que el script de Python reciba el nombre del fichero y el patrón por parámetros.
   - Que el script de PowerShell filtre solo eventos de inicio de sesión (por EventID, por ejemplo).

2. Pide a una IA (por ejemplo ChatGPT) que:
   - Tome tu script actual.
   - Añada la funcionalidad que quieres.

3. Recibe el script mejorado.
4. Revísalo línea a línea:
   - Entiendes qué hace cada parte.
   - Hace algo raro que no esperas.

5. Prueba el script en tu laboratorio:
   - Comprueba si el resultado se ajusta a lo que querías.
   - Si no cuadra, ajusta y vuelve a probar.

Documenta el proceso en:

"03-Automatizacion-Scripting-Semana_6_Vibecoding.md"

Incluye:

- Script original (resumen).
- Script mejorado por IA.
- Comentario: qué has entendido, qué te gustaría mejorar más adelante.
- Qué validación hiciste (por ejemplo: generaste 5 intentos fallidos y esperabas ver 5).

**Prompt IA:**  
"Actúa como asistente de vibecoding. Toma un script que cuenta 'Failed password' y mejóralo para: extraer IPs, agrupar por IP y ordenar de mayor a menor. Devuelve versión Bash y versión Python. Incluye comentarios y evita acciones de escritura o borrado, solo lectura."

---

## 9. Entregables de la Semana 6

Al final de la semana deberías tener:

1. "03-Automatizacion-Scripting-Semana_6_Bash_Logins.md"  
   - Script Bash para contar intentos fallidos.
   - Captura de salida y comentarios.

2. "03-Automatizacion-Scripting-Semana_6_Python_Log.md"  
   - Script Python para filtrar líneas con un patrón.
   - Resultados y posibles ideas de evolución.

3. "03-Automatizacion-Scripting-Semana_6_PowerShell_Eventos.md"  
   - Script PowerShell básico para listar eventos de seguridad.
   - Ejemplo de salida.

4. "03-Automatizacion-Scripting-Semana_6_Vibecoding.md"  
   - Proceso de mejora con IA.
   - Script resultante.
   - Reflexión sobre el uso responsable de IA en automatización de seguridad.

Opcional:

- Ampliar tu "Lab_Setup_Report.md" con una sección "Scripts creados y propósito".

Sugerencia para que el repositorio te quede "pro":

- En cada entregable, añade una sección final:
  - "Limitaciones"
  - "Siguiente mejora"
  - "Cómo lo usaría en un caso real"

Esto te entrena a pensar como profesional: nada es perfecto, todo es iterativo.

**Prompt IA:**  
"Actúa como revisor de repositorio. Genera plantillas Markdown para los 4 entregables de la Semana 6 con secciones: objetivo, input, script, ejecución, output esperado, validación, limitaciones, mejoras futuras. Añade un ejemplo corto de contenido en cada sección."

---

## 10. Checklist de cierre de la Semana 6

Marca como completado cuando:

- [ ] Puedes explicar por qué la automatización es importante en ciberseguridad (SOC, pentesting, ingeniería).
- [ ] Has creado y ejecutado al menos un script Bash que hace algo útil con logs.
- [ ] Has creado y ejecutado al menos un script Python que lee un fichero y filtra contenido.
- [ ] Has creado y ejecutado un script PowerShell que trabaja con eventos de seguridad.
- [ ] Has usado una IA para mejorar un script, lo has revisado y probado en tu laboratorio.
- [ ] Tienes todos los ficheros de la Semana 6 en tu repositorio y sabes qué hace cada script.

Si has completado esta semana, has dado un salto importante:  
ya no eres solo alguien que "lanza comandos", sino alguien que empieza a **automatizar tareas**, lo cual te acerca mucho más al trabajo real en un SOC, en un equipo de Blue Team o en un pentest.

Señales prácticas de progreso:

- Tardas menos en obtener la misma evidencia.
- Tu salida es más consistente.
- Puedes explicar "qué hiciste" con un script reproducible.
- Puedes evolucionar un script en vez de empezar de cero.

En la **Semana 7**, empezaremos a usar estos conocimientos para entrar de lleno en **fundamentos de hacking ético**, usando Kali y herramientas ofensivas, conectando red, sistemas y automatización.

**Prompt IA:**  
"Actúa como evaluador final de la Semana 6. Crea un mini-examen con 8 preguntas tipo test, 6 preguntas abiertas y 2 casos prácticos: uno en Linux con auth.log y otro en Windows con eventos. Para cada caso pide: objetivo del script, lógica, output esperado y cómo validar que funciona. Incluye una rúbrica simple de puntuación."

---

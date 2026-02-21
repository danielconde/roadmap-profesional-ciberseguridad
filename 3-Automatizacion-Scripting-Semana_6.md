# Semana 6 – Automatización y Scripting aplicado a Ciberseguridad 🤖🔐

## 0. Introducción a la semana

Hasta ahora has trabajado mucho de forma “manual”:

- Revisando procesos y logs.
- Lanzando comandos en Linux y Windows.
- Explorando redes y cloud.

Eso está muy bien, pero en ciberseguridad profesional hay un punto clave:

> Si haces lo mismo muchas veces a mano, necesitas automatizarlo.

La automatización te permite:

- Ahorrar tiempo en tareas repetitivas.
- Reducir errores humanos.
- Estandarizar análisis (siempre haces los mismos pasos, igual).
- Prepararte para entornos reales donde se manejan **miles de eventos** al día.

En esta Semana 6 vas a:

- Entender el papel de la automatización en ciberseguridad.
- Ver conceptos básicos de **Python**, **PowerShell** y **Bash** (sin entrar en programación avanzada).
- Aplicar estos lenguajes a tareas muy concretas de seguridad (leer logs, buscar patrones, hacer comprobaciones).
- Ver cómo encaja la **IA / vibecoding** para generar scripts, pero siempre:
  - Entendiéndolos mínimamente.
  - Probándolos en tu laboratorio.

No se trata de que te conviertas en programador, sino de que veas la programación como una **herramienta de trabajo para un analista / pentester / ingeniero**.

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
5. Diseñar un pequeño “entorno de pruebas para scripts” dentro de tu lab (sin tocar sistemas reales).
6. Documentar tus scripts y su propósito en tu repositorio.

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

La automatización aplicada a seguridad se usa, por ejemplo, para:

- Buscar patrones sospechosos en logs (intentos de login fallidos, IPs concretas, comandos raros).
- Comparar configuraciones (antes y después de aplicar parches).
- Generar informes a partir de datos (resúmenes, tablas, estadísticas).
- Orquestar acciones (bloquear IPs, aislar equipos, enviar notificaciones) en entornos más avanzados (SOAR, playbooks).

A tu nivel ahora mismo, la idea es:

> Empezar con scripts pequeños que hagan **una sola cosa útil**, pero bien.

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

…ya puedes hacer cosas muy útiles.

### 3.2. Estructura mínima de un script sencillo

Ejemplo conceptual (no necesitas ejecutarlo aún, solo entender la forma):

```python
# script_ejemplo.py

# Abrimos un fichero de log en modo lectura
with open("log_ejemplo.txt", "r", encoding="utf-8") as f:
    for linea in f:
        if "ERROR" in linea:
            print(linea.strip())
```

Qué hace:

- Abre un fichero.
- Recorre cada línea.
- Si la línea contiene la palabra “ERROR”, la muestra.

Aplicado a seguridad:

- Podrías buscar “Failed password” en auth.log.
- Podrías buscar ciertos códigos de estado HTTP.
- Podrías filtrar por IP concreta.

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

Es el “cuchillo suizo” para automatización en entornos Windows.

### 4.2. Ejemplo básico de PowerShell

Ejemplo conceptual:

```powershell
# Listar los 20 últimos eventos del log de seguridad
Get-EventLog -LogName Security -Newest 20

# Filtrar procesos que contengan 'powershell'
Get-Process | Where-Object { $_.ProcessName -like "*powershell*" }
```

Aplicado a seguridad:

- Buscar eventos de inicio de sesión concretos.
- Ver procesos sospechosos.
- Revisar qué scripts se ejecutan.

---

## 5. Bash para seguridad (Linux)

### 5.1. ¿Qué es Bash?

Bash es:

- El shell más común en sistemas Linux.
- Un lenguaje de scripting que te permite encadenar comandos.

Es ideal para:

- Automatizar tareas en servidores Linux.
- Procesar logs con herramientas como `grep`, `awk`, `cut`, etc.
- Crear scripts que se ejecuten con cron (tareas programadas).

### 5.2. Ejemplo básico de script Bash

```bash
#!/bin/bash

# Script que cuenta cuántas veces aparece "Failed password" en auth.log

ARCHIVO="/var/log/auth.log"

grep "Failed password" "$ARCHIVO" | wc -l
```

Aplicado a seguridad:

- Puedes contar intentos de login fallidos.
- Buscar ciertos patrones de error de servicios.
- Generar pequeños resúmenes.

---

## 6. IA y vibecoding: cómo usar ChatGPT/IA para scripts de forma responsable

En este curso, la IA no es “magia”, es un **asistente**.

Flujo de trabajo recomendado:

1. Tienes un objetivo claro:
   - “Quiero un script que lea este log y cuente intentos de login fallidos por IP.”
2. Pides a la IA un script inicial (en Python, Bash o PowerShell).
3. **Lees** el script:
   - ¿Qué hace línea a línea?
   - ¿Usa comandos que reconoces?
   - ¿Accede a rutas peligrosas sin sentido?
4. Lo ejecutas en tu **laboratorio**, nunca en producción.
5. Validas resultados:
   - ¿Da el conteo esperado?
   - ¿Se comporta como esperas?

La clave:

> La IA escribe rápido, pero el criterio y la responsabilidad son tuyos.

---

## 7. Entorno de pruebas para scripts dentro de tu laboratorio

Aunque ya tienes el lab montado, es buena idea que:

- Tengas una **carpeta de trabajo para scripts** en cada VM.
- Tengas algunos **ficheros de log de prueba** (copias, no los originales).
- Nunca empieces a automatizar tocando ficheros críticos sin copia.

Propuesta:

- En Ubuntu:  
  Carpeta `/home/tuusuario/scripts_seguridad/`
- En Kali:  
  Carpeta `/home/kali/scripts/`
- En Windows:  
  Carpeta `C:\ScriptsSeguridad\`

Usa esa estructura para:

- Guardar scripts.
- Guardar logs de prueba (copias de `/var/log/auth.log`, por ejemplo).
- Hacer experimentos sin miedo.

---

## 8. Laboratorio de la Semana 6

Vamos a hacer varios ejercicios sencillos, uno por lenguaje, pensados para seguridad.

### 8.1. Ejercicio 1 – Script Bash para contar intentos de login fallidos

En tu Ubuntu Server:

1. Crea la carpeta de scripts:

   ```bash
   mkdir -p ~/scripts_seguridad
   cd ~/scripts_seguridad
   ```

2. Haz una copia del log de autenticación (para no trabajar directamente sobre el original):

   ```bash
   sudo cp /var/log/auth.log ./auth_log_copia.log
   sudo chown "$USER":"$USER" auth_log_copia.log
   ```

3. Crea un script `contar_fallos_ssh.sh`:

   ```bash
   nano contar_fallos_ssh.sh
   ```

   Contenido sugerido:

   ```bash
   #!/bin/bash

   ARCHIVO="auth_log_copia.log"

   echo "Número de intentos de login fallidos (Failed password):"
   grep "Failed password" "$ARCHIVO" | wc -l
   ```

4. Hazlo ejecutable y ejecútalo:

   ```bash
   chmod +x contar_fallos_ssh.sh
   ./contar_fallos_ssh.sh
   ```

5. Prueba a hacer algunos logins fallidos desde otra máquina (Kali) y copia de nuevo el log para ver cómo aumenta el número.

Documenta en:

`03-Automatizacion-Scripting-Semana_6_Bash_Logins.md`

Incluye:

- Script final.
- Ejemplo de salida.
- Reflexión: ¿cómo podrías mejorar este script? (contar por IP, por ejemplo).

---

### 8.2. Ejercicio 2 – Script Python sencillo para filtrar líneas de log

En Ubuntu o Kali:

1. En la carpeta de scripts (`~/scripts_seguridad` o similar), crea un fichero `filtrar_errores.py`.

2. Copia dentro algo como:

   ```python
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
   ```

3. Ejecuta el script:

   ```bash
   python3 filtrar_errores.py
   ```

Observa:

- Cómo imprime cada línea que contiene el patrón.
- Cómo cuenta cuántas hay.

Documenta en:

`03-Automatizacion-Scripting-Semana_6_Python_Log.md`

Incluye:

- Código del script (o versión final).
- Resultado.
- Ideas de mejora (por ejemplo, agrupar por IP en el futuro).

---

### 8.3. Ejercicio 3 – PowerShell para listar eventos de seguridad en Windows

En tu máquina Windows:

1. Crea la carpeta `C:\ScriptsSeguridad\`.
2. Abre PowerShell como usuario normal (no hace falta admin para leer muchos eventos).
3. Crea un script `ListarEventosSeguridad.ps1` en esa carpeta.

   Contenido mínimo:

   ```powershell
   # ListarEventosSeguridad.ps1

   Write-Host "Últimos 30 eventos del log de Seguridad:"
   Get-EventLog -LogName Security -Newest 30 |
       Select-Object TimeGenerated, EntryType, Source, EventID, Message
   ```

4. Ejecuta el script desde PowerShell:

   ```powershell
   cd C:\ScriptsSeguridad
   .\ListarEventosSeguridad.ps1
   ```

5. Observa:

   - Qué eventos salen.
   - Qué EventID ves con más frecuencia.

Documenta en:

`03-Automatizacion-Scripting-Semana_6_PowerShell_Eventos.md`

Incluye:

- Script utilizado.
- Ejemplo de salida.
- Nota: si ves algún EventID repetitivo, anótalo (más adelante puede servir para buscar patrones).

---

### 8.4. Ejercicio 4 – Usar IA/vibecoding para mejorar un script

Elige uno de los scripts anteriores (Bash, Python o PowerShell) y:

1. Plantea una mejora concreta. Ejemplos:
   - Que el script de Bash muestre las **IP** de los intentos fallidos y cuántos intentos lleva cada IP.
   - Que el script de Python reciba el nombre del fichero y el patrón por parámetros.
   - Que el script de PowerShell filtre solo eventos de inicio de sesión (por EventID, por ejemplo).

2. Pide a una IA (por ejemplo ChatGPT) que:
   - Tome tu script actual.
   - Añada la funcionalidad que quieres.

3. Recibe el script mejorado.
4. Revísalo línea a línea:
   - ¿Entiendes qué hace cada parte?
   - ¿Hace algo raro que no esperas?

5. Prueba el script en tu laboratorio:
   - Comprueba si el resultado se ajusta a lo que querías.

Documenta el proceso en:

`03-Automatizacion-Scripting-Semana_6_Vibecoding.md`

Incluye:

- Script original (resumen).
- Script mejorado por IA.
- Comentario: qué has entendido, qué te gustaría mejorar más adelante.

---

## 9. Entregables de la Semana 6

Al final de la semana deberías tener:

1. `03-Automatizacion-Scripting-Semana_6_Bash_Logins.md`  
   - Script Bash para contar intentos fallidos.
   - Captura de salida y comentarios.

2. `03-Automatizacion-Scripting-Semana_6_Python_Log.md`  
   - Script Python para filtrar líneas con un patrón.
   - Resultados y posibles ideas de evolución.

3. `03-Automatizacion-Scripting-Semana_6_PowerShell_Eventos.md`  
   - Script PowerShell básico para listar eventos de seguridad.
   - Ejemplo de salida.

4. `03-Automatizacion-Scripting-Semana_6_Vibecoding.md`  
   - Proceso de mejora con IA.
   - Script resultante.
   - Reflexión sobre el uso responsable de IA en automatización de seguridad.

Opcional:

- Ampliar tu `Lab_Setup_Report.md` con una sección “Scripts creados y propósito”.

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
ya no eres solo alguien que “lanza comandos”, sino alguien que empieza a **automatizar tareas**, lo cual te acerca mucho más al trabajo real en un SOC, en un equipo de Blue Team o en un pentest.

En la **Semana 7**, empezaremos a usar estos conocimientos para entrar de lleno en **fundamentos de hacking ético**, usando Kali y herramientas ofensivas, conectando red, sistemas y automatización.

---

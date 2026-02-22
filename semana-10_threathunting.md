# Semana 10 – Threat Hunting Básico y Correlación de Eventos 🕵️‍♂️📈

## 0. Introducción a la semana

A estas alturas ya has tocado:

- Fundamentos de ciberseguridad, redes, sistemas y datos.
- Cloud y responsabilidad compartida.
- Automatización con Bash, Python y PowerShell.
- Hacking ético (visión ofensiva básica).
- Blue Team, SOC y detección basada en alertas.
- Seguridad en aplicaciones web (OWASP) y análisis con DevTools/Burp.

Ahora damos un paso importante hacia un rol más **proactivo**:

> Threat Hunting = buscar amenazas de forma activa, no solo reaccionar a alertas.

Mientras que en el SOC clásico esperas a que una alerta salte, en **Threat Hunting**:

- Planteas hipótesis de ataque.
- Buscas evidencias en los datos (logs, eventos).
- Intentas encontrar comportamientos sospechosos que no están cubiertos por reglas todavía.
- Aprendes del entorno y mejoras detecciones.

Esta Semana 10 te introduce en la mentalidad y el flujo del threat hunter, usando:

- Los logs que ya tienes (Linux, Windows, logs de pruebas que has generado).
- Scripts sencillos (Bash, Python, PowerShell).
- Tu conocimiento de MITRE, Kill Chain y ataques vistos.

---

## 1. Objetivos de aprendizaje de la Semana 10

Al finalizar esta semana deberías ser capaz de:

1. Explicar qué es Threat Hunting y en qué se diferencia de:
   - Detección basada en alertas.
   - Respuesta a incidentes.
2. Entender el ciclo básico de Threat Hunting:
   - Hipótesis → Búsqueda → Análisis → Conclusiones → Mejoras.
3. Formular hipótesis de caza basadas en:
   - Comportamientos de ataque conocidos (MITRE, Kill Chain).
   - Riesgos típicos (PowerShell malicioso, SSH sospechoso, credenciales).
4. Diseñar búsquedas sencillas sobre logs:
   - Linux: `auth.log` (SSH, sudo).
   - Windows: eventos de seguridad y PowerShell.
5. Usar scripts o comandos para:
   - Agregar y contar eventos por IP/usuario/proceso.
   - Identificar patrones anómalos (por ejemplo, muchos fallos desde una misma IP).
6. Documentar el resultado de cada “hunt”:
   - Qué buscabas.
   - Qué encontraste.
   - Cómo se podría convertir en una regla de detección.

---

## 2. ¿Qué es Threat Hunting?

### 2.1. Definición

Threat Hunting es:

> Un proceso proactivo y guiado por hipótesis para buscar actividades maliciosas o anómalas en un entorno, basándose en comportamientos, TTPs y conocimiento del entorno.

Claves:

- **Proactivo**: no esperas a que el SIEM dispare algo.
- **Guiado por hipótesis**: no miras datos al azar; tienes una idea concreta de lo que quieres encontrar.
- **Iterativo**: incluso si no encuentras nada, aprendes algo (sobre el entorno, los datos, los gaps).

### 2.2. Diferencias con detección tradicional

- Detección tradicional:
  - Basada en reglas o firmas predefinidas.
  - Reacciona a patrones ya conocidos.
- Threat Hunting:
  - Explora escenarios que pueden no estar cubiertos por reglas.
  - Basado en TTPs (tácticas, técnicas y procedimientos), no solo en IOCs.

Ambas se complementan:

- De un buen hunt pueden salir nuevas reglas y casos de uso.
- De incidentes detectados pueden salir nuevas hipótesis de hunting.

---

## 3. Ciclo básico de Threat Hunting

Podemos resumir el flujo en cuatro pasos:

1. **Hipótesis**
   - Basada en:
     - Inteligencia de amenazas (nuevas campañas, TTPs).
     - Conocimiento interno (comportamientos “normales” del entorno).
     - Experiencia o sospechas (ej. “vemos mucho uso de PowerShell…”).

2. **Búsqueda**
   - Diseñar consultas sobre los datos:
     - Logs de sistemas.
     - Eventos de EDR.
     - Logs de aplicaciones.
   - Usar scripts, SIEM, herramientas de análisis.

3. **Análisis**
   - Interpretar resultados:
     - ¿Lo que ves es normal?
     - ¿Hay patrones raros o anómalos?
   - Profundizar donde veas señales raras.

4. **Conclusiones y mejora**
   - Si encuentras actividad sospechosa:
     - ¿Es un incidente? ¿Se debe escalar?
     - ¿Se puede convertir en una regla de detección?
   - Si no encuentras nada:
     - ¿Es porque no pasa?
     - ¿O porque no tienes visibilidad adecuada?

---

## 4. Fuentes de datos en tu laboratorio

Tú ya tienes varias fuentes interesantes para practicar (aunque aún no tengas un SIEM “real”):

- **Linux (Ubuntu/Kali)**:
  - `/var/log/auth.log` → logins, sudo, SSH.
  - Otros logs de sistema (`/var/log/syslog`, etc.).

- **Windows**:
  - Visor de eventos → Registro de Seguridad.
  - Registros de PowerShell (si están habilitados).

- **Logs de pruebas** que generaste:
  - Ficheros de auth copiados (`auth_log_copia.log`, `auth_log_semana8.log`).
  - Logs de síntomas de IOC (`log_ioc_test.log`).
  - Logs de web (si tienes Apache/Nginx con acceso habilitado).

La idea es hacer hunts pequeños pero realistas sobre estos datos.

---

## 5. Diseñar hipótesis de hunting (ejemplos)

Algunos ejemplos sencillos adaptados a tu entorno:

1. **Hipótesis 1 – Sharp**  
   > “Si alguien estuviera intentando comprometer SSH en mi servidor, vería muchas combinaciones de usuario/IP con intentos fallidos concentrados en poco tiempo.”

2. **Hipótesis 2 – PowerShell sospechoso**  
   > “Si alguien abusa de PowerShell en Windows para actividades maliciosas, vería invocaciones con parámetros extraños, en modo oculto o sin perfil, probablemente desde usuarios no administradores.”

3. **Hipótesis 3 – Usuarios poco frecuentes con sudo**  
   > “Si un atacante consigue credenciales de un usuario poco privilegiado y las usa para escalar privilegios, vería usos de `sudo` inusuales para esos usuarios.”

En esta semana vamos a trabajar especialmente con:

- SSH y autenticaciones en Linux.
- PowerShell en Windows.

---

## 6. Laboratorio de la Semana 10

### 6.1. Ejercicio 1 – Definir tus hipótesis de hunting

Crea un fichero:

`07-Threat-Hunting-Semana_10_Hipotesis.md`

Incluye al menos **3 hipótesis** adaptadas a tu laboratorio. Por ejemplo:

1. **SSH brute force / password spraying en Ubuntu**
   - Logs: `/var/log/auth.log` o copias.
   - Indicadores:
     - Muchas líneas con `Failed password`.
     - Mismas IPs contra múltiples usuarios.
     - Muchos intentos contra un mismo usuario.

2. **Abuso de PowerShell en Windows**
   - Logs: Event Viewer, `Windows PowerShell` / `Operational`.
   - Indicadores:
     - Uso de `-NoProfile`, `-WindowStyle Hidden`.
     - Comandos con `DownloadString`, `Invoke-Expression`, etc. (aunque inicialmente solo simules algunos).

3. **Uso de sudo por usuarios poco frecuentes**
   - Logs: `/var/log/auth.log`.
   - Indicadores:
     - `sudo` utilizado por usuarios que normalmente no lo usan.
     - Comandos “sensibles” (por ejemplo, modificación de config, usuarios, etc.).

Para cada hipótesis:

- Describe:
  - **Qué buscarás**.
  - **En qué fuente de datos**.
  - **Qué resultado consideras sospechoso**.

---

### 6.2. Ejercicio 2 – Hunting en Linux: SSH e intentos de login

Vamos a extender lo que ya hiciste en semanas anteriores.

1. Asegúrate de tener un log de autenticación de prueba, por ejemplo:
   - `~/scripts_seguridad/auth_log_semana8.log`
   - O una nueva copia de `/var/log/auth.log`.

2. Desde tu máquina Kali, genera algunos intentos de login:
   - Algunos correctos (si tienes un usuario de pruebas).
   - Varios fallidos con diferentes usuarios (incluyendo usuarios que no existen).

3. Copia de nuevo el log actualizado:

   ```bash
   sudo cp /var/log/auth.log ~/scripts_seguridad/auth_log_semana10.log
   sudo chown "$USER":"$USER" ~/scripts_seguridad/auth_log_semana10.log
   ```

4. Crea un script **Python** o **Bash** para:

   - Extraer todas las líneas con `Failed password`.
   - Agrupar por IP origen.
   - Contar cuántos intentos fallidos hay por IP.
   - (Opcional) Agrupar también por usuario.

Ejemplo de enfoque en Python:

```python
# hunt_ssh_failed_logins.py

from collections import defaultdict

RUTA_LOG = "auth_log_semana10.log"

conteo_por_ip = defaultdict(int)

with open(RUTA_LOG, "r", encoding="utf-8", errors="ignore") as f:
    for linea in f:
        if "Failed password" in linea:
            partes = linea.split()
            # Suele aparecer "from <IP> port X ssh2"
            # Buscamos la palabra "from" y la siguiente como IP
            if "from" in partes:
                idx = partes.index("from")
                if idx + 1 < len(partes):
                    ip = partes[idx + 1]
                    conteo_por_ip[ip] += 1

print("Intentos fallidos de SSH por IP:")
for ip, cuenta in conteo_por_ip.items():
    print(f"{ip}: {cuenta}")
```

5. Ejecuta el script y analiza:

   ```bash
   python3 hunt_ssh_failed_logins.py
   ```

6. Interpreta resultados:

   - ¿Una IP destaca sobre las demás?
   - ¿Cuántos intentos serían “normales” y cuántos te parecen sospechosos?

Crea:

`07-Threat-Hunting-Semana_10_Hunt1_SSH_Linux.md`

Incluye:

- Hipótesis que estás testeando.
- Script usado (o pseudocódigo).
- Salida resumida.
- Conclusiones:
  - ¿Encontraste algo “raro”?
  - ¿Qué umbral usarías para disparar una alerta en un SIEM?

---

### 6.3. Ejercicio 3 – Hunting en Windows: PowerShell inusual

Ahora toca Windows.

1. Desde tu usuario de Windows, genera algunas ejecuciones de PowerShell “normales”, por ejemplo:

   ```powershell
   Get-Process
   Get-Service
   ```

2. Después, genera 2–3 ejecuciones “más sospechosas”, por ejemplo:

   ```powershell
   powershell.exe -NoProfile -WindowStyle Hidden -Command "Write-Output 'Test Hidden 1'"
   powershell.exe -NoProfile -Command "Invoke-Expression 'Get-Date'"
   ```

3. Abre el **Visor de eventos** y comprueba que los eventos de PowerShell están registrando estas actividades.

4. Ahora, crea un script **PowerShell** para buscar eventos con ciertas características:

   ```powershell
   # Hunt_PowerShell_Sospechoso.ps1

   $logName = "Windows PowerShell"
   $maxEvents = 200

   Write-Host "Buscando eventos de PowerShell sospechosos (NoProfile o WindowStyle Hidden)..."

   Get-EventLog -LogName $logName -Newest $maxEvents |
       Where-Object {
           $_.Message -like "*-NoProfile*" -or
           $_.Message -like "*WindowStyle Hidden*"
       } |
       Select-Object TimeGenerated, EntryType, Source, EventID, Message
   ```

5. Ejecuta el script:

   ```powershell
   .\Hunt_PowerShell_Sospechoso.ps1
   ```

6. Analiza:

   - ¿Qué eventos salen?
   - ¿Corresponden a los comandos que lanzaste?
   - ¿Qué otros datos se muestran (usuario, máquina, etc.)?

Crea:

`07-Threat-Hunting-Semana_10_Hunt2_PowerShell_Windows.md`

Incluye:

- Hipótesis probada.
- Script PowerShell.
- Ejemplo de eventos encontrados.
- Conclusión:
  - ¿Este patrón sería sospechoso en producción?
  - ¿Qué alertas crearías a partir de esto?

---

### 6.4. Ejercicio 4 – De hunting a detección: diseñar “reglas”

Ahora, convierte los resultados de tus hunts en ideas de reglas de detección (como si fueras a implementarlas en un SIEM).

Crea:

`07-Threat-Hunting-Semana_10_Reglas_Deteccion.md`

Para cada hunt (SSH y PowerShell):

1. Describe la **condición de detección**, por ejemplo:

   - SSH:
     - “Disparar alerta si se observan más de 5 intentos fallidos de login SSH desde la misma IP en 10 minutos.”

   - PowerShell:
     - “Disparar alerta si se ejecuta PowerShell con `-NoProfile` y `-WindowStyle Hidden` desde un usuario que no pertenece al grupo de administradores.”

2. Define:

   - **Campos necesarios**:
     - IP origen, usuario, timestamp, mensaje, etc.
   - **Umbral / frecuencia**:
     - N veces en X minutos.
   - **Severidad**:
     - Alta / Media / Baja.

3. Opcional:
   - Relación con MITRE ATT&CK:
     - PowerShell sospechoso → Execution (T1059.001).
     - SSH brute force → Credential Access / Initial Access (dependiendo del contexto).

Este ejercicio te ayuda a pensar como alguien que diseña casos de uso de SIEM.

---

### 6.5. Ejercicio 5 – Resumen del hunt: mini informe

Finalmente, crea un informe corto:

`07-Threat-Hunting-Semana_10_Informe_Hunting.md`

Estructura sugerida:

1. **Introducción**
   - Objetivo: practicar Threat Hunting en entorno de laboratorio sobre SSH y PowerShell.

2. **Hipótesis**
   - Lista de hipótesis investigadas.

3. **Metodología**
   - Fuentes de datos (auth.log, eventos de PowerShell).
   - Scripts utilizados.

4. **Hallazgos**
   - Si encontraste comportamientos anómalos (aunque sean generados por ti).
   - Patrones observados.

5. **Detecciones propuestas**
   - Resumen de reglas que propones (del fichero anterior).

6. **Próximos pasos**
   - Qué te gustaría investigar en futuros hunts:
     - Uso anómalo de sudo.
     - Accesos remotos RDP inusuales.
     - Combinaciones de eventos (ej. login exitoso tras muchos fallos).

---

## 7. Entregables de la Semana 10

Al finalizar la semana deberías tener:

1. `07-Threat-Hunting-Semana_10_Hipotesis.md`  
   - Lista de hipótesis de hunting adaptadas a tu lab.

2. `07-Threat-Hunting-Semana_10_Hunt1_SSH_Linux.md`  
   - Búsqueda sobre intentos de login SSH.
   - Resultados y conclusiones.

3. `07-Threat-Hunting-Semana_10_Hunt2_PowerShell_Windows.md`  
   - Búsqueda sobre actividad sospechosa de PowerShell.
   - Resultados y conclusiones.

4. `07-Threat-Hunting-Semana_10_Reglas_Deteccion.md`  
   - Traducción de tus hunts a reglas de detección.

5. `07-Threat-Hunting-Semana_10_Informe_Hunting.md`  
   - Informe breve de Threat Hunting con hipótesis, metodología, hallazgos y propuestas.

Opcional:

- Ampliar tu `Lab_Setup_Report.md` con una sección:
  - “Capacidades de Threat Hunting” donde resumas:
    - Qué logs tienes.
    - Qué hunts básicos podrías repetir periódicamente.

---

## 8. Checklist de cierre de la Semana 10

Marca como completado cuando:

- [ ] Puedes explicar con tus palabras qué es Threat Hunting y en qué se diferencia de otras funciones del SOC.
- [ ] Has definido al menos 3 hipótesis de hunting realistas para tu laboratorio.
- [ ] Has ejecutado un hunt sobre logs de SSH en Linux y has interpretado los resultados.
- [ ] Has ejecutado un hunt sobre eventos de PowerShell en Windows y has identificado actividad “sospechosa” de prueba.
- [ ] Has convertido los resultados de tus hunts en ideas de reglas de detección.
- [ ] Has escrito un mini informe de Threat Hunting, aunque sea sobre datos simulados.
- [ ] Tienes todos los ficheros de la Semana 10 en tu repositorio y entiendes su contenido.

Si has llegado aquí, ya no solo sabes **responder** a eventos, sino también **salir a buscarlos**.  
Esa es una diferencia muy importante entre un perfil puramente reactivo y uno más avanzado (Blue/Purple Team) con mentalidad de Threat Hunter.

En la siguiente semana podremos orientar el contenido hacia:

- Profundizar en la rama SOC (más detecciones, casos de uso, reporting).
- O reforzar la parte ofensiva (pentesting más técnico).
- O combinar ambos en escenarios Purple Team.

---

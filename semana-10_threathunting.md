# Semana 10 – Threat Hunting Básico y Correlación de Eventos 🕵️‍♂️📈

## 0. Introducción a la semana

A estas alturas ya has tocado:

- Fundamentos de ciberseguridad, redes, sistemas y datos.
- Cloud y responsabilidad compartida.
- Automatización con Bash, Python y PowerShell.
- Hacking ético (visión ofensiva básica).
- Blue Team, SOC y detección basada en alertas.
- Seguridad en aplicaciones web (OWASP) y análisis con DevTools/Burp.

Ahora damos un paso importante hacia un rol más proactivo:

> Threat Hunting = buscar amenazas de forma activa, no solo reaccionar a alertas.

Mientras que en el SOC clásico esperas a que una alerta salte, en Threat Hunting:

- Planteas hipótesis de ataque (basadas en TTPs, no solo en IOCs).
- Buscas evidencias en los datos (logs, eventos, telemetría).
- Intentas encontrar comportamientos sospechosos que no están cubiertos por reglas todavía.
- Aprendes del entorno:
  - Cómo es el comportamiento normal.
  - Qué visibilidad te falta.
  - Qué detecciones tienen huecos.
- Transformas el aprendizaje en mejoras:
  - Nuevos casos de uso.
  - Ajustes de umbrales.
  - Enriquecimiento con contexto.

Un punto clave:

- En SOC, mucha investigación empieza con una alerta.
- En hunting, muchas mejoras de detección empiezan con una hipótesis.

En esta Semana 10 te introduces en la mentalidad y el flujo del threat hunter, usando:

- Los logs que ya tienes (Linux, Windows, logs de pruebas generados).
- Scripts sencillos (Bash, Python, PowerShell).
- Tu conocimiento de MITRE, Kill Chain y patrones de ataque vistos.

**Prompt IA:**  
"Actúa como mentor de Threat Hunting para un perfil junior. Explica la diferencia entre SOC reactivo y hunting proactivo. Incluye 8 ejemplos de hipótesis realistas (4 para Linux, 4 para Windows) basadas en MITRE ATT&CK. Para cada hipótesis: fuente de datos, señales observables, falsos positivos típicos y cómo convertirlo en una detección."

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
   - Riesgos típicos (PowerShell, SSH, credenciales, escaladas).
4. Diseñar búsquedas sencillas sobre logs:
   - Linux: "auth.log" (SSH, sudo).
   - Windows: eventos de seguridad y PowerShell.
5. Usar scripts o comandos para:
   - Agregar y contar eventos por IP/usuario/proceso.
   - Identificar patrones anómalos (muchos fallos desde una misma IP, picos inusuales).
6. Documentar el resultado de cada hunt:
   - Qué buscabas.
   - Qué encontraste.
   - Qué limitaciones tuvo tu búsqueda (visibilidad).
   - Cómo se podría convertir en una regla de detección.

**Prompt IA:**  
"Actúa como evaluador. Crea una rúbrica de evaluación para los entregables de Semana 10 con criterios medibles: calidad de hipótesis, trazabilidad a datos, claridad de análisis, manejo de falsos positivos, propuesta de regla y severidad. Incluye una checklist de 20 puntos para revisar un 'hunt report'."

---

## 2. ¿Qué es Threat Hunting?

### 2.1. Definición

Threat Hunting es:

> Un proceso proactivo y guiado por hipótesis para buscar actividades maliciosas o anómalas en un entorno, basándose en comportamientos, TTPs y conocimiento del entorno.

Claves:

- Proactivo: no esperas a que el SIEM dispare algo.
- Guiado por hipótesis: no miras datos al azar; defines qué buscas y por qué.
- Iterativo: incluso si no encuentras nada, el hunt sigue siendo valioso:
  - Aprendes "qué es normal".
  - Detectas huecos de visibilidad.
  - Refinas preguntas y consultas.

Lenguaje típico de hunter:

- "Hypothesis-driven"
- "Signals"
- "Baselining"
- "Anomalías"
- "TTPs vs IOCs"
- "Detection engineering"

### 2.2. Diferencias con detección tradicional

- Detección tradicional:
  - Basada en reglas o firmas predefinidas.
  - Reacciona a patrones ya conocidos.
  - Es excelente para cosas repetitivas y medibles.

- Threat Hunting:
  - Explora escenarios que pueden no estar cubiertos por reglas.
  - Basado en TTPs (tácticas, técnicas y procedimientos), no solo en IOCs.
  - Es útil contra adversarios que cambian indicadores y evaden firmas.

Ambas se complementan:

- De un buen hunt pueden salir nuevas reglas y casos de uso.
- De incidentes detectados pueden salir nuevas hipótesis de hunting.
- De falsos positivos pueden salir mejoras de contexto y umbrales.

**Prompt IA:**  
"Explícame Threat Hunting para alguien que viene de SOC. Usa analogías y luego aterriza en 5 ejemplos de hunts típicos: brute force, abuso de PowerShell, escalada de privilegios, persistencia y movimiento lateral. Para cada uno, di qué datos mínimos necesito y qué conclusiones se pueden sacar si no encuentro nada."

---

## 3. Ciclo básico de Threat Hunting

Podemos resumir el flujo en cuatro pasos, pero con detalle práctico:

1. Hipótesis
   - Debe ser concreta y observable.
   - Debe indicar:
     - Qué comportamiento sospechoso esperas.
     - Qué señales lo evidencian.
     - En qué datos lo buscarás.

2. Búsqueda
   - Diseñar consultas o scripts sobre datos:
     - Logs de sistemas.
     - Eventos de PowerShell.
     - Logs web (si tienes).
   - Elegir ventana de tiempo:
     - "últimas 24h", "última semana", "durante el ejercicio".

3. Análisis
   - Interpretar resultados:
     - ¿Es normal en tu entorno?
     - ¿Es un pico aislado o patrón?
     - ¿Qué contexto falta?
   - Profundizar:
     - Enriquecer con usuario, equipo, hora, fuente, correlación.

4. Conclusiones y mejora
   - Si encuentras actividad sospechosa:
     - ¿Es un incidente? ¿Se debe escalar?
     - ¿Qué evidencia guardarías?
     - ¿Cómo convertirlo en detección?
   - Si no encuentras nada:
     - ¿Es porque no ocurre?
     - ¿O porque faltan logs / auditoría?
     - ¿Qué mejorarías de la visibilidad?

Formato mental útil:

- "Hipótesis" define el objetivo.
- "Búsqueda" ejecuta.
- "Análisis" interpreta.
- "Mejora" convierte aprendizaje en capacidad defensiva.

**Prompt IA:**  
"Genera una plantilla Markdown para el ciclo de hunting con secciones: hipótesis, alcance temporal, fuentes de datos, consultas/scripts, resultados, análisis, falsos positivos, gaps de visibilidad, detección propuesta, mapeo MITRE y próximos pasos."

---

## 4. Fuentes de datos en tu laboratorio

Aunque aún no tengas un SIEM real, tienes fuentes suficientes para practicar hunts con mentalidad correcta.

Linux (Ubuntu/Kali):

- "/var/log/auth.log"
  - SSH: intentos fallidos, logins válidos.
  - sudo: elevación, comandos ejecutados con sudo.
- "/var/log/syslog" (según distro)
  - Mensajes de sistema, servicios.
- Logs de servicios (si existen):
  - "/var/log/apache2/access.log" y "error.log"
  - "/var/log/nginx/access.log" y "error.log"

Windows:

- Visor de eventos:
  - Security log (inicios de sesión, cambios de cuenta, etc.)
  - Windows PowerShell / Operational (si habilitado)
- Comandos PowerShell para extraer eventos (cuando sea posible)

Logs de pruebas:

- Copias de auth.log:
  - "auth_log_copia.log"
  - "auth_log_semana8.log"
- Logs "IOC" inventados:
  - "log_ioc_test.log"
- Web logs si montaste Apache/Nginx (Semana 9)

Idea importante:

- Hunting no es solo "buscar algo malo", también es verificar visibilidad.
- Si no tienes eventos de PowerShell, eso en sí es un hallazgo: "gap de logging".

**Prompt IA:**  
"Actúa como arquitecto de logging para un laboratorio. Propón qué logs mínimos habilitar en Linux y Windows para hacer hunts básicos. Incluye qué se gana con cada fuente, qué limitaciones tiene y cómo validar que realmente se están generando eventos."

---

## 5. Diseñar hipótesis de hunting (ejemplos)

Algunos ejemplos sencillos adaptados a tu entorno, pero bien formulados:

1. Hipótesis 1 - SSH brute force o password spraying
   - "Si alguien intenta comprometer SSH, veré picos de 'Failed password' concentrados en ventana corta, desde una o varias IPs, contra uno o varios usuarios."

2. Hipótesis 2 - PowerShell inusual o evasivo
   - "Si hay abuso de PowerShell, veré invocaciones con flags típicos de ocultación o reducción de trazas (por ejemplo '-NoProfile' o ejecución no interactiva), especialmente desde usuarios no esperables."

3. Hipótesis 3 - Uso inusual de sudo
   - "Si un usuario poco privilegiado intenta escalar, veré intentos de sudo fallidos o comandos sensibles ejecutados con sudo en horarios raros."

4. Hipótesis 4 - Login exitoso tras muchos fallos
   - "Si hay intento de adivinación de credenciales, veré secuencias: múltiples fallos y luego un éxito desde misma IP o usuario."

5. Hipótesis 5 - Actividad web anómala en endpoints
   - "Si hay enumeración o abuso web, veré muchas peticiones a rutas inexistentes o patrones repetitivos en access logs."

Esta semana vamos a trabajar especialmente con:

- SSH y autenticaciones en Linux.
- PowerShell en Windows.
- Correlación simple (por ejemplo, fallos seguidos de éxito).

**Prompt IA:**  
"Dame 12 hipótesis de hunting para un laboratorio con Ubuntu, Windows y una web simple en Apache. Para cada hipótesis incluye: descripción, señales observables, fuente de datos y una idea de consulta (sin código ofensivo)."

---

## 6. Laboratorio de la Semana 10

### 6.1. Ejercicio 1 – Definir tus hipótesis de hunting

Crea un fichero:

"07-Threat-Hunting-Semana_10_Hipotesis.md"

Incluye al menos 3 hipótesis adaptadas a tu laboratorio. Por ejemplo:

1. SSH brute force / password spraying en Ubuntu
   - Logs: "/var/log/auth.log" o copias.
   - Señales:
     - Muchas líneas con "Failed password".
     - Mismas IPs contra múltiples usuarios.
     - Muchos intentos contra un mismo usuario.
   - Umbral tentativo:
     - N intentos en X minutos (se ajusta tras ver tu baseline).

2. Abuso de PowerShell en Windows
   - Logs: Event Viewer, "Windows PowerShell" / "Operational".
   - Señales:
     - Uso de "-NoProfile", "-WindowStyle Hidden".
     - Comandos inusuales para un usuario estándar.
   - Nota:
     - Si no hay logs suficientes, documenta el gap.

3. Uso de sudo por usuarios poco frecuentes
   - Logs: "/var/log/auth.log".
   - Señales:
     - sudo utilizado por usuarios que normalmente no lo usan.
     - Intentos fallidos o comandos sensibles.

Para cada hipótesis, añade:

- Qué buscarás.
- En qué fuente de datos.
- Qué ventana temporal usarás.
- Qué resultado consideras sospechoso.
- Qué falsos positivos esperas (y por qué).

**Prompt IA:**  
"Redacta 3 hipótesis de hunting en formato profesional: cada una con objetivo, señales, fuente, ventana temporal, umbral inicial, falsos positivos y riesgos de visibilidad. Que sea apto para pegarlo en 'Hipotesis.md'."

---

### 6.2. Ejercicio 2 – Hunting en Linux: SSH e intentos de login

Vamos a extender lo que ya hiciste en semanas anteriores.

1. Asegúrate de tener un log de autenticación de prueba, por ejemplo:
   - "~/scripts_seguridad/auth_log_semana8.log"
   - O una copia fresca de "/var/log/auth.log"

2. Desde tu máquina Kali, genera algunos intentos de login:
   - Algunos correctos (con usuario de pruebas).
   - Varios fallidos con diferentes usuarios (incluyendo usuarios que no existen).
   - Hazlo de forma controlada y documentada (sabes que es un ejercicio).

3. Copia de nuevo el log actualizado:

   sudo cp /var/log/auth.log ~/scripts_seguridad/auth_log_semana10.log  
   sudo chown "$USER":"$USER" ~/scripts_seguridad/auth_log_semana10.log

4. Crea un script en Python o Bash para:

- Extraer líneas con "Failed password".
- Agrupar por IP origen.
- Contar intentos fallidos por IP.
- Opcional:
  - Agrupar por usuario.
  - Detectar "fallos seguidos de éxito" (si aparece en el log).

Ejemplo de enfoque en Python (para entender la lógica; puedes adaptarlo):

    # hunt_ssh_failed_logins.py

    from collections import defaultdict

    RUTA_LOG = "auth_log_semana10.log"
    conteo_por_ip = defaultdict(int)

    with open(RUTA_LOG, "r", encoding="utf-8", errors="ignore") as f:
        for linea in f:
            if "Failed password" in linea:
                partes = linea.split()
                if "from" in partes:
                    idx = partes.index("from")
                    if idx + 1 < len(partes):
                        ip = partes[idx + 1]
                        conteo_por_ip[ip] += 1

    print("Intentos fallidos de SSH por IP:")
    for ip, cuenta in conteo_por_ip.items():
        print(f"{ip}: {cuenta}")

5. Ejecuta el script y analiza:

   python3 hunt_ssh_failed_logins.py

6. Interpreta resultados:

- ¿Una IP destaca sobre las demás?
- ¿Qué distribución ves (muchas IPs con 1 intento, o una IP con muchos)?
- ¿Qué umbral te parece razonable en tu lab?
- ¿Qué necesitarías en producción para reducir falsos positivos? (whitelists, geolocalización, horarios, usuarios críticos)

Crea:

"07-Threat-Hunting-Semana_10_Hunt1_SSH_Linux.md"

Incluye:

- Hipótesis testeada.
- Fuente de datos y ventana temporal.
- Script o pseudocódigo.
- Salida resumida (tabla o lista).
- Conclusiones:
  - ¿Se confirma la hipótesis?
  - ¿Qué parte es normal y qué parte sería anómala?
  - ¿Cómo lo convertirías en una detección (regla)?

**Prompt IA:**  
"Actúa como analista SOC. A partir de un extracto de auth.log y del conteo por IP, redacta un 'hunt report' en Markdown: resumen ejecutivo, resultados, análisis de falsos positivos y detección propuesta con umbral. Incluye un mapeo MITRE de alto nivel."

---

### 6.3. Ejercicio 3 – Hunting en Windows: PowerShell inusual

Ahora toca Windows.

1. Genera ejecuciones de PowerShell "normales":

   Get-Process  
   Get-Service

2. Después, genera 2-3 ejecuciones "más sospechosas" de forma controlada para tener señales que buscar (sin daño):

   powershell.exe -NoProfile -WindowStyle Hidden -Command "Write-Output 'Test Hidden 1'"  
   powershell.exe -NoProfile -Command "Write-Output 'Test NoProfile 1'"

3. Abre el Visor de eventos y confirma que hay eventos de PowerShell registrando estas actividades.
   - Si no aparecen, documenta:
     - Dónde buscaste.
     - Qué logs no están habilitados.
     - Qué harías en un entorno real (habilitar logging, políticas, etc.).

4. Crea un script PowerShell para buscar eventos que contengan "NoProfile" o "WindowStyle Hidden":

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

5. Ejecuta el script:

   .\Hunt_PowerShell_Sospechoso.ps1

6. Analiza:

- ¿Qué eventos salen?
- ¿Corresponden a tus comandos de prueba?
- ¿Qué campos te faltan para hacerlo más robusto? (usuario, proceso padre, línea de comandos completa, etc.)
- ¿Qué falsos positivos esperas? (admins, scripts corporativos, tareas programadas)

Crea:

"07-Threat-Hunting-Semana_10_Hunt2_PowerShell_Windows.md"

Incluye:

- Hipótesis probada.
- Fuente de datos (qué log exacto usaste).
- Script PowerShell.
- Ejemplo de eventos encontrados (recortado).
- Conclusión:
  - ¿Este patrón sería sospechoso en producción?
  - ¿Qué contexto pedirías antes de escalar?

**Prompt IA:**  
"Actúa como threat hunter. Dado un listado de eventos de PowerShell filtrados por 'NoProfile' y 'Hidden', clasifica los hallazgos en: probablemente benigno, necesita contexto, sospechoso. Define qué contexto faltaría para cada caso y propone una regla con condiciones y exclusiones."

---

### 6.4. Ejercicio 4 – De hunting a detección: diseñar "reglas"

Ahora convierte los resultados de tus hunts en ideas de reglas de detección, como si fueras a implementarlas en un SIEM.

Crea:

"07-Threat-Hunting-Semana_10_Reglas_Deteccion.md"

Para cada hunt (SSH y PowerShell), describe:

1. Condición de detección (ejemplos):

- SSH:
  - "Disparar alerta si se observan más de 5 intentos fallidos de login SSH desde la misma IP en 10 minutos."
  - Variante:
    - "Disparar alerta si una IP falla contra 5 usuarios distintos en 10 minutos" (spraying)

- PowerShell:
  - "Disparar alerta si se ejecuta PowerShell con '-NoProfile' y '-WindowStyle Hidden' desde un usuario no administrador."
  - Variante:
    - "Disparar alerta si aparece PowerShell con flags de ocultación fuera de horas laborales" (si aplica)

2. Define:

- Campos necesarios:
  - IP origen, usuario, timestamp, mensaje, host, proceso, línea de comandos (si disponible).
- Umbral / frecuencia:
  - N veces en X minutos.
- Severidad:
  - Alta / Media / Baja.
- Respuesta sugerida:
  - Qué haría un N1 vs N2.
- Falsos positivos esperables:
  - Qué escenarios legítimos podrían disparar la regla.

3. Opcional:

- Relación con MITRE ATT&CK (alto nivel):
  - PowerShell sospechoso → Execution (T1059.001).
  - SSH brute force → Credential Access o Initial Access (según contexto).

**Prompt IA:**  
"Actúa como ingeniero de detección. Convierte dos hunts (SSH fallidos y PowerShell con flags) en reglas de detección bien definidas. Para cada regla, incluye: lógica, umbrales, campos requeridos, exclusiones, severidad, pasos de triage N1 y mapeo MITRE."

---

### 6.5. Ejercicio 5 – Resumen del hunt: mini informe

Finalmente, crea un informe corto:

"07-Threat-Hunting-Semana_10_Informe_Hunting.md"

Estructura sugerida:

1. Introducción
- Objetivo: practicar Threat Hunting en entorno de laboratorio sobre SSH y PowerShell.
- Alcance temporal y sistemas incluidos.

2. Hipótesis
- Lista de hipótesis investigadas y por qué.

3. Metodología
- Fuentes de datos:
  - auth.log
  - eventos de PowerShell
- Scripts utilizados y enfoque de agregación.
- Limitaciones (por ejemplo, falta de ciertos logs).

4. Hallazgos
- Patrones observados.
- Qué consideras benigno en tu lab.
- Qué sería alarmante en producción.

5. Detecciones propuestas
- Resumen de reglas que propones.
- Umbrales tentativos.
- Ideas de mejora (enriquecimiento, listas permitidas, correlación con éxitos, etc.)

6. Próximos pasos
- Qué hunts harías después:
  - sudo inusual en Linux.
  - Login exitoso tras muchos fallos.
  - Actividad web anómala en access logs.
  - Correlación simple entre eventos Linux y Windows (misma IP interna, misma ventana temporal).

**Prompt IA:**  
"Redacta un mini informe de Threat Hunting en Markdown (1-2 páginas) a partir de: hipótesis, resultados resumidos de SSH y PowerShell, limitaciones y reglas propuestas. Debe parecer un informe profesional de SOC/Blue Team. Incluye un apartado 'gaps de visibilidad' y 'acciones recomendadas'."

---

## 7. Entregables de la Semana 10

Al finalizar la semana deberías tener:

1. "07-Threat-Hunting-Semana_10_Hipotesis.md"
- Lista de hipótesis adaptadas al lab, con señales y fuentes.

2. "07-Threat-Hunting-Semana_10_Hunt1_SSH_Linux.md"
- Búsqueda sobre intentos de login SSH.
- Agregación por IP (y opcionalmente por usuario).
- Resultados y conclusiones.

3. "07-Threat-Hunting-Semana_10_Hunt2_PowerShell_Windows.md"
- Búsqueda sobre actividad inusual de PowerShell.
- Resultados, contexto y conclusiones.

4. "07-Threat-Hunting-Semana_10_Reglas_Deteccion.md"
- Traducción de hunts a reglas de detección:
  - lógica
  - umbrales
  - falsos positivos
  - severidad
  - triage sugerido

5. "07-Threat-Hunting-Semana_10_Informe_Hunting.md"
- Informe breve con hipótesis, metodología, hallazgos y propuestas.

Opcional:

- Ampliar "Lab_Setup_Report.md" con:
  - "Capacidades de Threat Hunting"
  - qué logs tienes
  - qué hunts puedes repetir periódicamente

**Prompt IA:**  
"Actúa como revisor de repositorio. Propón una estructura de carpetas para almacenar hunts y evidencias (logs, scripts, outputs) sin mezclarlo con otras semanas. Incluye convenciones de nombres y un ejemplo de índice README en Markdown."

---

## 8. Checklist de cierre de la Semana 10

Marca como completado cuando:

- [ ] Puedes explicar con tus palabras qué es Threat Hunting y en qué se diferencia de otras funciones del SOC.
- [ ] Has definido al menos 3 hipótesis de hunting realistas para tu laboratorio.
- [ ] Has ejecutado un hunt sobre logs de SSH en Linux y has interpretado resultados.
- [ ] Has ejecutado un hunt sobre eventos de PowerShell en Windows y has identificado actividad "sospechosa" de prueba.
- [ ] Has convertido resultados de tus hunts en ideas de reglas de detección con umbrales y severidad.
- [ ] Has escrito un mini informe de Threat Hunting, aunque sea sobre datos simulados.
- [ ] Tienes todos los ficheros de la Semana 10 en tu repositorio y entiendes su contenido.
- [ ] Has documentado al menos un "gap de visibilidad" y cómo lo resolverías.

Si has llegado aquí, ya no solo sabes responder a eventos, sino también salir a buscarlos.  
Esa diferencia separa un perfil puramente reactivo de uno más avanzado (Blue/Purple Team) con mentalidad de Threat Hunter.

En la siguiente semana podremos orientar el contenido hacia:

- Profundizar en la rama SOC (más detecciones, casos de uso, reporting).
- Reforzar la parte ofensiva (pentesting más técnico).
- O combinar ambos en escenarios Purple Team con correlaciones más completas.

**Prompt IA:**  
"Actúa como examinador final. Genera un checklist de verificación de Semana 10 para revisar el repo: qué scripts deben existir, qué outputs mínimos, qué conclusiones y qué reglas propuestas. Incluye 10 preguntas de reflexión sobre umbrales, falsos positivos y visibilidad."

---

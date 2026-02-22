# Semana 11 – Enfoque Purple Team y MITRE ATT&CK aplicado 🟣🧩

## 0. Introducción a la semana

Hasta ahora has visto, bastante separados, dos mundos:

- **Red Team / Hacking ético** (Semana 7 y 9):
  - Escaneo, enumeración, conceptos de explotación.
  - Seguridad web básica (OWASP), XSS, uso de Burp, etc.

- **Blue Team / SOC / Hunting** (Semana 8 y 10):
  - Eventos, alertas, incidentes.
  - SIEM, IOCs, triage, threat hunting sobre SSH y PowerShell.

En la realidad de muchos equipos modernos se busca algo más integrado:

> Enfoque **Purple Team** = colaboración directa entre ofensiva y defensiva.

La idea no es que existan dos bandos enfrentados, sino que:

- Los que atacan (simulan al atacante) ayudan a entender brechas y carencias.
- Los que defienden utilizan ese conocimiento para mejorar detecciones y respuesta.
- Todo se apoya en un lenguaje común, y ahí entra **MITRE ATT&CK**.

En esta Semana 11 vamos a:

- Entender qué es un ejercicio Purple Team.
- Usar la matriz **MITRE ATT&CK** como mapa de técnicas.
- Tomar un ataque sencillo en tu laboratorio (SSH, PowerShell o web) y:
  - Mapearlo a ATT&CK.
  - Ver qué logs genera.
  - Proponer detecciones y mejoras.
- Documentar el ejercicio como si fuera una colaboración Red/Blue.

---

## 1. Objetivos de aprendizaje de la Semana 11

Al finalizar esta semana deberías ser capaz de:

1. Explicar qué es el enfoque **Purple Team** y por qué es útil.
2. Describir, a alto nivel, qué es la matriz **MITRE ATT&CK** y cómo se usa.
3. Elegir un escenario de ataque sencillo en tu laboratorio (SSH, PowerShell, web) y:
   - Descomponerlo en pasos/técnicas.
   - Mapear esos pasos a tácticas/técnicas de MITRE (de forma razonable).
4. Identificar qué **evidencias** genera cada paso en los logs (Linux, Windows, web).
5. Diseñar **detecciones** básicas por cada fase (reglas de SIEM, alarmas, scripts).
6. Escribir un pequeño informe de ejercicio Purple Team:
   - Qué hizo “Red”.
   - Qué vio “Blue”.
   - Qué se ha mejorado gracias al ejercicio.

---

## 2. ¿Qué es el enfoque Purple Team?

### 2.1. Red + Blue trabajando juntos

Tradicionalmente:

- El Red Team lanza ataques simulados.
- El Blue Team defiende y responde.
- A veces se genera fricción (“nos habéis pillado / no nos habéis pillado”).

El enfoque **Purple Team** propone:

- Diseñar ejercicios donde Red y Blue colaboran **desde el principio**.
- Que Red ejecute pasos concretos, mientras Blue:
  - Observa qué se ve (o no) en los logs.
  - Ajusta detecciones.
  - Aprende cómo se materializan las técnicas de ataque.

Ventajas:

- Mejora la calidad de detecciones.
- Alinea a los equipos alrededor de los mismos escenarios.
- Acelera el aprendizaje (sobre todo para juniors).

### 2.2. Escenarios controlados y repetibles

En un ejercicio Purple bien hecho:

- Se define un **escenario**:
  - Ej: “Intento de fuerza bruta SSH + acceso posterior con credenciales débiles”.
  - Ej: “Uso malicioso de PowerShell para descargar y ejecutar código”.
  - Ej: “XSS para robo de cookies”.

- Se documentan los pasos ofensivos.
- Se anotan evidencias generadas (logs, alertas, eventos).
- Se diseñan o ajustan reglas de detección.

La idea es que el ejercicio sea **repetible** en el futuro para comprobar:

- Si los controles y detecciones siguen funcionando.
- Si alguna actualización rompió algo.

---

## 3. MITRE ATT&CK como lenguaje común

### 3.1. ¿Qué es MITRE ATT&CK?

MITRE ATT&CK es una **matriz de tácticas y técnicas** utilizada para describir:

- **Tácticas** → El “por qué” / objetivo de alto nivel del atacante (ej. Execution, Persistence, Lateral Movement…).
- **Técnicas** → El “cómo” concreto (ej. PowerShell, valid accounts, SSH, Scheduled Tasks…).

Se puede usar para:

- Describir campañas reales de amenazas.
- Diseñar ejercicios Red Team / Purple Team.
- Mapear reglas de detección a técnicas específicas.
- Evaluar la cobertura de seguridad (qué técnicas detectas y cuáles no).

### 3.2. No hace falta memorizar la matriz

En este curso:

- No necesitas conocer todos los IDs (Txxxx) de memoria.
- Lo importante es entender:
  - Que existe un catálogo de técnicas.
  - Que puedes decir “este paso corresponde a Execution (PowerShell)”.
  - Que puedes mapear tu entorno a esas técnicas.

Ejemplos típicos para tus ejercicios:

- **Uso malicioso de PowerShell**  
  → Táctica: Execution.  
  → Técnica típica: “Command and Scripting Interpreter: PowerShell”.

- **SSH brute force / intentos masivos de login**  
  → Tácticas relacionadas: Credential Access / Initial Access (según el escenario).

- **Uso de cuentas válidas tras robo de credenciales**  
  → Táctica: Lateral Movement / Persistence / Defense Evasion, etc.

---

## 4. Elegir un escenario Purple Team en tu laboratorio

Para esta semana, vas a escoger **un escenario sencillo**, basado en lo que ya has hecho.

Propuestas:

1. **Escenario A – SSH brute force + login exitoso en Ubuntu**  
   - Red:
     - Escanear puertos.
     - Intentar múltiples logins SSH (fallidos).
     - Usar credenciales débiles hasta conseguir un login correcto.
   - Blue:
     - Buscar picos de `Failed password`.
     - Detectar login exitoso posterior desde la misma IP.
   - MITRE:
     - Credential Access / Brute Force.
     - Valid Accounts.
     - Remote Services.

2. **Escenario B – PowerShell “malicioso” en Windows**  
   - Red:
     - Ejecutar PowerShell con flags raros: `-NoProfile`, `-WindowStyle Hidden`.
     - Incluir pseudo-descarga o ejecución simulada.
   - Blue:
     - Revisar eventos de PowerShell.
     - Crear detección basada en esos parámetros.
   - MITRE:
     - Execution / Command and Scripting Interpreter: PowerShell.
     - Possibly Defense Evasion si se oculta ventana.

3. **Escenario C – XSS simple en aplicación de prueba**  
   - Red:
     - Inyectar `<script>` en un campo de la web de test.
   - Blue:
     - Revisar logs web.
     - Pensar detecciones (WAF, logs de aplicación).
   - MITRE:
     - Impact / Collection / Credential Access (si roba cookies, etc.).

Te recomiendo elegir **A o B**, porque se apoyan muy bien en los logs que ya has trabajado (auth.log y eventos de PowerShell).

---

## 5. Laboratorio de la Semana 11

### 5.1. Ejercicio 1 – Definir el escenario Purple Team

Crea:

`08-PurpleTeam-MITRE-Semana_11_Escenario.md`

Incluye:

1. Escenario elegido (A, B o C, o uno mixto).
2. Rol Red Team:
   - Qué pasos ofensivos va a ejecutar (concretos, en orden).
3. Rol Blue Team:
   - Qué fuentes de datos va a mirar (auth.log, eventos de PowerShell, logs web, etc.).
   - Qué detecciones quiere evaluar o crear.
4. Objetivo del ejercicio:
   - Ej: “Validar si podemos detectar un intento de fuerza bruta SSH seguido de un login exitoso”.
   - Ej: “Validar si somos capaces de identificar el abuso de PowerShell con flags sospechosos”.

Piensa en esto como el “guion” del ejercicio.

---

### 5.2. Ejercicio 2 – Ejecución del escenario (lado Red)

Sigue los pasos definidos.

#### Si elegiste Escenario A – SSH en Ubuntu:

1. Desde Kali, ejecuta un escaneo básico para localizar el puerto 22:
   ```bash
   nmap -sV IP_DE_UBUNTU
   ```
2. Lanza varios intentos de login fallidos:
   ```bash
   ssh usuario1@IP_DE_UBUNTU   # con password incorrecta
   ```
   Repite varias veces con el mismo usuario y, si quieres, con otros usuarios.
3. Finalmente, haz un login exitoso con credencial correcta (para tu usuario de pruebas).

#### Si elegiste Escenario B – PowerShell en Windows:

1. Lanza algunas ejecuciones “normales” de PowerShell (Get-Process, Get-Service).
2. Después, ejecuta comandos más “raros”, por ejemplo:
   ```powershell
   powershell.exe -NoProfile -WindowStyle Hidden -Command "Write-Output 'PurpleTeamTest1'"
   powershell.exe -NoProfile -Command "Invoke-Expression 'Get-Date'"
   ```
3. Opcional: simula un download benigno (sin malware) para ver cómo quedaría el patrón, siempre en entorno controlado.

#### Si elegiste Escenario C – XSS:

1. Usa tu formulario de `test_form.php`.
2. Inyecta `<script>alert('PurpleTeamXSS');</script>` en un campo.
3. Observa el comportamiento desde “víctima”.

En tu fichero:

`08-PurpleTeam-MITRE-Semana_11_RedTeam_Pasos.md`

Describe:

- Comandos usados.
- Secuencia de acciones.
- Pequeñas notas de lo que observas desde la perspectiva ofensiva.

---

### 5.3. Ejercicio 3 – Observación de evidencias (lado Blue)

Ahora te pones el gorro Blue Team.

#### Para Escenario A – SSH

En tu Ubuntu:

1. Copia el log de autenticación:

   ```bash
   sudo cp /var/log/auth.log ~/scripts_seguridad/auth_log_semana11.log
   sudo chown "$USER":"$USER" ~/scripts_seguridad/auth_log_semana11.log
   ```

2. Usa Python/Bash (puedes reutilizar scripts de la Semana 10) para:

   - Listar intentos `Failed password` por IP.
   - Localizar el login `Accepted password` posterior desde la misma IP.

3. Intenta construir un mini timeline:

   - Hora de primeros fallos.
   - Hora del login exitoso.
   - IP origen.

#### Para Escenario B – PowerShell

En Windows:

1. Abre Event Viewer y revisa:
   - Registros de `Windows PowerShell` / `Microsoft-Windows-PowerShell/Operational`.
2. Fíjate en:
   - Eventos que contengan `-NoProfile`.
   - Eventos con `WindowStyle Hidden`.
3. Puedes usar de nuevo tu script de hunting de Semana 10 o crear uno parecido.

#### Para Escenario C – XSS

- Revisa logs de Apache/Nginx:
  - `/var/log/apache2/access.log` o equivalente.
- Busca peticiones donde el parámetro contenga `<script>` o una cadena identificable.
- Piensa qué registraría la aplicación.

Crea:

`08-PurpleTeam-MITRE-Semana_11_BlueTeam_Evidencias.md`

Incluye:

- Fragmentos de log relevantes.
- Explicación (con tus palabras) de qué está pasando.
- Pequeña línea temporal de los eventos importantes.

---

### 5.4. Ejercicio 4 – Mapeo a MITRE ATT&CK

Ahora vas a usar MITRE ATT&CK como “lenguaje común”.

No hace falta que te obsesiones con el ID exacto (Txxxx), pero sí con:

- Táctica (qué objetivo buscaba el atacante en ese paso).
- Técnica (forma general de cómo lo ha hecho).

Crea:

`08-PurpleTeam-MITRE-Semana_11_Mapeo_MITRE.md`

Para tu escenario:

1. Lista los pasos ofensivos que ejecutaste (desde el fichero de Red Team).
2. Para cada paso, indica:

   - **Descripción del paso**  
     Ej: “Múltiples intentos de autenticación SSH con credenciales incorrectas.”

   - **Táctica ATT&CK (nivel alto)**  
     Ej: Credential Access / Initial Access.

   - **Técnica (descripción general)**  
     Ej: “Password Brute Force contra servicio SSH.”

   - **Evidencias / logs asociados**  
     Ej: Entradas `Failed password` en `/var/log/auth.log`.

Ejemplo simplificado (texto):

- Paso 1: Descubrir puerto SSH en Ubuntu  
  - Táctica: Discovery.  
  - Técnica: Port Scanning.  
  - Evidencias: Logs de firewall / posibles registros en IDS (si existiera), en tu lab: salida de nmap, posible rastro en logs si estuviera configurado.

- Paso 2: Fuerza bruta SSH  
  - Táctica: Credential Access / Initial Access.  
  - Técnica: Brute Force sobre protocolo SSH.  
  - Evidencias: `Failed password` repetidos en auth.log.

- Paso 3: Login exitoso tras múltiples fallos  
  - Táctica: Valid Accounts / Lateral Movement (según contexto).  
  - Técnica: Uso de cuentas válidas.  
  - Evidencias: `Accepted password` desde la misma IP.

---

### 5.5. Ejercicio 5 – De MITRE a detecciones y mejoras

Ahora conecta todo:

Crea:

`08-PurpleTeam-MITRE-Semana_11_Detecciones_Mejoras.md`

Para cada paso mapeado a MITRE:

1. Define una **detección** potencial:

   - Condición:
     - Ej: “Más de 5 `Failed password` en 10 minutos desde una misma IP”.
   - Fuente de datos:
     - `/var/log/auth.log`, eventos de PowerShell, logs web, etc.
   - Tecnología (en un entorno real):
     - SIEM, EDR, WAF, script de monitorización.

2. Propón **mejoras defensivas**:

   - A nivel técnico:
     - Limitar intentos de login.
     - Añadir MFA.
     - Restricciones de IP para SSH / RDP.
     - Logging avanzado.
   - A nivel de proceso:
     - Procedimientos de respuesta.
     - Playbooks.
     - Alertas en el SOC.

El objetivo es ver el ciclo completo:

> Técnica ATT&CK → Evidencias → Regla de detección → Mitigaciones.

---

### 5.6. Ejercicio 6 – Informe Purple Team

Por último, junta todo en un informe corto de ejercicio Purple:

`08-PurpleTeam-MITRE-Semana_11_Informe_PurpleTeam.md`

Estructura sugerida:

1. **Introducción**
   - Objetivo del ejercicio (ej. “Ejercicio Purple Team básico: SSH brute force + login exitoso”).

2. **Escenario**
   - Resumen del entorno (Kali, Ubuntu, Windows).
   - Alcance del ejercicio.

3. **Perspectiva Red Team**
   - Pasos ofensivos realizados.
   - Objetivo (qué se intentaba conseguir).

4. **Perspectiva Blue Team**
   - Logs revisados.
   - Evidencias encontradas.
   - Detecciones actuales (scripts, búsquedas manuales).

5. **Mapeo MITRE ATT&CK**
   - Tabla/resumen de tácticas/técnicas implicadas.

6. **Detecciones y recomendaciones**
   - Qué reglas propones.
   - Qué controles técnicos/procedimentales mejorarías.

7. **Lecciones aprendidas**
   - Qué has entendido mejor sobre ataque y defensa.
   - Qué te gustaría mejorar en futuros ejercicios.

---

## 6. Entregables de la Semana 11

Al terminar esta semana, deberías tener:

1. `08-PurpleTeam-MITRE-Semana_11_Escenario.md`  
   - Descripción del ejercicio Purple Team (escenario elegido).

2. `08-PurpleTeam-MITRE-Semana_11_RedTeam_Pasos.md`  
   - Detalle de los pasos ejecutados como “atacante”.

3. `08-PurpleTeam-MITRE-Semana_11_BlueTeam_Evidencias.md`  
   - Logs y evidencias observadas, con explicación.

4. `08-PurpleTeam-MITRE-Semana_11_Mapeo_MITRE.md`  
   - Relación pasos ↔ tácticas/técnicas ATT&CK ↔ evidencias.

5. `08-PurpleTeam-MITRE-Semana_11_Detecciones_Mejoras.md`  
   - Detecciones propuestas y mejoras defensivas.

6. `08-PurpleTeam-MITRE-Semana_11_Informe_PurpleTeam.md`  
   - Informe final del ejercicio Purple Team.

---

## 7. Checklist de cierre de la Semana 11

Marca como completado cuando:

- [ ] Puedes explicar qué es un ejercicio Purple Team y cómo se diferencia de un pentest clásico o de detección reactiva.
- [ ] Has definido un escenario de laboratorio que combine acciones ofensivas y análisis defensivo.
- [ ] Has ejecutado los pasos ofensivos (SSH, PowerShell o web) en tu entorno de pruebas.
- [ ] Has recopilado y entendido las evidencias generadas en los logs.
- [ ] Has mapeado cada paso a tácticas y técnicas de MITRE ATT&CK de forma razonable.
- [ ] Has diseñado detecciones y mejoras defensivas basadas en ese escenario.
- [ ] Has escrito un informe Purple Team que conecte Red, Blue y MITRE ATT&CK.
- [ ] Has añadido todos los ficheros de la Semana 11 a tu repositorio.

Si has llegado hasta aquí, ya no ves ataque y defensa como mundos separados, sino como partes de un mismo ciclo de mejora continua, con un lenguaje común (MITRE ATT&CK) y ejercicios concretos (Purple Team) para subir el nivel técnico y de detección.

En la **Semana 12** podemos cerrar el plan con:

- Repaso global.
- Proyecto final integrador.
- Y una parte clara de **preparación para empleo real**: portfolio, cómo contar tu lab en entrevistas, cómo convertir estos `.md` en ventaja frente a otros juniors.

---

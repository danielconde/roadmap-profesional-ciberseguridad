# Semana 11 – Enfoque Purple Team y MITRE ATT&CK aplicado 🟣🧩

## 0. Introducción a la semana

Hasta ahora has visto, bastante separados, dos mundos:

- Red Team / Hacking ético (Semana 7 y 9):
  - Escaneo, enumeración, conceptos de explotación.
  - Seguridad web básica (OWASP), XSS, uso de Burp, etc.

- Blue Team / SOC / Hunting (Semana 8 y 10):
  - Eventos, alertas, incidentes.
  - SIEM, IOCs, triage, threat hunting sobre SSH y PowerShell.

En la realidad de muchos equipos modernos se busca algo más integrado:

- Enfoque Purple Team = colaboración directa entre ofensiva y defensiva.

La idea no es que existan dos bandos enfrentados, sino que:

- Los que atacan (simulan al atacante) ayudan a entender brechas y carencias.
- Los que defienden utilizan ese conocimiento para mejorar detecciones y respuesta.
- Todo se apoya en un lenguaje común, y ahí entra MITRE ATT&CK.

En esta Semana 11 vamos a:

- Entender qué es un ejercicio Purple Team y cómo se diseña para que sea útil.
- Usar la matriz MITRE ATT&CK como mapa de técnicas y como forma de hablar el mismo idioma.
- Tomar un ataque sencillo en tu laboratorio (SSH, PowerShell o web) y:
  - Descomponerlo en pasos claros.
  - Mapearlo a ATT&CK.
  - Revisar qué evidencia deja en logs.
  - Proponer detecciones y mejoras defensivas.
- Documentar el ejercicio como si fuera una colaboración Red/Blue.

Una diferencia importante frente a un pentest clásico:

- En Purple Team no basta con "he conseguido acceso".
- La meta es "he generado señales" y "hemos mejorado detecciones y respuesta".

**Prompt IA:**  
"Actúa como coordinador Purple Team. Explica en términos sencillos la diferencia entre Red Team, Blue Team y Purple Team. Luego define un ejercicio Purple Team de 1 hora para un laboratorio con Kali, Ubuntu y Windows: objetivos, alcance, pasos del atacante, qué debe observar el defensor, evidencias esperadas y entregables finales."

---

## 1. Objetivos de aprendizaje de la Semana 11

Al finalizar esta semana deberías ser capaz de:

1. Explicar qué es el enfoque Purple Team y por qué es útil para elevar detección y respuesta.
2. Describir, a alto nivel, qué es la matriz MITRE ATT&CK y cómo se usa como lenguaje común.
3. Elegir un escenario de ataque sencillo en tu laboratorio (SSH, PowerShell, web) y:
   - Descomponerlo en pasos y técnicas.
   - Mapear esos pasos a tácticas y técnicas de MITRE (de forma razonable).
4. Identificar qué evidencias genera cada paso en los logs (Linux, Windows, web).
5. Diseñar detecciones básicas por cada fase:
   - reglas de SIEM (conceptuales)
   - búsquedas
   - scripts de agregación
6. Escribir un informe de ejercicio Purple Team:
   - Qué hizo Red.
   - Qué vio Blue.
   - Qué se ha mejorado gracias al ejercicio.
   - Qué gaps de visibilidad han aparecido.

**Prompt IA:**  
"Genera una lista de objetivos de aprendizaje para una semana Purple Team nivel junior, con criterios medibles. Incluye ejemplos de evidencias y una forma clara de validar si el objetivo se cumple en un laboratorio."

---

## 2. ¿Qué es el enfoque Purple Team?

### 2.1. Red + Blue trabajando juntos

Tradicionalmente:

- El Red Team lanza ataques simulados.
- El Blue Team defiende y responde.
- A veces se genera fricción: métricas mal entendidas, expectativas distintas, falta de feedback accionable.

El enfoque Purple Team propone:

- Diseñar ejercicios donde Red y Blue colaboran desde el principio.
- Que Red ejecute pasos concretos, mientras Blue:
  - Observa qué se ve (o no) en los logs.
  - Ajusta detecciones y umbrales.
  - Construye playbooks y criterios de triage.
  - Valida si los controles y la visibilidad son suficientes.

Ventajas:

- Mejora la calidad y la cobertura de detecciones.
- Reduce el tiempo entre "ataque simulado" y "mejora defensiva aplicada".
- Acelera el aprendizaje, especialmente en perfiles junior:
  - se entiende qué señal produce cada técnica
  - se aprende a traducir acciones ofensivas a telemetría y reglas

### 2.2. Escenarios controlados y repetibles

En un ejercicio Purple bien hecho:

- Se define un escenario:
  - "Intento de fuerza bruta SSH + acceso posterior con credenciales débiles"
  - "Uso inusual de PowerShell para ejecutar acciones no habituales"
  - "XSS para demostrar riesgo de manipulación de entrada"
- Se documentan pasos ofensivos.
- Se anotan evidencias generadas (logs, eventos).
- Se diseñan o ajustan reglas de detección.

La idea es que el ejercicio sea repetible para comprobar:

- Si las detecciones siguen funcionando con el tiempo.
- Si cambios en configuración o versiones alteran la telemetría.
- Si el equipo puede repetir el triage de forma consistente.

**Prompt IA:**  
"Dame una plantilla de ejercicio Purple Team repetible. Incluye: pre-requisitos, checklist previo, pasos ofensivos, pasos defensivos, evidencias esperadas, criterios de éxito, falsos positivos esperables, y un apartado 'si no veo nada, qué revisar'."

---

## 3. MITRE ATT&CK como lenguaje común

### 3.1. ¿Qué es MITRE ATT&CK?

MITRE ATT&CK es una base de conocimiento de tácticas y técnicas utilizada para describir:

- Tácticas: el objetivo de alto nivel del atacante (por qué).
- Técnicas: el método concreto (cómo).

Se puede usar para:

- Describir campañas reales.
- Diseñar ejercicios Red Team / Purple Team.
- Mapear reglas de detección a técnicas específicas.
- Evaluar cobertura: qué técnicas detectas, cuáles solo registras y cuáles no ves.

### 3.2. No hace falta memorizar la matriz

En este curso:

- No necesitas memorizar IDs (Txxxx) de memoria.
- Lo importante es:
  - Saber navegar la matriz y buscar técnicas relevantes.
  - Explicar con lenguaje claro el mapeo:
    - "esto es Execution mediante PowerShell"
    - "esto es Credential Access con intentos de fuerza bruta"
  - Relacionar técnicas con evidencias en logs.

Ejemplos típicos para tus ejercicios:

- Uso inusual de PowerShell
  - Táctica: Execution
  - Técnica: Command and Scripting Interpreter: PowerShell

- SSH brute force
  - Táctica: Credential Access (y a veces Initial Access según objetivo)
  - Técnica: Brute Force (password guessing / spraying)

- Uso de cuentas válidas tras obtener credenciales
  - Táctica: Initial Access o Lateral Movement (según escenario)
  - Técnica: Valid Accounts
  - Técnica adicional común: Remote Services

**Prompt IA:**  
"Actúa como instructor MITRE. Explícame cómo mapear un escenario paso a paso a tácticas y técnicas sin memorizar IDs. Usa un ejemplo con SSH y otro con PowerShell. Para cada paso indica qué evidencia esperaría ver en logs de Linux o Windows."

---

## 4. Elegir un escenario Purple Team en tu laboratorio

Para esta semana, vas a escoger un escenario sencillo y medible. Debe cumplir:

- Repetible: lo puedes ejecutar varias veces.
- Observable: deja evidencia en logs.
- Controlado: sin acciones destructivas ni fuera del laboratorio.

Propuestas:

1. Escenario A - SSH brute force + login exitoso en Ubuntu
   - Red:
     - Localizar el servicio.
     - Generar intentos fallidos.
     - Realizar un login exitoso con credenciales válidas de prueba.
   - Blue:
     - Identificar picos de "Failed password".
     - Detectar un "Accepted password" posterior desde la misma IP.
     - Construir timeline.
   - MITRE (alto nivel):
     - Brute Force
     - Valid Accounts
     - Remote Services

2. Escenario B - PowerShell "malicioso" controlado en Windows
   - Red:
     - Ejecutar PowerShell con flags raros: "-NoProfile", "-WindowStyle Hidden".
     - Ejecutar acciones benignas pero con forma sospechosa (para generar señales).
   - Blue:
     - Revisar eventos de PowerShell.
     - Detectar flags sospechosos y evaluar falsos positivos.
   - MITRE (alto nivel):
     - Execution mediante PowerShell
     - Defense Evasion (si se oculta ventana o se reduce trazabilidad)

3. Escenario C - XSS simple en aplicación de prueba
   - Red:
     - Inyectar "<script>" en campo de prueba.
   - Blue:
     - Revisar logs web.
     - Proponer controles (validación, output encoding, WAF).
   - MITRE (alto nivel):
     - Dependerá del impacto (Collection / Credential Access si se roba sesión, etc.)

Recomendación de aprendizaje:

- Elige A o B si quieres reforzar SOC y hunting con logs claros.
- Elige C si quieres reforzar visión web y OWASP con enfoque defensivo.

**Prompt IA:**  
"Recomiéndame un escenario Purple Team para un laboratorio con Kali, Ubuntu y Windows, priorizando evidencias claras en logs. Dame 3 opciones con pros, contras, evidencias esperadas y dificultad. Termina con una recomendación final."

---

## 5. Laboratorio de la Semana 11

### 5.1. Ejercicio 1 – Definir el escenario Purple Team

Crea:

`08-PurpleTeam-MITRE-Semana_11_Escenario.md`

Incluye:

1. Escenario elegido (A, B o C, o uno mixto).
2. Rol Red Team:
   - Pasos ofensivos en orden.
   - Qué pretende demostrar cada paso.
3. Rol Blue Team:
   - Fuentes de datos a revisar.
   - Qué preguntas responderá (triage).
   - Qué detecciones quiere evaluar o crear.
4. Objetivo del ejercicio:
   - Qué se considera éxito (criterios medibles).
5. Ventana temporal:
   - En qué franja harás el ejercicio para poder filtrar logs.

**Prompt IA:**  
"Genera el contenido completo en Markdown para 'Escenario.md' de un ejercicio Purple Team. Debe incluir: objetivo, alcance, roles, pasos, criterios de éxito, fuentes de datos, riesgos y checklist previo."

---

### 5.2. Ejercicio 2 – Ejecución del escenario (lado Red)

Sigue los pasos definidos.

Si elegiste Escenario A - SSH en Ubuntu:

1. Desde Kali, verifica exposición del servicio:

   nmap -sV IP_DE_UBUNTU

2. Genera intentos fallidos controlados:

   ssh usuario1@IP_DE_UBUNTU

   - Introduce contraseña incorrecta varias veces.
   - Opcional: repite con un usuario inexistente para ver diferencias de logging.

3. Realiza un login exitoso con credenciales válidas de prueba.

Si elegiste Escenario B - PowerShell en Windows:

1. Ejecuta comandos normales para baseline:

   Get-Process  
   Get-Service

2. Ejecuta comandos con forma sospechosa pero benignos:

   powershell.exe -NoProfile -WindowStyle Hidden -Command "Write-Output 'PurpleTeamTest1'"  
   powershell.exe -NoProfile -Command "Write-Output 'PurpleTeamTest2'"

Si elegiste Escenario C - XSS:

1. Usa tu formulario de prueba.
2. Envía un payload de prueba para demostrar riesgo de output no sanitizado:
   - Si tu página vuelve a ser vulnerable a propósito solo durante el ejercicio, documenta que luego revertirás el cambio.

En tu fichero:

`08-PurpleTeam-MITRE-Semana_11_RedTeam_Pasos.md`

Describe:

- Acciones en orden.
- Qué esperas conseguir con cada acción.
- Qué "señales" esperas que deje cada acción (aunque luego se validen en Blue).

**Prompt IA:**  
"Actúa como Red Team coordinado con Blue Team. Redacta los pasos ofensivos en Markdown para un ejercicio Purple Team (escenario SSH o PowerShell). Debe incluir objetivo por paso, evidencias esperadas y controles de seguridad para no dañar el laboratorio."

---

### 5.3. Ejercicio 3 – Observación de evidencias (lado Blue)

Ahora te pones el gorro Blue Team. La idea es contestar:

- Qué evidencia existe.
- Qué evidencia falta.
- Qué se podría detectar de forma fiable.

Para Escenario A - SSH:

1. Copia el log de autenticación:

   sudo cp /var/log/auth.log ~/scripts_seguridad/auth_log_semana11.log  
   sudo chown "$USER":"$USER" ~/scripts_seguridad/auth_log_semana11.log

2. Reutiliza scripts de Semana 10 para:
- Contar "Failed password" por IP.
- Identificar "Accepted password".
- Correlacionar:
  - misma IP
  - ventana temporal
  - secuencia fallos → éxito

3. Construye un mini timeline:
- primer fallo
- pico de fallos
- primer éxito
- usuario afectado

Para Escenario B - PowerShell:

1. Revisa logs de PowerShell:
- "Windows PowerShell"
- "Microsoft-Windows-PowerShell/Operational" (si aplica)

2. Filtra por:
- "-NoProfile"
- "WindowStyle Hidden"
- la cadena de prueba "PurpleTeamTest1"

3. Extrae:
- TimeGenerated
- EventID
- Message (recortado)
- usuario y host si aparecen

Para Escenario C - XSS:

1. Revisa logs web:
- "/var/log/apache2/access.log" o equivalente
- "/var/log/apache2/error.log" si hay errores

2. Busca:
- la cadena "<script>" o un marcador de prueba
- patrones de parámetros en query o body (según configuración)

Crea:

`08-PurpleTeam-MITRE-Semana_11_BlueTeam_Evidencias.md`

Incluye:

- Fragmentos relevantes de logs.
- Explicación de qué representa cada fragmento.
- Timeline resumido.
- Gaps de visibilidad:
  - "no tengo command line completa"
  - "no tengo el usuario en el evento"
  - "no se registran parámetros en logs web" (si aplica)

**Prompt IA:**  
"Actúa como analista Blue Team. Redacta en Markdown un documento de evidencias: cómo recopilar logs, qué buscar, cómo crear un timeline y cómo decidir severidad en un ejercicio Purple Team. Incluye un apartado de 'gaps de visibilidad' con soluciones típicas."

---

### 5.4. Ejercicio 4 – Mapeo a MITRE ATT&CK

Ahora vas a usar MITRE ATT&CK como lenguaje común.

No hace falta que el ID sea perfecto. Lo importante es:

- Táctica coherente.
- Técnica coherente.
- Evidencia asociada.

Crea:

`08-PurpleTeam-MITRE-Semana_11_Mapeo_MITRE.md`

Para tu escenario:

1. Lista los pasos ofensivos ejecutados.
2. Para cada paso, indica:

- Descripción del paso
- Táctica ATT&CK (alto nivel)
- Técnica (descripción general)
- Evidencias y fuentes de datos
- Señal detectable (qué podrías convertir en regla)

Ejemplo (A - SSH):

- Paso: Descubrir servicio SSH
  - Táctica: Discovery
  - Técnica: Network Service Discovery o Port Scanning (según cómo lo describas)
  - Evidencias: en un entorno real, IDS/IPS o firewall; en tu lab, al menos el artefacto del escaneo y el hecho de que el puerto está expuesto

- Paso: Intentos fallidos repetidos
  - Táctica: Credential Access
  - Técnica: Brute Force
  - Evidencias: "Failed password" en auth.log

- Paso: Login exitoso tras fallos
  - Táctica: Initial Access o Lateral Movement (según objetivo)
  - Técnica: Valid Accounts y Remote Services
  - Evidencias: "Accepted password" y usuario en auth.log

Ejemplo (B - PowerShell):

- Paso: Ejecución con flags inusuales
  - Táctica: Execution
  - Técnica: PowerShell
  - Evidencias: eventos que contienen "-NoProfile" y "WindowStyle Hidden"

**Prompt IA:**  
"Genera un mapeo MITRE ATT&CK en formato Markdown para un ejercicio Purple Team (elige SSH o PowerShell). Debe incluir una tabla con columnas: Paso, Táctica, Técnica, Evidencia, Idea de detección, Riesgo. No hace falta incluir IDs exactos."

---

### 5.5. Ejercicio 5 – De MITRE a detecciones y mejoras

Ahora conectas:

- Técnica ATT&CK
- Evidencia
- Detección
- Mitigación

Crea:

`08-PurpleTeam-MITRE-Semana_11_Detecciones_Mejoras.md`

Para cada paso:

1. Detección potencial
- Condición (umbral, ventana temporal).
- Fuente de datos.
- Severidad propuesta.
- Falsos positivos esperables.
- Contexto necesario para confirmar.

2. Mejora defensiva
- Técnica:
  - hardening
  - autenticación fuerte
  - restricción de acceso por red
  - logging
- Proceso:
  - playbook
  - criterios de escalado
  - enriquecimiento (inventario, CMDB, lista de admins)

Ejemplos:

- SSH brute force:
  - Detección: N fallos por IP en X minutos.
  - Mejora: limitar intentos, restringir acceso, claves en vez de password, MFA si aplica, monitoreo continuo.

- PowerShell con flags:
  - Detección: flags sospechosos y usuario no esperado.
  - Mejora: logging reforzado, control de scripts, reducción de permisos, alertas basadas en línea de comandos.

**Prompt IA:**  
"Actúa como ingeniero de detección. Para un escenario Purple Team, escribe detecciones y mejoras en Markdown. Incluye: regla conceptual, campos requeridos, umbral, severidad, falsos positivos, y un mini playbook de triage N1 en 6 pasos."

---

### 5.6. Ejercicio 6 – Informe Purple Team

Por último, junta todo en un informe corto:

`08-PurpleTeam-MITRE-Semana_11_Informe_PurpleTeam.md`

Estructura sugerida:

1. Introducción
- Qué se pretendía demostrar y por qué.

2. Escenario
- Sistemas, alcance, ventana temporal.

3. Perspectiva Red Team
- Acciones ejecutadas, objetivo por acción.

4. Perspectiva Blue Team
- Evidencias encontradas, timeline, puntos ciegos.

5. Mapeo MITRE ATT&CK
- Resumen de tácticas y técnicas (tabla o lista).

6. Detecciones y recomendaciones
- Reglas propuestas.
- Umbrales.
- Mejoras técnicas y de proceso.

7. Lecciones aprendidas
- Qué funcionó.
- Qué no se vio.
- Qué se mejorará para repetir el ejercicio.

**Prompt IA:**  
"Redacta un informe Purple Team profesional en Markdown (1-2 páginas) con secciones claras y lenguaje de SOC. Debe incluir: objetivo, metodología, evidencias, mapeo MITRE, detecciones propuestas, mitigaciones y lecciones aprendidas. No inventes datos técnicos, usa placeholders donde falte información."

---

## 6. Entregables de la Semana 11

Al terminar esta semana, deberías tener:

1. `08-PurpleTeam-MITRE-Semana_11_Escenario.md`
- Guion del ejercicio Purple Team.

2. `08-PurpleTeam-MITRE-Semana_11_RedTeam_Pasos.md`
- Pasos ofensivos ejecutados y expectativas de señal.

3. `08-PurpleTeam-MITRE-Semana_11_BlueTeam_Evidencias.md`
- Evidencias observadas, fragmentos de logs, timeline y gaps.

4. `08-PurpleTeam-MITRE-Semana_11_Mapeo_MITRE.md`
- Relación pasos ↔ tácticas/técnicas ↔ evidencias ↔ idea de detección.

5. `08-PurpleTeam-MITRE-Semana_11_Detecciones_Mejoras.md`
- Detecciones propuestas y mejoras defensivas.

6. `08-PurpleTeam-MITRE-Semana_11_Informe_PurpleTeam.md`
- Informe final integrado.

**Prompt IA:**  
"Actúa como revisor de repositorio. Propón un checklist de validación para los entregables de Semana 11: qué debe aparecer en cada fichero, qué evidencias mínimas, y cómo evaluar si el ejercicio es repetible."

---

## 7. Checklist de cierre de la Semana 11

Marca como completado cuando:

- [ ] Puedes explicar qué es Purple Team y cómo se diferencia de pentest clásico o detección reactiva.
- [ ] Has definido un escenario de laboratorio que combine acciones ofensivas y análisis defensivo.
- [ ] Has ejecutado los pasos ofensivos (SSH, PowerShell o web) en tu entorno de pruebas.
- [ ] Has recopilado y entendido evidencias generadas en los logs.
- [ ] Has mapeado cada paso a tácticas y técnicas de MITRE ATT&CK de forma razonable.
- [ ] Has diseñado detecciones y mejoras defensivas basadas en ese escenario.
- [ ] Has escrito un informe Purple Team que conecte Red, Blue y MITRE ATT&CK.
- [ ] Has documentado al menos un gap de visibilidad y una acción para corregirlo.
- [ ] Has añadido todos los ficheros de la Semana 11 a tu repositorio.

Si has llegado hasta aquí, ya no ves ataque y defensa como mundos separados, sino como partes de un mismo ciclo de mejora continua, con un lenguaje común (MITRE ATT&CK) y ejercicios concretos (Purple Team) para subir el nivel técnico y de detección.

En la Semana 12 podemos cerrar el plan con:

- Repaso global.
- Proyecto final integrador.
- Preparación para empleo real:
  - portfolio
  - cómo presentar tu lab en entrevistas
  - cómo convertir estos .md en ventaja frente a otros juniors

**Prompt IA:**  
"Actúa como entrevistador técnico. Crea 12 preguntas basadas en tu ejercicio Purple Team: 4 de MITRE, 4 de evidencias/logs y 4 de detección y respuesta. Incluye qué respondería un junior sólido y qué sería una mala respuesta."

---

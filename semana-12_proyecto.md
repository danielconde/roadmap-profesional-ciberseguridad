# Semana 12 – Proyecto Final, Portfolio y Preparación para Empleo Real 🎓💼

## 0. Introducción a la semana

Has llegado al final de este plan intensivo de 3 meses.  
En estas 11 semanas anteriores has tocado:

- Fundamentos de ciberseguridad, redes, sistemas y datos.
- Seguridad en cloud, automatización y scripting.
- Hacking ético y seguridad web.
- Blue Team, SOC, Threat Hunting y enfoque Purple Team apoyado en MITRE ATT&CK.

Es mucho contenido para una persona que empieza, pero la clave es que has creado una base estructurada y un laboratorio sobre el que puedes seguir construyendo.

En esta Semana 12 vamos a centrar el foco en tres cosas:

1. Repaso global estructurado de lo aprendido (tipo "mapa mental").
2. Proyecto final integrador, que junte Red + Blue + automatización + documentación.
3. Preparación para empleo real:
   - Cómo presentar tu laboratorio y tu GitHub.
   - Cómo aprovechar estos .md como portfolio.
   - Qué puntos destacar en entrevistas para un puesto junior / SOC / ciberseguridad generalista.

**Prompt IA:**  
"Actúa como mentor de carrera en ciberseguridad. Resume mi ruta de 12 semanas en 12 bullets muy claros, y después propone un plan de la Semana 12 con: repaso, proyecto final, portfolio GitHub y preparación de entrevistas. Mantén tono profesional y accionable."

---

## 1. Objetivos de aprendizaje de la Semana 12

Al finalizar esta semana deberías ser capaz de:

1. Tener una visión global tipo "mapa" de todo lo visto en 3 meses.
2. Diseñar y ejecutar un proyecto final integrador que:
   - Use tu laboratorio.
   - Incluya al menos un escenario ofensivo + análisis defensivo.
   - Aplique automatización ligera (scripts).
   - Termine en un informe profesional.
3. Organizar tu repositorio GitHub para que parezca un mini-curso/portfolio y no solo un montón de archivos sueltos.
4. Saber explicar en una entrevista qué has aprendido:
   - A nivel técnico (Linux, redes, SOC, pentesting básico).
   - A nivel de mentalidad (metodología, documentación, laboratorio).
5. Tener una lista de siguientes pasos tras estos 3 meses (profundización, certificaciones, proyectos adicionales).

**Prompt IA:**  
"Define objetivos medibles para la Semana 12 (portfolio y empleo). Para cada objetivo incluye: criterio de éxito, evidencia que lo demuestra en GitHub, y cómo explicarlo en 30 segundos en una entrevista."

---

## 2. Repaso global estructurado

Antes de hacer el proyecto final, conviene que consolides la imagen general.  
Piensa en tus conocimientos como en capas.

### 2.1. Capa 1 – Base técnica

- Sistemas Operativos:
  - Linux (Kali, Ubuntu):
    - Estructura del sistema.
    - Servicios, procesos, logs (/var/log/auth.log, syslog).
    - Permisos y usuarios.
  - Windows:
    - Servicios, procesos, registro.
    - Visor de eventos.
    - PowerShell.

- Redes:
  - IP, puertos, TCP/UDP.
  - Rutas básicas, NAT, subredes.
  - Tráfico normal vs sospechoso.
  - Concepto de superficie expuesta: qué servicios escuchan, dónde, y con qué controles.

Idea clave de repaso:

- Si entiendes "qué ocurre" en sistemas y red, luego puedes entender "cómo se ataca" y "cómo se detecta".

### 2.2. Capa 2 – Ciberseguridad general

- Modelo CIA (Confidencialidad, Integridad, Disponibilidad).
- Tipos de ataques (phishing, malware, ransomware, fuerza bruta, etc.).
- Kill Chain / MITRE ATT&CK como forma de describir ataques:
  - Kill Chain como secuencia.
  - ATT&CK como catálogo de técnicas.
- Datos, backups, RPO/RTO, identidad, permisos:
  - La mayoría de incidentes afectan a datos e identidad, no a "máquinas" como tal.
  - Una buena estrategia de backup y de permisos reduce muchísimo impacto real.

### 2.3. Capa 3 – Cloud y automatización

- Cloud:
  - IaaS, PaaS, SaaS.
  - Modelo de responsabilidad compartida.
  - VM en cloud, redes virtuales, Security Groups / NSG.
  - Buckets/Blobs y problemas típicos de configuración (exposición accidental, credenciales, permisos excesivos).

- Automatización y scripting:
  - Bash para procesar logs Linux (grep, wc, pipelines).
  - Python para procesar ficheros y agrupar resultados.
  - PowerShell para eventos Windows.
  - Uso responsable de IA (vibecoding):
    - La IA acelera, pero tú validas.
    - Todo se prueba en laboratorio.

### 2.4. Capa 4 – Ofensiva básica (Red Team / Hacking ético)

- Fases de un pentest:
  - Reconocimiento, escaneo, enumeración.
  - Explotación y post-explotación (a nivel conceptual, sin necesidad de CVEs complejas).
  - Informe: siempre documentar.

- Kali Linux:
  - nmap para escaneo.
  - Herramientas básicas orientadas a web (Burp, DevTools).

- Seguridad web:
  - HTTP, parámetros, sesiones, cookies.
  - OWASP Top 10 (visión general).
  - Conceptos de XSS, inyección, exposición de datos.
  - La web es un flujo de peticiones donde los parámetros viajan y se pueden manipular.

### 2.5. Capa 5 – Defensiva (Blue Team / SOC / Hunting)

- Eventos vs alertas vs incidentes.
- Concepto de SIEM, EDR, IDS/IPS, WAF como fuentes de señales.
- IOCs, reglas, casos de uso, triage.
- Threat Hunting:
  - Hipótesis → búsquedas → análisis → conclusiones → mejoras.
  - Hunting con SSH (auth.log) y PowerShell.

Concepto importante para entrevistas:

- No es solo "sé herramientas", es "sé el flujo": qué miro, por qué, cómo concluyo.

### 2.6. Capa 6 – Enfoque Purple Team

- Red y Blue colaborando.
- Uso de MITRE ATT&CK para describir técnicas y enlazarlas con evidencias.
- Ejercicios Purple:
  - Escenario concreto y repetible.
  - Pasos ofensivos.
  - Evidencias en logs.
  - Detecciones y mejoras.

Aquí está uno de tus diferenciales de junior:

- Sabes unir ataque → evidencia → detección → mejora.

**Prompt IA:**  
"Convierte estas 6 capas en un mapa mental en texto: nodos principales y sub-nodos. Después, genera un guion de 60 segundos para explicar el roadmap completo en una entrevista."

---

## 3. Proyecto Final Integrador

El proyecto final es la parte más importante de esta semana.  
La idea es que, cuando termines, puedas decir:

- "No solo he seguido un curso, he construido y documentado un ejercicio completo que muestra cómo trabajo."

### 3.1. Nombre del fichero principal

Proyecto principal:

- `09-Proyecto-Empleo-Semana_12_Proyecto_Final.md`

Además, puedes crear sub-ficheros si se alarga demasiado (logs, scripts, capturas, etc.).

### 3.2. Objetivo del proyecto

Diseñar y ejecutar un escenario ataque–defensa en tu laboratorio, que incluya:

1. Parte ofensiva:
   - Descubrimiento.
   - Ataque controlado (SSH, PowerShell, web o combinación).
2. Parte defensiva:
   - Recogida de evidencias (logs).
   - Análisis, hunting básico.
   - Propuesta de detecciones y mejoras.
3. Automatización:
   - Al menos 1 script (Bash, Python o PowerShell) usado para analizar logs o apoyar la detección.
4. Informe:
   - Documento final con estructura profesional.

Regla de oro:

- Todo debe ser reproducible: que dentro de 1 mes puedas repetir el proyecto y obtener señales parecidas.

**Prompt IA:**  
"Propón 3 ideas de proyecto final integrador para un laboratorio con Kali, Ubuntu y Windows. Para cada idea: pasos ofensivos, evidencias en logs, script de automatización sugerido, y valor para un reclutador. Recomienda una opción final."

---

## 4. Fases del proyecto final

En el fichero `09-Proyecto-Empleo-Semana_12_Proyecto_Final.md`, estructura recomendada:

### 4.1. 1. Descripción del entorno de laboratorio

Incluye:

- Máquinas:
  - Kali (atacante).
  - Ubuntu (servidor).
  - Windows (estación / servidor).
- Red:
  - IPs.
  - Tipo de red (Host-Only, NAT, etc.).
- Servicios expuestos (puertos principales por máquina).
- Control previo:
  - Snapshot si aplica.
  - Usuarios de prueba (y cómo los distingues de usuarios "reales" del lab).

**Prompt IA:**  
"Redacta en Markdown una sección 'Entorno' para mi proyecto final: inventario de máquinas, red, servicios expuestos y pre-requisitos (snapshots, usuarios de prueba, rutas de logs). Usa placeholders donde falten datos."

---

### 4.2. 2. Escenario de ataque elegido

Elige uno (o mezcla dos si está muy claro y no te complica).

#### Opción 1 – SSH + Linux

- Ataque:
  - Escaneo de puertos.
  - Intentos de login fallidos controlados.
  - Login exitoso con credenciales de prueba.
- Defensa:
  - Picos de Failed password.
  - Correlación con Accepted password desde la misma IP.
  - Regla conceptual de detección.

#### Opción 2 – PowerShell en Windows

- Ataque:
  - Ejecución de PowerShell con flags sospechosos (-NoProfile, -WindowStyle Hidden).
  - Simulación de comportamiento (sin malware real).
- Defensa:
  - Eventos de PowerShell.
  - Script de hunting para filtrar.
  - Alerta conceptual y hardening.

#### Opción 3 – WebApp + parámetros / XSS

- Ataque:
  - Manipulación de parámetros en web de prueba.
  - XSS de prueba en entorno controlado.
- Defensa:
  - Logs web.
  - Idea de detección (WAF / SIEM).
  - Medidas de sanitización y desarrollo seguro.

En el proyecto, define claramente:

- Escenario seleccionado.
- Justificación: por qué lo elegiste y qué demuestra.

**Prompt IA:**  
"Quiero elegir un escenario (SSH, PowerShell o Web). Hazme un cuestionario rápido de 8 preguntas para decidir cuál encaja mejor con mi perfil objetivo (SOC vs pentest vs generalista). Al final recomienda el escenario."

---

### 4.3. 3. Paso a paso ofensivo (Red Team mini)

Describe, paso a paso:

1. Reconocimiento:
   - Descubrimiento de servicios expuestos.
   - Enumeración mínima.
2. Ataque:
   - Acciones concretas (SSH, PowerShell, web).
   - Qué pretende demostrar cada acción.

Recomendación de estilo:

- Usa pasos numerados.
- Incluye comandos solo si son de tu laboratorio.
- Manténlo controlado: el objetivo es generar evidencia, no "romper" nada.

**Prompt IA:**  
"Redacta un 'Paso a paso Red Team' para el escenario que elija (SSH o PowerShell o Web). Debe incluir objetivo por paso, comando o acción, y qué evidencia debería quedar en logs. Mantén todo en entorno de laboratorio."

---

### 4.4. 4. Evidencias y análisis defensivo (Blue/Threat Hunting)

Para cada paso ofensivo, responde:

- Qué log se genera.
- Dónde está (ruta o visor).
- Qué campos importan:
  - IP, usuario, timestamp, proceso, comando, endpoint, etc.

Incluye:

- Extractos de logs (recortados).
- Resultado de scripts de hunting.

Ficheros opcionales si quieres detalle:

- `09-Proyecto-Empleo-Semana_12_Logs_SSH.md`
- `09-Proyecto-Empleo-Semana_12_Logs_PowerShell.md`
- `09-Proyecto-Empleo-Semana_12_Logs_Web.md`

La idea es que el proyecto final quede limpio, y los logs estén en anexos.

**Prompt IA:**  
"Actúa como analista SOC. Genera en Markdown una sección de 'Evidencias y análisis' para mi proyecto final: qué buscar en logs, cómo correlacionar eventos, cómo construir un timeline y cómo decidir si sería True Positive o prueba controlada."

---

### 4.5. 5. Automatización aplicada

Elige al menos un script que forme parte del proyecto final:

- Bash, Python o PowerShell.
- Debe aportar valor real:
  - contar, agrupar, extraer, filtrar
  - producir un output que podrías pegar en un informe

Incluye:

- Código del script.
- Explicación de alto nivel de cómo funciona.
- Ejemplo de salida.
- Ideas de mejora futura.

Buenas ideas de automatización (según escenario):

- SSH:
  - contar fallos por IP
  - detectar secuencia fallos → éxito
- PowerShell:
  - filtrar eventos por flags sospechosos
  - destacar usuario y hora
- Web:
  - buscar patrones en access.log
  - extraer top rutas o parámetros sospechosos

**Prompt IA:**  
"Genera un script sencillo (elige Bash o Python o PowerShell según mi escenario) para analizar logs del laboratorio: agrupar por IP o usuario, y mostrar un resumen tipo reporte. Incluye explicación de uso y ejemplo de salida."

---

### 4.6. 6. Mapeo a MITRE ATT&CK

Haz un resumen del escenario usando MITRE:

- Tácticas implicadas (Execution, Credential Access, Initial Access, etc.).
- Técnicas en texto (no hace falta ID exacto).
- Evidencias asociadas.

Ejemplo:

- Fuerza bruta SSH:
  - Táctica: Credential Access / Initial Access.
  - Técnica: intentos repetidos de autenticación sobre servicio SSH.
  - Evidencias: Failed password repetidos en auth.log.

- PowerShell con WindowStyle Hidden:
  - Táctica: Execution / Defense Evasion.
  - Técnica: PowerShell con parámetros que ocultan ventana o reducen trazabilidad.
  - Evidencias: eventos que contienen esos flags.

**Prompt IA:**  
"Haz un mapeo MITRE ATT&CK en Markdown para mi escenario. Incluye una tabla: Paso, Táctica, Técnica, Evidencia, Idea de detección y Mitigación. No uses IDs si no son seguros."

---

### 4.7. 7. Conclusiones, detecciones y mejoras

Cierra el proyecto con:

1. Resumen del ataque simulado:
   - qué se hizo
   - qué se demostró

2. Capacidad de detección actual:
   - qué se ve bien
   - qué no se ve
   - qué automatización te ayuda

3. Detecciones propuestas:
   - idea de regla SIEM
   - umbrales y ventana temporal
   - severidad
   - falsos positivos esperables

4. Mejoras de hardening:
   - a nivel técnico
   - a nivel de proceso (mini playbook)

5. Reflexión personal:
   - qué te costó
   - qué te gustó
   - qué repetirías para mejorar

**Prompt IA:**  
"Actúa como ingeniero de detección. A partir de un escenario (SSH o PowerShell o Web), escribe conclusiones y detecciones propuestas: reglas, campos necesarios, umbrales, severidad, falsos positivos y pasos de triage N1."

---

## 5. Organizar tu GitHub como portfolio

Hasta ahora has creado muchos .md. Vamos a convertir eso en un mini-curso/portfolio limpio.

### 5.1. Crear una estructura clara

No hace falta que muevas todo de golpe, pero sí que:

- haya una ruta lógica
- haya enlaces desde el README
- se entienda el orden

### 5.2. README principal orientado a reclutadores

En tu README.md:

- Qué es este repositorio:
  - "Roadmap de 3 meses de aprendizaje autodidacta en ciberseguridad (SOC + ofensiva básica + automatización)."
- Qué incluye:
  - semanas
  - prácticas
  - scripts
  - proyecto final
- Cómo navegarlo:
  - enlaces a carpetas
  - enlaces al proyecto final
- Tecnologías y conceptos:
  - Linux, Windows, redes, cloud, logs, scripting, MITRE, OWASP

Consejo práctico:

- El README debe poder leerse en 30 segundos y dejar claro que hay método y entrega.

**Prompt IA:**  
"Redacta un README.md para reclutadores sobre mi repositorio de ciberseguridad. Debe incluir: resumen, qué encontrarán, estructura por carpetas con enlaces, skills demostradas, y cómo ejecutar o revisar el proyecto final. Tono profesional y claro."

---

## 6. Preparación para entrevistas y empleo real (junior)

Crea un fichero:

- `09-Proyecto-Empleo-Semana_12_Empleo_Preparacion.md`

### 6.1. Preguntas típicas y guiones de respuesta

Responde por escrito:

1. "Cuéntame tu experiencia en ciberseguridad."
- Habla del laboratorio, del plan por semanas y del proyecto final.

2. "¿Qué sabes de SOC y de un analista N1/N2?"
- Evento, alerta, incidente.
- Triage, escalado, documentación.
- Threat hunting básico.

3. "¿Has hecho algo de pentesting?"
- Escaneos y enumeración en laboratorio.
- Seguridad web básica (OWASP, Burp).
- Siempre recalcando entorno controlado.

4. "¿Sabes programar / automatizar?"
- Scripts Bash/Python/PowerShell para analizar logs.
- Uso de IA para generar scripts y revisión manual.

5. "¿Has trabajado con MITRE ATT&CK / Purple Team?"
- Ejercicios Purple.
- Mapeo técnica → evidencia → detección.

### 6.2. Qué te diferencia como junior

Incluye una sección:

- "Qué me diferencia como junior"

Ideas típicas:

- Laboratorio funcional con varios sistemas.
- Documentación sistemática (no solo práctica).
- Visión ofensiva y defensiva unidas.
- Automatización aplicada desde el inicio.
- Proyecto final integrador con mapeo MITRE.

**Prompt IA:**  
"Actúa como entrevistador para un rol junior SOC o generalista. Genera 15 preguntas y una guía de respuesta en formato STAR. Usa como base mi laboratorio, mis scripts y mi proyecto final."

---

## 7. Siguientes pasos tras los 3 meses

En `09-Proyecto-Empleo-Semana_12_Siguientes_Pasos.md`:

1. Define foco principal:
- SOC / Blue Team
- Pentest / Red Team
- Cloud Security
- Automatización / Security Engineering

2. Define 2 líneas de trabajo:
- 1 certificación o ruta formativa
- 1 proyecto práctico

Ejemplos de proyectos:

- Montar un mini SIEM local (ingesta básica de logs y dashboards).
- Repetir hunts y convertirlos en reglas formales.
- Ampliar web lab con una app vulnerable y documentar mitigaciones.
- Automatizar generación de reportes a partir de logs.

**Prompt IA:**  
"Propón un plan de 8 semanas post-curso según mi enfoque (SOC o Pentest o Cloud). Incluye: objetivos semanales, entregables en GitHub, y una mini lista de certificaciones o rutas recomendadas."

---

## 8. Entregables de la Semana 12

Al menos:

1. `09-Proyecto-Empleo-Semana_12_Proyecto_Final.md`
- Proyecto integrador: entorno, ataque, defensa, automatización, MITRE y conclusiones.

2. `09-Proyecto-Empleo-Semana_12_Empleo_Preparacion.md`
- Respuestas a entrevistas.
- Guiones y puntos diferenciales.

3. Opcional pero recomendable:
- `09-Proyecto-Empleo-Semana_12_Siguientes_Pasos.md`
- Plan post-curso.

Además:

- README principal revisado.
- Estructura de carpetas coherente y navegable.

**Prompt IA:**  
"Actúa como revisor final. Crea un checklist de validación para Semana 12: proyecto final, README, estructura de repo y preparación de entrevistas. Debe ser una lista accionable y específica."

---

## 9. Checklist de cierre de la Semana 12

Marca como completado cuando:

- [ ] Has hecho un repaso global de las capas de conocimiento que has adquirido.
- [ ] Has diseñado y ejecutado un proyecto final integrador en tu laboratorio.
- [ ] Has documentado el proyecto de forma clara y profesional.
- [ ] Has preparado tu repositorio GitHub para que sea entendible como portfolio.
- [ ] Has escrito respuestas a preguntas típicas de entrevistas de ciberseguridad junior.
- [ ] Tienes una idea clara de tus siguientes pasos (especialización, certificaciones, proyectos).
- [ ] Puedes explicar lo que sabes de forma ordenada y convincente en 60-90 segundos.

Si completas esta semana, no solo tendrás conocimientos, sino también:

- Evidencias (lab, scripts, documentos).
- Narrativa (cómo contar tu historia de aprendizaje).
- Plan de acción para seguir creciendo.

Y eso, en un perfil junior, marca mucha diferencia. 💼🚀

**Prompt IA:**  
"Actúa como coach. Escribe un pitch de 45 segundos para entrevista basado en mi ruta de 12 semanas. Debe mencionar laboratorio, proyecto final, scripts, visión SOC y MITRE/OWASP, sin sonar a texto memorizado."

---

## 10. Mensaje de cierre del curso

Enhorabuena por llegar hasta el final de este curso de introducción a la ciberseguridad. 🙌

Espero de verdad que todo el contenido - laboratorio, semanas, ejercicios prácticos y proyecto final - te haya resultado útil para construir una base sólida y empezar, o reforzar, tu camino en este mundo.

Si algo de todo esto te ha aportado aunque sea una sola idea o un pequeño empujón, para mí ya habrá merecido la pena haberlo creado.

Te invito a:

- Seguirme para no perderte futuros contenidos, mejoras del curso y nuevos proyectos.
- Compartir el repositorio/curso con cualquier persona que esté empezando en ciberseguridad o quiera montar su propio laboratorio.

Cuanta más gente pueda aprovechar este material, mejor para la comunidad.  
Nos vemos en los siguientes proyectos. 💻🛡️🚀

---

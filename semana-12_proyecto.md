# Semana 12 – Proyecto Final, Portfolio y Preparación para Empleo Real 🎓💼

## 0. Introducción a la semana

Has llegado al final de este plan intensivo de 3 meses.  
En estas 11 semanas anteriores has tocado:

- Fundamentos de ciberseguridad, redes, sistemas y datos.
- Seguridad en cloud, automatización y scripting.
- Hacking ético y seguridad web.
- Blue Team, SOC, Threat Hunting y enfoque Purple Team apoyado en MITRE ATT&CK.

Es mucho contenido para una persona que empieza, pero la clave es que **has creado una base estructurada** y un laboratorio sobre el que puedes seguir construyendo.

En esta Semana 12 vamos a centrar el foco en tres cosas:

1. **Repaso global estructurado** de lo aprendido (tipo “mapa mental”).
2. **Proyecto final integrador**, que junte Red + Blue + automatización + documentación.
3. **Preparación para empleo real**:
   - Cómo presentar tu laboratorio y tu GitHub.
   - Cómo aprovechar estos `.md` como portfolio.
   - Qué puntos destacar en entrevistas para un puesto junior / SOC / ciberseguridad generalista.

---

## 1. Objetivos de aprendizaje de la Semana 12

Al finalizar esta semana deberías ser capaz de:

1. Tener una visión global (tipo “mapa”) de todo lo que has visto en 3 meses.
2. Diseñar y ejecutar un **proyecto final integrador** que:
   - Use tu laboratorio.
   - Incluya al menos un escenario ofensivo + análisis defensivo.
   - Aplique automatización ligera (scripts).
   - Termine en un informe profesional.
3. Organizar tu repositorio GitHub para que parezca un **mini-curso/portfolio** y no solo un montón de archivos sueltos.
4. Saber explicar, en una entrevista, qué has aprendido:
   - A nivel técnico (Linux, redes, SOC, pentesting básico).
   - A nivel de mentalidad (metodología, documentación, laboratorio).
5. Tener una lista de **siguientes pasos** tras estos 3 meses (profundización, certificaciones, proyectos adicionales).

---

## 2. Repaso global estructurado

Antes de hacer el proyecto final, conviene que consolides la imagen general.  
Piensa en tus conocimientos como en **capas**:

### 2.1. Capa 1 – Base técnica

- **Sistemas Operativos**:
  - Linux (Kali, Ubuntu):
    - Estructura del sistema.
    - Servicios, procesos, logs (`/var/log/auth.log`, syslog).
    - Permisos y usuarios.
  - Windows:
    - Servicios, procesos, registro.
    - Visor de eventos.
    - PowerShell.

- **Redes**:
  - IP, puertos, TCP/UDP.
  - Rutas básicas, NAT, subredes.
  - Tráfico normal vs sospechoso.

### 2.2. Capa 2 – Ciberseguridad general

- Modelo CIA (Confidencialidad, Integridad, Disponibilidad).
- Tipos de ataques (phishing, malware, ransomware, fuerza bruta, etc.).
- Kill Chain / MITRE ATT&CK como forma de describir ataques.
- Datos, backups, RPO/RTO, identidad, permisos.

### 2.3. Capa 3 – Cloud y automatización

- Conceptos de cloud:
  - IaaS, PaaS, SaaS.
  - VM en cloud, redes virtuales, Security Groups / NSG.
  - Buckets/Blobs y problemas típicos de configuración.
- Automatización y scripting:
  - Bash para logs Linux.
  - Python para procesar ficheros.
  - PowerShell para eventos Windows.
  - Uso responsable de IA (vibecoding) para generar y mejorar scripts.

### 2.4. Capa 4 – Ofensiva básica (Red Team / Hacking ético)

- Fases de un pentest:
  - Reconocimiento, escaneo, enumeración.
  - (Pre-explotación, explotación, post-explotación, informe).
- Kali Linux:
  - nmap para escaneo.
  - Herramientas básicas orientadas a web (Burp, etc.).
- Seguridad web:
  - HTTP, parámetros, sesiones, cookies.
  - OWASP Top 10 (visión general).
  - Conceptos de XSS, inyección, exposición de datos.

### 2.5. Capa 5 – Defensiva (Blue Team / SOC / Hunting)

- Eventos vs alertas vs incidentes.
- Concepto de SIEM, EDR, IDS/IPS, WAF.
- IOCs, reglas, casos de uso.
- Threat Hunting:
  - Hipótesis → búsquedas → análisis → conclusiones.
  - Hunting con SSH (auth.log) y PowerShell.

### 2.6. Capa 6 – Enfoque Purple Team

- Red y Blue colaborando.
- Uso de MITRE ATT&CK para describir técnicas.
- Ejercicios Purple:
  - Escenario concreto.
  - Pasos ofensivos.
  - Evidencias en logs.
  - Detecciones y mejoras.

---

## 3. Proyecto Final Integrador

El proyecto final es la parte más importante de esta semana.  
La idea es que, cuando termines, puedas decir:

> “No solo he seguido un curso, he construido y documentado un **ejercicio completo** que muestra cómo trabajo.”

### 3.1. Nombre del fichero principal

Proyecto principal:

`09-Proyecto-Empleo-Semana_12_Proyecto_Final.md`

Además, puedes crear sub-ficheros si se alarga demasiado (logs, scripts, etc.).

### 3.2. Objetivo del proyecto

Diseñar y ejecutar un **escenario de ataque–defensa** en tu laboratorio, que incluya:

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

---

## 4. Fases del proyecto final

En el fichero `09-Proyecto-Empleo-Semana_12_Proyecto_Final.md`, te propongo esta estructura:

### 4.1. 1. Descripción del entorno de laboratorio

- Resumen de tus máquinas:
  - Kali (atacante).
  - Ubuntu (servidor).
  - Windows (estación de trabajo / servidor).
- Esquema sencillo de red:
  - IPs principales.
  - Tipo de red (Host-Only, NAT, etc.).
- Servicios expuestos en cada máquina (puertos principales).

Puedes reutilizar info de `Lab_Setup_Report.md` pero resumiendo.

---

### 4.2. 2. Escenario de ataque elegido

Elige **UNO** de estos enfoques (o mezcla dos si te ves con fuerzas):

#### Opción 1 – SSH + Linux

- Ataque:
  - Escaneo de puertos.
  - Fuerza bruta SSH controlada con intentos limitados.
  - Login exitoso con credenciales débiles de usuario de pruebas.
- Defensa:
  - Búsqueda de picos de `Failed password`.
  - Detección de login exitoso desde la misma IP.
  - Propuesta de regla de detección.

#### Opción 2 – PowerShell en Windows

- Ataque:
  - Ejecución de PowerShell con flags sospechosos (`-NoProfile`, `-WindowStyle Hidden`).
  - Simulación de comando de descarga o ejecución (sin malware real).
- Defensa:
  - Eventos de PowerShell.
  - Script de hunting para filtrar eventos con esos patrones.
  - Propuesta de alerta y medidas de hardening.

#### Opción 3 – WebApp + XSS / parámetros

- Ataque:
  - Manipulación de parámetros en tu web de prueba (`test_form.php`).
  - XSS sencillo o manipulación de valores inesperados.
- Defensa:
  - Logs del servidor web.
  - Reflexión sobre cómo detectarlo con WAF/Reglas SIEM.
  - Propuesta de sanitización y medidas de desarrollo seguro.

En tu proyecto, define claramente:

- Escenario seleccionado.
- Justificación de por qué lo has elegido (por ejemplo, alineado con SOC, pentest web, etc.).

---

### 4.3. 3. Paso a paso ofensivo (Red Team mini)

Describe, paso a paso:

1. Reconocimiento:
   - Escaneo con nmap (si aplica).
   - Descubrimiento de servicios.
2. Ataque:
   - Comandos/conexiones específicas (SSH, PowerShell, web).
   - Ejemplos de parámetros, comandos, scripts.

Aquí no necesitas exploits avanzados; lo importante es la **metodología y claridad**.

---

### 4.4. 4. Evidencias y análisis defensivo (Blue/Threat Hunting)

Para cada paso ofensivo, responde:

- ¿Qué logs se han generado?
- ¿En qué rutas/visores se encuentran?
- ¿Qué campos son clave? (IP, usuario, comando, endpoint, etc.)

Incluye:

- Extractos de logs.
- Resultado de scripts de hunting (por ejemplo, conteo de intentos SSH, filtrado de eventos de PowerShell).

Puedes crear ficheros adicionales para detalle, por ejemplo:

- `09-Proyecto-Empleo-Semana_12_Logs_SSH.md`
- `09-Proyecto-Empleo-Semana_12_Logs_PowerShell.md`
- etc.

---

### 4.5. 5. Automatización aplicada

Elige al menos **un script** que forme parte del proyecto final:

- Bash, Python o PowerShell.
- Que haga algo útil, como:
  - Contar intentos de login fallidos por IP.
  - Extraer eventos PowerShell con ciertos parámetros.
  - Buscar patrones concretos en un log web.

Incluye:

- Código del script.
- Breve explicación de cómo funciona.
- Ejemplo de salida.
- Ideas de mejora futura.

---

### 4.6. 6. Mapeo a MITRE ATT&CK

Haz un resumen del escenario usando MITRE:

- Tácticas implicadas (Execution, Credential Access, Initial Access, etc.).
- Técnicas (de forma textual, no hace falta ID exacto).
- Evidencias asociadas a cada técnica.

Ejemplo:

- **Fuerza bruta SSH**:
  - Táctica: Credential Access / Initial Access.
  - Técnica: Intentos repetidos de autenticación sobre servicio SSH.
  - Evidencias: `Failed password` repetidos en auth.log.

- **PowerShell con WindowStyle Hidden**:
  - Táctica: Execution / Defense Evasion.
  - Técnica: Uso de PowerShell con parámetros que evitan mostrar ventanas.
  - Evidencias: Eventos de PowerShell con `-NoProfile`, `WindowStyle Hidden`.

---

### 4.7. 7. Conclusiones, detecciones y mejoras

Cierra el proyecto con:

1. **Resumen del ataque simulado**:
   - Qué se ha hecho.
   - Qué se ha demostrado.

2. **Capacidad de detección actual en tu lab**:
   - Qué has conseguido ver.
   - Qué scripts o búsquedas funcionan bien.

3. **Detecciones propuestas**:
   - Idea de reglas SIEM.
   - Umbrales (x intentos en y minutos, etc.).
   - Severidad.

4. **Mejoras de hardening**:
   - A nivel de sistema:
     - Configuración de SSH (no root login, MFA, reglas de firewall).
     - Políticas de PowerShell.
   - A nivel de procesos:
     - Respuesta ante detecciones similares.
     - Documentación/playbook.

5. **Reflexión personal**:
   - Qué parte te ha costado más.
   - Qué parte te ha gustado más (SOC, pentest, automatización, Purple…).

---

## 5. Organizar tu GitHub como portfolio

Hasta ahora has creado muchos `.md`. Vamos a convertir eso en un mini-curso/portfolio limpio.

### 5.1. Crear una estructura clara

En tu repositorio, algo así:

- `/README.md` (principal del curso).
- `/00-Lab` → documentación del laboratorio.
- `/01-Fundamentos` → semanas 1–4 (conceptos base).
- `/02-Cloud-Automatizacion` → semanas 5–6.
- `/03-Ofensiva` → semanas 7 y 9.
- `/04-Defensiva` → semanas 8 y 10.
- `/05-Purple-MITRE` → semana 11.
- `/06-Proyecto-Final` → semana 12 (este proyecto).

No hace falta que muevas los ficheros sí o sí, pero tener esta idea te ayuda a explicarlo en entrevistas.

### 5.2. README principal orientado a reclutadores

En tu `README.md` principal (del repo), explica:

- Qué es este repositorio:
  - “Roadmap de 3 meses de aprendizaje autodidacta en ciberseguridad (SOC + Red Team + automatización).”
- Qué incluye:
  - Estructura por semanas.
  - Proyecto final.
- Cómo se puede navegar:
  - Enlaces a carpetas/secciones.

Piensa en alguien que no te conoce y entra desde LinkedIn; debe entender rápido:

> “Esta persona se ha organizado un plan serio, ha montado un lab, ha hecho un proyecto final y lo ha documentado.”

---

## 6. Preparación para entrevistas y empleo real (junior)

En esta parte no vas a escribir código, sino **respuestas y guiones** para ti.

Crea un fichero:

`09-Proyecto-Empleo-Semana_12_Empleo_Preparacion.md`

### 6.1. Preguntas típicas que te pueden caer

Responde, por escrito, a cosas como:

1. **“Cuéntame tu experiencia en ciberseguridad.”**  
   → Aquí puedes hablar de:
   - Tu laboratorio.
   - Este curso estructurado por semanas.
   - El proyecto final integrador.

2. **“¿Qué sabes de SOC y de un analista N1/N2?”**  
   - Explica:
     - Eventos, alertas, incidentes.
     - Triage, uso de SIEM.
     - Threat Hunting básico.

3. **“¿Has hecho algo de pentesting?”**  
   - Cuenta:
     - Tus prácticas con Kali, nmap, escenarios de laboratorio.
     - Seguridad web básica (OWASP, XSS de prueba).
     - Siempre recalcando que ha sido en entorno controlado.

4. **“¿Sabes programar / automatizar?”**  
   - Menciona:
     - Scripts en Bash, Python y PowerShell.
     - Cómo los has usado para analizar logs.
     - Uso de IA para generar scripts y luego revisarlos.

5. **“¿Has trabajado con MITRE ATT&CK / Purple Team?”**  
   - Explica:
     - Que has hecho ejercicios simples de Purple Team.
     - Cómo has mapeado técnicas a logs y detecciones.

### 6.2. Destacar lo que te hace diferente como junior

En el mismo fichero, escribe una mini sección:

> “Qué me diferencia como junior”

Por ejemplo:

- Tienes un laboratorio funcional con varios sistemas.
- Has documentado sistemáticamente tus ejercicios.
- Has simulado ejercicios Purple Team simples.
- Has trabajado tanto visión ofensiva como defensiva.
- Has aplicado automatización (scripts) desde el principio.

Esto luego lo podrás usar en tu perfil de LinkedIn, CV y entrevistas.

---

## 7. Siguientes pasos tras los 3 meses

Por último, en el mismo fichero o en `09-Proyecto-Empleo-Semana_12_Siguientes_Pasos.md`, define:

- ¿Quieres orientarte más a:**
  - SOC / Blue Team.
  - Red Team / pentesting.
  - Cloud Security.
  - Automatización / ingeniería de seguridad.

- En función de eso, decide:
  - 1–2 certificaciones a medio plazo (ej: orientadas a SOC, a pentest, etc.).
  - 1–2 proyectos extra:
    - Más máquinas vulnerables (CTFs).
    - Más casos de uso de detección.
    - Más integración con herramientas reales (cuando tengas acceso a SIEM/EDR).

La idea es que el curso no “termine”, sino que se convierta en tu base para seguir construyendo.

---

## 8. Entregables de la Semana 12

Al menos:

1. `09-Proyecto-Empleo-Semana_12_Proyecto_Final.md`  
   - Proyecto integrador con entorno, ataque, defensa, automatización, MITRE y conclusiones.

2. `09-Proyecto-Empleo-Semana_12_Empleo_Preparacion.md`  
   - Respuestas preparadas a preguntas típicas de entrevista.
   - Puntos clave para destacar tu perfil.

3. (Opcional pero recomendable)  
   - `09-Proyecto-Empleo-Semana_12_Siguientes_Pasos.md`  
     - Plan personal post-curso (certificaciones, proyectos, enfoque).

Además de:

- README principal del repositorio revisado.
- Carpetas organizadas de forma lógica.

---

## 9. Checklist de cierre de la Semana 12

Marca como completado cuando:

- [ ] Has hecho un repaso global de las capas de conocimiento que has adquirido.
- [ ] Has diseñado y ejecutado un proyecto final integrador en tu laboratorio.
- [ ] Has documentado el proyecto de forma clara y profesional.
- [ ] Has preparado tu repositorio GitHub para que sea entendible como portfolio.
- [ ] Has escrito respuestas a preguntas típicas de entrevistas de ciberseguridad junior.
- [ ] Tienes una idea clara de tus siguientes pasos (especialización, certificaciones, proyectos).
- [ ] Sientes que puedes explicar lo que sabes de forma ordenada y convincente.

Si completas esta semana, no solo tendrás **conocimientos**, sino también:

- **Evidencias** (lab, scripts, documentos).
- **Narrativa** (cómo contar tu historia de aprendizaje).
- **Plan de acción** para seguir creciendo.

Y eso, en un perfil junior, marca mucha diferencia. 💼🚀

Enhoorabuena por llegar hasta el final de este curso de introducción a la ciberseguridad! 🙌  

Espero de verdad que todo el contenido —laboratorio, semanas, ejercicios prácticos y proyecto final— te haya resultado útil para construir una base sólida y empezar (o reforzar) tu camino en este mundo.  
Si algo de todo esto te ha aportado, aunque sea una sola idea o un pequeño empujón, para mí ya habrá merecido la pena haberlo creado.  

Te invito a:  

- ⭐ **Seguirme** para no perderte futuros contenidos, mejoras del curso y nuevos proyectos.  
- 🔁 **Compartir el repositorio/curso** con cualquier persona que esté empezando en ciberseguridad o quiera montar su propio laboratorio.  

Cuanta más gente pueda aprovechar este material, mejor para la comunidad.  
¡Nos vemos en los siguientes proyectos! 💻🛡️🚀


---

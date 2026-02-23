# Semana 8 – Blue Team, SOC y Detección Básica 🛡️📊

## 0. Introducción a la semana

Hasta ahora has visto:

- Cómo montar y manejar tu laboratorio.
- Fundamentos de ciberseguridad, redes y sistemas.
- Datos, backups e identidad.
- Cloud y su modelo de responsabilidad compartida.
- Automatización básica (Bash, Python, PowerShell).
- Fundamentos de hacking ético y mini pentest interno.

En esta Semana 8 cambiamos el enfoque hacia donde trabaja muchísima gente en ciberseguridad:  
el "Blue Team" y, en concreto, el "SOC (Security Operations Center)".

El SOC es el "centro de mando" de la defensa:

- Monitoriza logs, alertas, eventos.
- Detecta actividades sospechosas.
- Investiga incidentes.
- Escala y coordina respuesta.

En esta semana vas a:

- Entender qué es un SOC y qué tipos de SOC existen.
- Ver la diferencia entre "evento", "alerta", "incidente".
- Introducir conceptos básicos de "SIEM", "EDR", "IOC", "playbooks".
- Relacionar todo esto con los logs y sistemas que ya has visto.
- Hacer laboratorios sencillos para simular el trabajo de un analista junior.

No hace falta que tengas un SIEM real en esta etapa; vamos a simular la mentalidad y el flujo usando tus propios logs y herramientas básicas. Lo importante es empezar a pensar como un analista:

- ¿Qué significa esta señal?
- ¿Qué contexto me falta?
- ¿Cómo diferencio prueba, error humano y ataque real?
- ¿Qué acción tomaría sin romper el negocio?

**Prompt IA:**  
"Actúa como instructor de Blue Team. Explica qué hace un SOC en el día a día y cómo se diferencia Blue Team de Incident Response y Threat Hunting. Incluye 10 ejemplos de tareas típicas de un analista SOC L1 y 10 de un L2. Añade una sección de 'errores típicos del junior' y cómo evitarlos."

---

## 1. Objetivos de aprendizaje de la Semana 8

Al finalizar esta semana deberías ser capaz de:

1. Explicar qué es un SOC, qué tipos hay y qué hace un analista de nivel 1/2.
2. Diferenciar entre "evento", "alerta" e "incidente".
3. Entender qué es un "SIEM" y qué papel juega en la detección.
4. Explicar, a alto nivel, qué son "EDR", "IDS/IPS", "WAF" como fuentes de información.
5. Saber qué es un "IOC (Indicator of Compromise)" y cómo se usa en análisis.
6. Comprender el flujo básico: evento → alerta → análisis → clasificación → respuesta.
7. Realizar laboratorios en tu lab para:
   - Simular eventos sospechosos.
   - "Hacer de SIEM" a pequeña escala usando grep, Python o PowerShell.
   - Documentar triage y decisiones.
8. Empezar a construir mentalidad de analista: qué preguntas hacerse ante una alerta.

A nivel profesional, estos objetivos apuntan a dos habilidades que marcan la diferencia:

- "Lectura de señales": interpretar logs y eventos sin quedarte solo en la superficie.
- "Toma de decisiones": cerrar, escalar o pedir más contexto de forma justificada.

**Prompt IA:**  
"Actúa como evaluador de aprendizaje. Crea 12 preguntas (6 tipo test y 6 abiertas) para los objetivos de la Semana 8. Incluye 3 mini-casos: fuerza bruta SSH, PowerShell sospechoso en Windows y detección de IOC en logs. Para cada pregunta indica la respuesta esperada y el fallo típico del principiante."

---

## 2. ¿Qué es un SOC (Security Operations Center)?

### 2.1. Definición

Un "SOC" es un equipo (y normalmente también un lugar físico/virtual) encargado de:

- Monitorizar la seguridad de una organización de forma continua (24/7 en muchos casos).
- Detectar actividades maliciosas o sospechosas.
- Investigar y clasificar esas actividades.
- Coordinar la respuesta ante incidentes.

Sus herramientas principales son:

- SIEM (Security Information and Event Management).
- EDR (Endpoint Detection & Response).
- IDS/IPS (Intrusion Detection/Prevention Systems).
- Firewalls, WAF, plataformas de correo, etc.

Una forma simple de entenderlo:

- El SOC vive de "telemetría" (logs y señales).
- Convierte telemetría en "decisiones" (alerta real, falso positivo, escalado).
- Y documenta todo (para trazabilidad, auditoría y mejora continua).

### 2.2. Tipos de SOC

A grandes rasgos:

- "SOC interno:"  
  Dentro de la propia empresa, con personal propio.

- "SOC externo / MSSP (Managed Security Service Provider):"  
  Empresa que da servicio de SOC a múltiples clientes.

- "Híbrido:"  
  Parte de la monitorización la hace un proveedor, y otra parte la propia empresa.

Desde el punto de vista del analista, lo importante es:

- Qué visibilidad tiene (qué logs llegan y con qué calidad).
- Qué procedimientos existen (playbooks, SLAs, escalados).
- Cómo se registran y gestionan los incidentes (ticketing, evidencias, comunicaciones).

Diferencias prácticas que notarías como analista:

- En MSSP suele haber más volumen y más estandarización.
- En SOC interno suele haber más contexto de negocio y menos "clientes distintos".
- En híbrido, la fricción típica es el "handoff": quién hace qué y cuándo.

**Prompt IA:**  
"Actúa como consultor. Explica los tipos de SOC (interno, MSSP, híbrido) con ventajas, desventajas y riesgos operativos. Incluye un ejemplo de flujo de escalado para cada tipo. Mantén el contenido orientado a un analista junior."

---

## 3. Evento, alerta, incidente: conceptos clave

En la práctica, se manejan cientos de miles de "eventos", pero solo una parte se convierten en "alertas", y de esas aún menos son "incidentes reales".

Una forma útil de visualizarlo:

- Evento: "lo que pasó"
- Alerta: "algo cumple condición sospechosa"
- Incidente: "confirmado como problema de seguridad"

### 3.1. Evento

Un evento es:

> Cualquier hecho registrado por un sistema o dispositivo.

Ejemplos:

- Inicio de sesión correcto.
- Inicio de sesión fallido.
- Conexión de red.
- Ejecución de un proceso.
- Cambio de configuración.

Los eventos son datos "en bruto". Por sí solos, muchas veces no dicen "malo" o "bueno". Necesitan contexto:

- ¿Qué usuario?
- ¿Qué equipo?
- ¿Qué hora?
- ¿Se repite?
- ¿Es normal para ese entorno?

### 3.2. Alerta

Una alerta es:

> Un evento (o conjunto de eventos) que cumple una condición definida como sospechosa o relevante.

Ejemplos:

- 10 intentos de login fallidos desde la misma IP en 5 minutos.
- Un proceso que ejecuta Powershell con parámetros extraños.
- Una conexión hacia IPs asociadas a C2 conocidos.

Las alertas suelen generarlas:

- Reglas del SIEM.
- Políticas del EDR.
- Firmas del IDS/IPS.

Una alerta siempre es una "hipótesis":

- "Esto podría ser un ataque"
- Tu trabajo es validarlo o descartarlo

### 3.3. Incidente

Un incidente es:

> Una alerta (o conjunto de alertas) que se confirma como actividad maliciosa o indeseada.

Ejemplos:

- Ransomware en ejecución en un endpoint.
- Exfiltración de datos.
- Compromiso de una cuenta administradora.

No todas las alertas son incidentes; muchos son falsos positivos o actividad legítima pero inusual.

El trabajo del analista es:

- Investigar la alerta.
- Clasificarla ("true positive" / "false positive" / "benign").
- Escalar si es necesario.

Regla mental útil:

- "True positive" no significa "catástrofe", significa "la detección era real".
- "False positive" no significa "mal SIEM", significa "regla/telemetría/contexto mejorable".

**Prompt IA:**  
"Actúa como formador SOC. Explica evento vs alerta vs incidente con 10 ejemplos concretos y su clasificación. Para cada ejemplo, indica: fuente típica (SIEM/EDR/IDS/WAF), contexto mínimo necesario y acción recomendada (cerrar, escalar, investigar más)."

---

## 4. ¿Qué es un SIEM?

### 4.1. Función del SIEM

Un "SIEM (Security Information and Event Management)":

- Recoge logs de muchos sistemas distintos:
  - Servidores (Windows/Linux).
  - Firewalls.
  - IDS/IPS.
  - Aplicaciones.
  - Plataformas cloud.

- Normaliza y correlaciona esos datos.
- Aplica reglas para generar alertas.
- Permite buscar y analizar eventos (consultas, dashboards).

En esta etapa no necesitas manejar un SIEM concreto (Splunk, Elastic, Sentinel, etc.), pero sí entender:

- Que es el "cerebro" donde se reúne la información.
- Que un analista SOC pasa buena parte del tiempo dentro del SIEM.

Detalles importantes para un junior:

- "Ingesta": que lleguen logs (sin logs no hay SOC).
- "Normalización": campos consistentes (usuario, ip, host, evento).
- "Correlación": unir señales (mismo host, mismo usuario, misma campaña).
- "Retención": cuánto tiempo de histórico hay (7 días, 30 días, 180 días).
- "Calidad": si el log es incompleto, la detección falla o genera ruido.

### 4.2. Detecciones basadas en reglas

Las reglas del SIEM suelen ser:

- Condiciones sobre campos de logs:
  - IP origen/destino.
  - Usuario.
  - Evento concreto (ID).
  - Número de repeticiones.
  - Intervalos de tiempo.

Ejemplo (conceptual):

> "Dispara alerta si hay más de 5 inicios de sesión fallidos desde la misma IP contra el mismo usuario en 10 minutos."

En tus laboratorios, vas a simular este tipo de reglas usando:

- grep en Linux.
- Python.
- PowerShell.

Un concepto clave aquí es el "equilibrio" detección vs ruido:

- Umbrales demasiado bajos = alertas constantes.
- Umbrales demasiado altos = ataques que pasan desapercibidos.

**Prompt IA:**  
"Actúa como instructor. Explica qué hace un SIEM con un ejemplo de pipeline: ingesta, normalización, correlación, detección, triage y reporting. Incluye 5 ejemplos de reglas típicas y qué campos necesitarías en los logs para que funcionen."

---

## 5. Otras piezas: EDR, IDS/IPS, WAF

### 5.1. EDR (Endpoint Detection & Response)

- Agente instalado en endpoints (PCs, servidores).
- Monitoriza:
  - Procesos.
  - Conexiones de red.
  - Actividad en memoria.
- Puede bloquear comportamientos sospechosos (ej. patrones de ransomware).
- Envía eventos/alertas al SOC.

En la mentalidad SOC:

- EDR = "lo que pasa dentro del endpoint"
- Muy útil cuando el SIEM solo tiene logs parciales o cuando necesitas evidencia del proceso padre/hijo

### 5.2. IDS/IPS

- IDS: detecta tráfico sospechoso en la red.
- IPS: además de detectar, puede bloquear.

Suelen mirar:

- Firmas conocidas de ataques.
- Patrones anómalos.

En la mentalidad SOC:

- IDS/IPS = "lo que cruza la red"
- Muy útil para ver scans, exploits a servicios, anomalías de tráfico

### 5.3. WAF (Web Application Firewall)

- Protege aplicaciones web.
- Inspecciona tráfico HTTP/HTTPS.
- Detecta y bloquea:
  - Inyección SQL.
  - XSS.
  - Otras técnicas conocidas contra aplicaciones web.

En la mentalidad SOC:

- WAF = "qué está intentando hacer el atacante contra la app"
- Muy útil para diferenciar tráfico legítimo de patrones de ataque a nivel HTTP

Para ti, por ahora, lo importante es verlos como "fuentes de señales" que llegan al SOC. En un caso real, el valor aumenta cuando puedes correlacionar:

- WAF detecta ataque a login
- EDR ve powershell raro
- SIEM une usuario + host + IP + tiempo

**Prompt IA:**  
"Actúa como analista SOC. Explica la diferencia práctica entre SIEM y EDR y cómo se complementan. Añade IDS/IPS y WAF como fuentes adicionales. Incluye 3 ejemplos de incidentes y qué verías en cada herramienta (tipo de evidencia)."

---

## 6. IOCs (Indicadores de Compromiso)

Un "IOC (Indicator of Compromise)" es un dato que indica que algo malo puede estar ocurriendo o ha ocurrido.

Ejemplos de IOCs:

- Dirección IP asociada a C2 o campañas maliciosas.
- Hash de un fichero (malware conocido).
- Nombre de dominio malicioso.
- Ruta de archivo característica de un malware.
- Cadena específica en un comando o script.

En un SOC:

- Se utilizan IOCs de fuentes de threat intelligence.
- Se comparan con los eventos en el SIEM.
- Si se detecta un IOC en tu entorno, se dispara una alerta.

En tu laboratorio:

- Puedes usar "IOCs caseros" como:
  - Ciertas IPs internas que simulan atacante.
  - Cadenas específicas en logs (por ejemplo, "MALICIOUS_TEST").

Matiz importante para pensar como analista:

- Un IOC no siempre significa "incidente confirmado".
- Un IOC puede ser:
  - True positive (real)
  - Coincidencia (falso positivo)
  - Actividad de prueba (benigno)
- El contexto y la repetición importan mucho.

**Prompt IA:**  
"Actúa como analista de threat intelligence. Explica qué es un IOC y diferencia IOC vs IOB (Indicator of Behavior) con ejemplos sencillos. Crea una lista de 15 IOCs comunes (IP, dominio, hash, ruta, user-agent, comando) y explica cómo los validarías en logs sin asumir que son 100% maliciosos."

---

## 7. Flujo básico de manejo de alertas en un SOC

Simplificado, el flujo sería:

1. "Generación" de alerta (por una regla, EDR, IDS, etc.).
2. "Recepción" por parte del analista nivel 1.
3. "Triage":
   - ¿Es real?
   - ¿Es falso positivo?
   - ¿Es benigno pero inusual?
4. "Recolección de contexto":
   - Más logs.
   - Usuario.
   - Equipo.
   - Histórico.
5. "Clasificación":
   - True Positive → Escalar a nivel 2 / responder.
   - False Positive → Cerrar y revisar regla si es necesario.
   - Benigno → Documentar y cerrar.
6. "Respuesta" (si es incidente):
   - Contención (bloquear IP, aislar equipo).
   - Erradicación (eliminar malware, cambiar credenciales).
   - Recuperación (restaurar servicio).
   - Lecciones aprendidas.

Lo que vamos a hacer esta semana es simular un trozo de este flujo a pequeña escala.

Mentalidad práctica de triage (preguntas típicas):

- ¿Qué pasó exactamente? (evento)
- ¿Por qué se alertó? (regla)
- ¿Qué tan fiable es la fuente? (telemetría)
- ¿Se repite? ¿Desde cuándo? (tendencia)
- ¿Qué impacto potencial tiene? (riesgo)
- ¿Necesito escalar o puedo cerrar con evidencia? (decisión)

**Prompt IA:**  
"Actúa como analista SOC L1. Crea una checklist de triage reutilizable para alertas genéricas. Incluye: preguntas, datos a recopilar, señales de severidad, criterios de escalado a L2 y ejemplo completado para 'fuerza bruta SSH' y 'PowerShell oculto'."

---

## 8. Laboratorio de la Semana 8

### 8.1. Escenario de trabajo

- Tus máquinas (Ubuntu, Windows, Kali) generan eventos.
- Tú vas a actuar como un mini-SIEM + analista.
- Harás:
  - Generación controlada de eventos "raros".
  - Búsqueda de esos eventos con scripts sencillos.
  - Pequeño triage y clasificación.

Objetivo real del lab:

- Aprender a transformar un "hecho" en "evidencia".
- Aprender a justificar una clasificación.
- Aprender a proponer acciones sin pasarte de agresivo.

**Prompt IA:**  
"Actúa como instructor. Propón un laboratorio seguro de Blue Team para simular 3 tipos de alertas (autenticación, ejecución de procesos, IOC en logs) usando solo acciones no destructivas. Incluye qué evidencias capturar y cómo documentar resultados en Markdown."

---

### 8.2. Ejercicio 1 – Simular intentos de login sospechosos en Linux

En "Ubuntu":

1. Asegúrate de tener SSH activo.
2. Desde "Kali", lanza varios intentos de login fallidos a un usuario inventado:

   ssh usuario_inexistente@IP_DE_UBUNTU

   Hazlo varias veces (por ejemplo, 5-10 intentos).

3. En Ubuntu, haz una copia del log de autenticación:

   sudo cp /var/log/auth.log ~/scripts_seguridad/auth_log_semana8.log  
   sudo chown "$USER":"$USER" ~/scripts_seguridad/auth_log_semana8.log

4. Usa un script en Python o Bash (puedes reutilizar lo de Semana 6) para:

   - Contar cuántas líneas contienen "Failed password" desde una misma IP.
   - Considerar como "alerta" si hay más de X intentos (ej. 3).

Crea un archivo:

"05-BlueTeam-SOC-Semana_8_LoginSSH_Alerta.md"

Incluye:

- Descripción de cómo generaste los intentos.
- Extracto de las líneas relevantes del log.
- Lógica de "regla" que has aplicado (más de N intentos = alerta).
- ¿Sería esto una alerta de fuerza bruta? ¿En qué condiciones?

Ampliación recomendada (para subir nivel sin complicarte):

- Añade "ventana temporal": ¿ocurre en 1 minuto o en 8 horas?
- Añade "usuario objetivo": ¿es admin? ¿es inexistente?
- Añade "tendencia": ¿esa IP ya lo intentó antes?

**Prompt IA:**  
"Actúa como analista SOC. Diseña una regla conceptual para detectar fuerza bruta SSH basada en logs de auth.log. Define: umbral, ventana temporal, campos a extraer (usuario, IP, timestamp) y cómo reducir falsos positivos. Devuelve un ejemplo de reporte en Markdown con secciones de evidencia y conclusión."

---

### 8.3. Ejercicio 2 – Simular evento sospechoso en Windows (PowerShell)

En tu máquina "Windows":

1. Ejecuta un comando de PowerShell con parámetros poco habituales (similar a Semana 3, pero más enfocado a detección), por ejemplo:

   powershell.exe -NoProfile -WindowStyle Hidden -Command "Write-Output 'TestMaliciousCommand'"

2. Abre el "Visor de eventos" y busca:
   - Registros en "Windows PowerShell" o "Microsoft-Windows-PowerShell/Operational" (si está habilitado).
   - Si no lo tienes habilitado, puedes usar "Get-EventLog" desde PowerShell.

3. Observa:
   - Qué EventID aparece.
   - Qué información contiene el evento (comando ejecutado, usuario, hora).

Crea:

"05-BlueTeam-SOC-Semana_8_PowerShell_Evento.md"

Incluye:

- Comando ejecutado.
- EventID encontrado.
- Descripción de por qué esto podría considerarse sospechoso (PowerShell en modo oculto, por ejemplo).
- Regla conceptual: "disparar alerta si se ejecuta PowerShell con WindowStyle Hidden" (aunque no la implementes de verdad todavía).

Ampliación recomendada:

- Define el "contexto benigno": ¿hay scripts internos que lo hacen?
- Define el "contexto sospechoso": ¿aparece junto con conexiones externas? ¿descargas?

**Prompt IA:**  
"Actúa como analista Blue Team. Explica por qué PowerShell con 'NoProfile' y 'WindowStyle Hidden' es un patrón frecuente en ataques. Propón 6 señales adicionales que buscarías (proceso padre, usuario, hora, rutas, conexiones, scriptblock logging) y cómo documentarlas en un ticket SOC."

---

### 8.4. Ejercicio 3 – "Mini SIEM" con script (detección basada en IOC sencillo)

Vamos a usar un IOC inventado.

En "Ubuntu":

1. Abre o crea un archivo de log de prueba, por ejemplo:

   echo "Normal log line" > ~/scripts_seguridad/log_ioc_test.log  
   echo "Connection from 10.10.10.10 - MALICIOUS_TEST" >> ~/scripts_seguridad/log_ioc_test.log  
   echo "Another normal line" >> ~/scripts_seguridad/log_ioc_test.log

   Considera que "MALICIOUS_TEST" es tu IOC (indicador de prueba).

2. Crea un script Python llamado "detectar_ioc.py":

   # detectar_ioc.py  
   RUTA_LOG = "log_ioc_test.log"  
   IOC = "MALICIOUS_TEST"  
   with open(RUTA_LOG, "r", encoding="utf-8") as f:  
       for numero_linea, linea in enumerate(f, start=1):  
           if IOC in linea:  
               print(f"[ALERTA IOC] Línea {numero_linea}: {linea.strip()}")

3. Ejecuta el script y observa:

   python3 detectar_ioc.py

4. Imagina que esto forma parte de un proceso que revisa muchos logs buscando ese IOC.

Crea:

"05-BlueTeam-SOC-Semana_8_IOC_MiniSIEM.md"

Incluye:

- Descripción de tu IOC inventado.
- Código del script (o la parte relevante).
- Resultado de la detección.
- Reflexión: ¿cómo podría integrarse esta lógica en un SIEM real? (comparando IOCs de Threat Intel con los eventos ingeridos).

Ampliación recomendada:

- Añade "lista de IOCs" (varios) en un fichero y que el script los lea.
- Añade salida tipo "resumen": cuántas coincidencias y en qué líneas.

**Prompt IA:**  
"Actúa como desarrollador de herramientas SOC. Propón una evolución del mini script de IOC: leer lista de IOCs desde archivo, soportar varios logs, generar resumen (conteos por IOC), y producir salida en formato fácil de pegar en un ticket. Devuelve pseudocódigo y un ejemplo de salida esperada."

---

### 8.5. Ejercicio 4 – Triage y clasificación de "alertas"

Ahora, vas a actuar como analista de nivel 1.

1. Reúne las "alertas" generadas en los ejercicios anteriores:
   - Intentos repetidos de login SSH.
   - Ejecución sospechosa de PowerShell.
   - Detección de IOC en log.

2. Para cada una, responde en un fichero:

"05-BlueTeam-SOC-Semana_8_Triage_Alertas.md"

Estructura sugerida:

Para cada alerta:

- "Descripción:"  
  Ej: "Varios intentos fallidos de SSH desde la IP 192.168.56.50 hacia el usuario 'usuario_inexistente'."

- "Fuente de datos:"  
  Ej: "/var/log/auth.log", Event Viewer, log_ioc_test.log.

- "Contexto adicional que buscarías:"  
  - ¿Solo ocurre una vez?
  - ¿Es un patrón repetido a lo largo del tiempo?
  - ¿Afecta a usuarios críticos?
  - ¿Coincide con mantenimiento o actividad esperada?

- "Clasificación:"  
  - True positive (actividad maliciosa real en el contexto del lab).
  - False positive (resultado de pruebas controladas).
  - Benigno.

- "Severidad:"  
  (Alta/Media/Baja en el contexto de tu laboratorio).

- "Acciones sugeridas:"  
  - En un entorno real, ¿bloquearías IP? ¿Investigarías más? ¿Solo documentarías?

Este ejercicio es muy representativo de lo que hace un analista SOC en su día a día.

Ampliación recomendada (muy útil para mentalidad SOC):

- Añade "decisión y justificación": una frase clara del tipo:
  - "Cierro como FP porque..."
  - "Escalo porque..."
- Añade "siguientes pasos si se repite": qué harías si vuelve mañana

**Prompt IA:**  
"Actúa como analista SOC L1. Dado un conjunto de 3 alertas (SSH brute force, PowerShell oculto, IOC match), genera un triage completo con: hipótesis, evidencia mínima, contexto a buscar, clasificación, severidad, acciones recomendadas y criterios de escalado a L2. Devuelve el resultado en Markdown listo para un ticket."

---

## 9. Entregables de la Semana 8

Al finalizar la semana deberías tener:

1. "05-BlueTeam-SOC-Semana_8_LoginSSH_Alerta.md"  
   - Simulación de intentos de login sospechosos.
   - Lógica de alerta y reflexión sobre fuerza bruta.

2. "05-BlueTeam-SOC-Semana_8_PowerShell_Evento.md"  
   - Evento de PowerShell sospechoso.
   - Análisis de por qué podría ser una alerta.

3. "05-BlueTeam-SOC-Semana_8_IOC_MiniSIEM.md"  
   - Script e idea de detección basada en IOC.
   - Comentarios sobre integración en un SIEM real.

4. "05-BlueTeam-SOC-Semana_8_Triage_Alertas.md"  
   - Triage de las alertas simuladas.
   - Clasificación y acciones sugeridas.

Opcionalmente, puedes añadir una sección en tu "Lab_Setup_Report.md" llamada:

- "Perspectiva Blue Team", donde resumas:
  - Qué fuentes de logs tienes.
  - Qué tipos de detecciones podrías montar con ellas.

Consejo de calidad de repo:

- Cada entregable debería terminar con "Conclusión" y "Siguientes pasos".
- Si incluyes evidencia, recorta a lo necesario (no pegues 500 líneas).

**Prompt IA:**  
"Actúa como revisor de repositorio. Propón una estructura de carpetas y un estándar de documentación para la Semana 8 (y futuras): naming, secciones mínimas por fichero, cómo incluir evidencias, y cómo redactar conclusiones y próximos pasos."

---

## 10. Checklist de cierre de la Semana 8

Marca como completado cuando:

- [ ] Puedes explicar qué es un SOC, qué hace y qué herramientas usa a alto nivel.
- [ ] Diferencias claramente entre evento, alerta e incidente.
- [ ] Entiendes el papel del SIEM y otras herramientas (EDR, IDS/IPS, WAF) como fuentes de información.
- [ ] Sabes qué es un IOC y puedes dar ejemplos.
- [ ] Has simulado al menos:
  - Intentos de login sospechosos en Linux.
  - Un evento sospechoso de PowerShell en Windows.
  - Una detección basada en IOC usando un script.
- [ ] Has realizado un triage básico y clasificado alertas simuladas.
- [ ] Tienes todos los ficheros de la Semana 8 en tu repositorio.

Si has completado esta semana, ya has tocado los tres "mundos":

- "Sistemas y red."
- "Ofensiva controlada (hacking ético)."
- "Defensiva y análisis (Blue Team / SOC)."

En las próximas semanas podrás profundizar en cada rama (SOC, pentest, automatización avanzada, cloud security) según el enfoque que quieras dar a tu perfil, pero la base que estás construyendo aquí es justo la que muchos juniors no tienen tan clara al principio.

**Prompt IA:**  
"Actúa como evaluador final. Crea un checklist ampliado con criterios observables (qué debe verse en el repo) para validar la Semana 8. Añade 8 preguntas de reflexión orientadas a SOC: qué contexto pedirías, qué reglas mejorarías y qué acciones tomarías sin impactar la operativa."

---

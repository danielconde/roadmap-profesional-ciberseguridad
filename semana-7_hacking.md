# Semana 7 – Fundamentos de Hacking Ético 🕵️‍♂️💻

## 0. Introducción a la semana

En este punto del roadmap ya tienes:

- Un laboratorio funcional (Kali, Windows, Ubuntu, red interna, algo de cloud).
- Fundamentos de ciberseguridad (CIA, amenazas, riesgo, Kill Chain, MITRE).
- Base sólida de redes (IP, puertos, protocolos, nmap, Wireshark).
- Conocimiento básico de Linux y Windows orientado a seguridad.
- Nociones de datos, copias de seguridad, identidad e IAM.
- Primeros pasos en automatización y scripting (Bash, Python, PowerShell + IA).

Esta Semana 7 es el punto en el que empezamos a mirar el mundo desde el otro lado:

> ¿Qué pasa si piensas como atacante, pero con objetivos defensivos y en un entorno controlado?

El objetivo no es que te conviertas en "hacker" en el sentido sensacionalista, sino que entiendas:

- Cómo se estructura un "test de intrusión (pentest)".
- Qué fases hay desde el reconocimiento hasta la explotación y post-explotación.
- Qué herramientas se usan en cada fase (a nivel básico).
- Cómo esa mentalidad ofensiva te ayuda luego a defender mejor (Blue/Purple Team).

En la práctica, cuando entiendes cómo se ataca algo:

- Entiendes por qué existe una medida defensiva (no es "por cumplir").
- Sabes priorizar riesgos (qué es realmente explotable y qué es ruido).
- Puedes mejorar detecciones (sabes qué señales deja el atacante).
- Puedes explicar impacto con claridad (técnico y de negocio).

⚠️ "Aviso ético y legal:"  
Todo lo que aprendas aquí debe usarse "solo" en:

- Tu laboratorio.
- Sistemas de los que seas propietario.
- Sistemas para los que tengas autorización formal (por escrito).

Nunca lo apliques contra objetivos reales sin permiso.

**Prompt IA:**  
"Actúa como instructor de ciberseguridad y pentesting para principiantes. Explica qué cambia cuando piensas como atacante de forma ética. Incluye 8 ejemplos de cómo la mentalidad ofensiva mejora el trabajo defensivo. Añade una mini-sección de 'errores comunes del principiante' y cómo evitarlos en un laboratorio."

---

## 1. Objetivos de aprendizaje de la Semana 7

Al finalizar esta semana deberías ser capaz de:

1. Explicar qué es el "hacking ético" y en qué se diferencia de la actividad ilegal.
2. Describir las fases clásicas de un test de intrusión:
   - Reconocimiento (OSINT, info pasiva).
   - Escaneo y enumeración.
   - Explotación.
   - Post-explotación.
   - Informe.
3. Conectar estas fases con el modelo de Kill Chain y MITRE ATT&CK que ya conoces.
4. Utilizar Kali Linux para:
   - Hacer un reconocimiento básico de tu laboratorio.
   - Lanzar escaneos más estructurados con nmap.
   - Enumerar servicios y versiones.
5. Entender, a nivel conceptual, qué significa explotación y post-explotación sin necesidad de usar exploits avanzados todavía.
6. Documentar tus descubrimientos en tu repositorio como lo haría un pentester junior.

Estos objetivos tienen un enfoque "realista": el pentesting no es solo "encontrar algo vulnerable", es seguir un proceso, generar evidencia y comunicarlo. Incluso en un laboratorio, si documentas bien, estás construyendo una habilidad profesional.

**Prompt IA:**  
"Actúa como evaluador del aprendizaje. Crea 12 preguntas (6 tipo test y 6 abiertas) para comprobar los objetivos de la Semana 7. Incluye 2 mini-casos: uno con un host Linux con SSH y HTTP abiertos, y otro con un host Windows con RDP visible. Para cada pregunta indica respuesta esperada y un error típico de principiante."

---

## 2. ¿Qué es el hacking ético?

### 2.1. Definición clara

El "hacking ético" es la práctica de:

- Identificar vulnerabilidades.
- Intentar explotarlas de forma controlada.
- Demostrar el impacto que tendrían para la organización.

...con el objetivo de:

- Mejorar la seguridad.
- Prevenir ataques reales.

Y siempre bajo:

- Autorización explícita.
- Alcance (scope) definido.
- Condiciones acordadas (horarios, sistemas, técnicas permitidas).

Una idea clave: un pentest no es "romper por romper". Es validar si una amenaza es posible y medir cuánto daño podría hacer en un escenario realista. Por eso el pentest tiene límites: no se trata de destruir, se trata de demostrar.

### 2.2. Diferencia con ataques maliciosos

La diferencia no está en las técnicas (que pueden ser similares), sino en:

- La "intención" (ayudar vs dañar o lucrarse ilegalmente).
- El "contexto" (entorno controlado vs objetivo sin permiso).
- La "formalización" (contrato, NDA, scope).

Un atacante:

- No tiene autorización.
- Busca beneficio propio (económico, reputacional, ideológico...).
- No informa a la víctima.

Un hacker ético / pentester:

- Tiene autorización.
- Informa de los hallazgos.
- Ayuda a corregirlos.

En un entorno profesional, además, se trabaja con principios de minimización:

- Minimizar impacto.
- Minimizar exposición de datos.
- Minimizar cambios permanentes.
- Maximizar evidencia y reproducibilidad.

**Prompt IA:**  
"Actúa como consultor de ciberseguridad. Explica la diferencia entre hacking ético y hacking malicioso con 5 escenarios prácticos. En cada escenario indica: intención, permiso, límites, evidencias que se recogen y cómo se reporta. Mantén el lenguaje claro y orientado a un junior."

---

## 3. Ética, legalidad y "scope"

Antes de tocar herramientas ofensivas, hay tres conceptos clave:

### 3.1. Autorización

Nunca pruebes técnicas de hacking:

- En sistemas de terceros sin permiso.
- En webs aleatorias.
- En redes públicas.

En este curso, tu "cliente" es tu "laboratorio".

En entornos reales, la autorización se define de forma explícita. Sin esa autorización, aunque la intención sea "aprender", estás entrando en una zona ilegal y además peligrosa: podrías provocar indisponibilidad, alertas, bloqueos o incluso daños.

### 3.2. Alcance (scope)

En un pentest real, el scope define:

- Qué sistemas se pueden atacar (IPs, dominios).
- Qué técnicas están permitidas o vetadas.
- Qué horarios son aceptables (evitar impactos de negocio).

En tu lab, puedes definir un scope para ti mismo. Por ejemplo:

- Alcance:
  - Ubuntu Server en la red Host-Only.
  - Windows como estación de trabajo objetivo.
- Técnicas:
  - Escaneo de puertos, enumeración de servicios.
  - Autenticaciones de prueba con usuarios creados específicamente.
- No hacer:
  - Ataques destructivos al sistema host.
  - Borrar datos críticos de tu lab si no tienes snapshot.

Un scope claro también te entrena a "no dispersarte". Un pentester junior suele querer probarlo todo. En la realidad, se prioriza por:

- Superficie expuesta.
- Riesgo.
- Impacto potencial.
- Tiempo disponible.

### 3.3. Informe y responsabilidad

Un hacker ético no solo "rompe cosas"; documenta:

- Qué encontró.
- Cómo lo explotó.
- Impacto.
- Recomendaciones.

Este hábito lo empezaremos a practicar con mini-informes en tus ficheros ".md".

La documentación es parte del trabajo. Si no puedes explicar y reproducir lo que hiciste, tu hallazgo pierde valor. Un reporte sólido permite:

- Reproducir en un entorno controlado.
- Corregir la causa raíz.
- Validar que el fix funciona.
- Mejorar controles defensivos (detección y respuesta).

**Prompt IA:**  
"Actúa como pentester senior. Genera una plantilla de 'scope' para un mini-pentest de laboratorio en Markdown. Incluye secciones de autorización, alcance, fuera de alcance, reglas de engagement, ventanas horarias, limitaciones, criterios de éxito y cómo manejar evidencia. Hazlo usable por un junior."

---

## 4. Fases de un test de intrusión (visión global)

Aunque hay distintos modelos, una estructura clásica es:

1. "Reconocimiento / Inteligencia (OSINT)"
2. "Escaneo y enumeración"
3. "Explotación"
4. "Post-explotación"
5. "Informe"

Un matiz importante: estas fases no siempre son lineales. En la práctica, vuelves atrás:

- Escaneas, enumeras, descubres un servicio nuevo.
- Vuelves a enumerar ese servicio en más detalle.
- Encuentras credenciales o una vía, vuelves a validar el alcance.
- Documentas sobre la marcha para no perder evidencia.

### 4.1. Reconocimiento / OSINT

En un entorno real:

- Recoger información pública sobre la organización:
  - Dominios.
  - Subdominios.
  - Tecnologías visibles (cabeceras HTTP, certificados).
  - Empleados en redes sociales, etc.

En tu laboratorio:

- Puedes hacer una versión reducida:
  - Identificar IPs activas.
  - Descubrir servicios visibles.
  - Ver banners y cabeceras de servicios.

Objetivo de esta fase: reducir incertidumbre. Cuanto mejor reconozcas, mejor decidirás dónde enfocar energía.

### 4.2. Escaneo y enumeración

Una vez sabes qué objetivos hay, empiezas a:

- Escanear puertos.
- Identificar servicios.
- Ver versiones.
- Enumerar posibles vectores (por ejemplo, un servidor web con rutas interesantes).

Herramientas típicas en esta fase:

- "nmap", "masscan" (escaneo de puertos).
- Herramientas de enumeración específicas (por ejemplo, en web: gobuster, ffuf).

Diferencia práctica:

- Escaneo: "qué hay abierto"
- Enumeración: "qué es exactamente y cómo se comporta"

### 4.3. Explotación

En esta fase se intenta:

- Aprovechar vulnerabilidades:
  - Desbordamientos.
  - Inyecciones.
  - Configuraciones débiles.
- O credenciales débiles / reutilizadas.

En laboratorio:

- Empezaremos muy suave: sin explotar CVEs complejas, más bien entendiendo la lógica.
- Más adelante podrás usar máquinas vulnerables específicas (como VMs diseñadas para CTFs).

Un punto clave para hacerlo bien: explotación ética no significa "arrasar". Significa:

- Probar la hipótesis.
- Conseguir evidencia mínima suficiente.
- Parar y reportar.

### 4.4. Post-explotación

Una vez consigues acceso:

- Elevar privilegios.
- Mantener persistencia.
- Recopilar evidencia de impacto (pero sin destruir).
- En un entorno ético, se limita el daño y se documenta.

En tu lab, esta fase se va a tratar de manera conceptual y muy básica, porque lo importante ahora es entender:

- Qué buscaría un atacante después de entrar.
- Qué datos indicarían que algo así ha pasado.
- Qué controles defensivos serían relevantes (logs, alertas, hardening).

### 4.5. Informe

Finalmente:

- Se documentan hallazgos.
- Se explica el impacto.
- Se dan recomendaciones de mitigación.

En este curso, cada semana tienes ya una mini-hábito de documentación. Aquí lo convertimos en algo más parecido a un reporte de pentest.

**Prompt IA:**  
"Actúa como instructor. Explica las fases de un pentest con ejemplos prácticos en un laboratorio (Kali atacante, Ubuntu objetivo, Windows secundario). Para cada fase indica: objetivo, herramientas básicas, evidencia a recolectar y qué NO hacer para mantener ética y control."

---

## 5. Conexión con Kill Chain y MITRE ATT&CK

Todo esto encaja con conceptos previos:

- Reconocimiento ↔ Recon de Kill Chain, tácticas de "Reconnaissance" en MITRE.
- Escaneo / enumeración ↔ Discovery, explotación ↔ "Execution", "Privilege Escalation".
- Post-explotación ↔ Lateral Movement, Collection, Exfiltration.
- Informe ↔ Lecciones aprendidas / mejora continua (defensiva).

Comprender el ataque te ayuda a:

- Diseñar mejores detecciones.
- Entender por qué un determinado log es relevante.
- Priorizar qué configurar o parchear antes.

Ejemplo mental útil: cada paso ofensivo deja señales defensivas potenciales:

- Escaneo: picos de conexiones a muchos puertos.
- Enumeración: consultas repetitivas, patrones de requests.
- Explotación: payloads, errores específicos, comportamientos raros.
- Post-explotación: nuevos procesos, conexiones salientes, credenciales tocadas.

No necesitas memorizarlo todo. Solo interiorizar que ataque y defensa son dos caras del mismo sistema.

**Prompt IA:**  
"Actúa como analista Purple Team. Mapea las fases de pentest a Kill Chain y MITRE ATT&CK con un ejemplo sencillo (escaneo, enumeración SSH/HTTP, acceso inicial hipotético). Para cada fase, indica 3 señales defensivas que un SOC podría detectar y 2 medidas de mitigación."

---

## 6. Herramientas de Kali para esta semana (nivel básico)

En esta semana usaremos, sobre todo:

- "nmap" → escaneo y enumeración de servicios.
- "curl / wget" → interactuar con servicios web y ver banners.
- Utilidades OSINT muy básicas (solo en tu lab, por ahora).

Más adelante podrás introducirte en:

- "gobuster/ffuf" para descubrimiento de directorios.
- "hydra" para pruebas controladas de contraseñas en tu lab.

Para esta semana, vamos a mantenerlo contenible y muy controlado.

Importante: aunque las herramientas sean conocidas, la habilidad real es saber:

- Cuándo usar cada una.
- Qué output es relevante.
- Cómo convertir output en evidencia y recomendación.

**Prompt IA:**  
"Actúa como instructor de Kali Linux. Explica el propósito de nmap, curl y wget en un mini pentest. Incluye ejemplos de 'qué buscar' en la salida (puertos, servicios, banners, cabeceras) y cómo registrar evidencia en un reporte. Evita instrucciones ofensivas avanzadas y mantén enfoque de laboratorio."

---

## 7. Laboratorio de la Semana 7

Vamos a enfocar el laboratorio como un "mini pentest interno" en tu propia red Host-Only.

La idea no es hacer algo agresivo, sino construir un flujo profesional:

- Definir alcance.
- Descubrir activos.
- Enumerar servicios.
- Razonar riesgos.
- Documentar hallazgos y recomendaciones.

**Prompt IA:**  
"Actúa como instructor. Diseña una guía de laboratorio para un mini pentest interno Host-Only con Kali atacante y Ubuntu/Windows objetivos. Incluye pasos, validaciones, evidencias a capturar y cómo redactar conclusiones. Mantén el laboratorio seguro y controlado."

### 7.1. Escenario de laboratorio

- Máquina atacante: "Kali Linux".
- Objetivo principal: "Ubuntu Server".
- Objetivo secundario: "Windows" (a nivel muy básico).

Supón que:

- Tienes autorización total en este entorno (es tu lab).
- El "scope" es: toda la red Host-Only, pero sin destruir datos.

Un detalle útil: si tienes snapshots de tus VMs, este es un buen momento para:

- Hacer snapshot antes de empezar.
- Trabajar sin miedo a romper algo accidentalmente.
- Volver atrás si te equivocas.

---

### 7.2. Ejercicio 1 – Definir "scope" y reglas de juego

Crea un fichero:

"04-Hacking-Etico-Semana_7_Scope.md"

Incluye:

1. Alcance:
   - Qué máquinas puedes atacar (IPs, nombres).
   - Qué servicios crees que pueden ser objetivos (SSH, HTTP, etc.).
2. Acciones permitidas:
   - Escaneo de puertos.
   - Enumeración de servicios.
   - Conexiones de prueba.
3. Acciones no permitidas:
   - Borrar ficheros del sistema.
   - Formatear discos.
   - Romper totalmente el sistema host.
4. Objetivo del ejercicio:
   - Descubrir qué servicios expone tu Ubuntu y tu Windows.
   - Pensar qué riesgos tendrían esos servicios si estuvieran expuestos a Internet.

Este documento imita, en pequeño, un documento de alcance de pentest.

Consejo: añade una mini-sección de "criterios de éxito":

- "He identificado IPs y puertos"
- "He enumerado al menos 2 servicios"
- "He redactado 3 recomendaciones aplicables"

**Prompt IA:**  
"Actúa como pentester junior creando un documento de scope. Redacta un ejemplo completo para un lab con Kali atacante, Ubuntu objetivo principal y Windows secundario, en red Host-Only. Incluye fuera de alcance, reglas de engagement, limitaciones y criterios de éxito. Devuélvelo en Markdown."

---

### 7.3. Ejercicio 2 – Escaneo estructurado con nmap

En tu "Kali", desde la terminal:

1. Identifica la subred Host-Only (por ejemplo "192.168.56.0/24").
2. Haz un escaneo de descubrimiento de hosts:

   nmap -sn 192.168.56.0/24

   Esto hará un "ping scan" para ver qué máquinas están activas.

3. Anota qué IP corresponde a cada máquina (Kali, Ubuntu, Windows).
4. Haz un escaneo de puertos para "Ubuntu":

   nmap -sV IP_DE_UBUNTU

   Explicación:
   - "-sV" intenta identificar el servicio y la versión.

5. Haz un escaneo menos intrusivo para "Windows":

   nmap -sV -Pn IP_DE_WINDOWS

   - "-Pn" para omitir ping si tienes problemas de respuesta.

En tu repositorio, crea:

"04-Hacking-Etico-Semana_7_nmap_Resultados.md"

Incluye:

- Salida relevante de nmap (recortada, no hace falta todo bruto).
- Tabla simple:

  | Máquina  | IP             | Puertos abiertos | Servicios detectados  |
  |----------|----------------|------------------|-----------------------|
  | Ubuntu   | x.x.x.x        | 22, 80, ...      | SSH, HTTP, ...        |
  | Windows  | x.x.x.x        | 3389, ...        | RDP, ...              |

- Comentarios:
  - ¿Algo te sorprende?
  - ¿Qué servicio crees que sería más crítico si estuviera expuesto a Internet?

Pistas para tu comentario (sin hacer explotación):

- Puertos administrativos (SSH, RDP) suelen ser críticos si están accesibles desde redes no confiables.
- Servicios web suelen ser críticos por exposición y complejidad.
- Cuantos más puertos abiertos, más superficie de ataque (aunque depende del contexto).

**Prompt IA:**  
"Actúa como analista de resultados de nmap. Dado un listado de puertos abiertos y servicios, genera: un resumen ejecutivo, una tabla de hallazgos por host, una explicación del riesgo por servicio (bajo/medio/alto) y recomendaciones básicas de hardening. Pide explícitamente los datos de salida de nmap como input."

---

### 7.4. Ejercicio 3 – Enumeración básica de servicios

El objetivo aquí no es explotar nada, sino "entender mejor" qué hay detrás de esos puertos abiertos.

#### 7.4.1. SSH (Linux)

Si tu Ubuntu tiene el puerto 22 abierto:

1. Desde Kali, intenta conectarte:

   ssh usuario@IP_DE_UBUNTU

   (Usa un usuario que ya tengas creado, por ejemplo "usuario1" de semanas anteriores).

2. Si el login funciona:
   - Estás viendo cómo un atacante podría intentar fuerza bruta si las contraseñas fueran débiles.
   - No ejecutes nada dañino: tu objetivo es comprender el flujo.

3. Si no funciona:
   - Observa el mensaje (puede ayudar a un atacante a saber si el usuario existe).

Qué documentar:

- Si el servicio responde rápido o lento.
- Si hay banner o mensajes informativos.
- Si el sistema da pistas (por ejemplo: "user invalid" o "password incorrect").

#### 7.4.2. HTTP (si tienes servidor web)

Si tienes un servidor web en Ubuntu (nginx, apache):

1. Desde Kali:

   curl -I http://IP_DE_UBUNTU

   Esto mostrará solo las cabeceras HTTP.

2. Observa:

   - Cabecera "Server:" (a veces revela versión de servidor).
   - Otros metadatos.

Reflexión:

- ¿Crees que es buena idea exponer versiones exactas de software?
- ¿Cómo podría usar eso un atacante?

Qué documentar:

- Cabeceras relevantes.
- Código de respuesta (200, 301, 403, etc.).
- Si hay redirecciones o comportamientos extraños.

#### 7.4.3. Servicios en Windows

Si en el escaneo nmap viste, por ejemplo, RDP (3389):

- No es necesario que explotes nada, solo reflexiona:
  - Si este puerto estuviera abierto a Internet con una contraseña débil, ¿qué pasaría?
  - ¿Cómo lo verías desde un punto de vista de hardening?

Añade tus notas de enumeración a:

"04-Hacking-Etico-Semana_7_Enumeracion_Servicios.md"

**Prompt IA:**  
"Actúa como pentester junior. Genera una plantilla Markdown para documentar enumeración de servicios (SSH, HTTP y RDP). Incluye campos de: evidencia (output), observación técnica, riesgo potencial, mitigación recomendada, y notas para Blue Team (logs a revisar)."

---

### 7.5. Ejercicio 4 – "Threat modeling" sencillo a partir de lo que ves

Con lo que has recogido de nmap y enumeración, crea un fichero:

"04-Hacking-Etico-Semana_7_Modelado_Amenazas.md"

En él, describe para "Ubuntu" y "Windows":

1. Superficie de ataque:
   - ¿Qué servicios están expuestos?
   - ¿Qué puertos escuchan?

2. Posibles amenazas:
   - Contraseñas débiles.
   - Versiones desactualizadas de servicios.
   - Configuraciones por defecto.

3. Recomendaciones básicas:
   - Cambiar puertos por defecto (no siempre es seguridad real, pero puede ayudar).
   - Deshabilitar servicios que no se usan.
   - Aplicar actualizaciones.
   - Restringir acceso a ciertos puertos solo desde determinadas IPs.

Este ejercicio te pone ya en modo "pentester + consultor", no solo técnico.

Pistas de modelado (nivel junior):

- Si el servicio es administrativo, prioriza:
  - Restricción por IP, VPN, MFA si aplica
  - Contraseñas fuertes, bloqueo de intentos
- Si el servicio es web, prioriza:
  - Actualizaciones, hardening, cabeceras, WAF si aplica, logs
- Si hay servicios que no necesitas, prioriza:
  - Deshabilitar (reduce superficie y mantenimiento)

**Prompt IA:**  
"Actúa como consultor de seguridad. A partir de una lista de puertos y servicios por host (Ubuntu y Windows), genera un threat modeling simple: activos, superficies, amenazas plausibles, impacto (CIA), probabilidad cualitativa y 8 recomendaciones priorizadas. Devuélvelo en Markdown y en tono profesional."

---

### 7.6. Ejercicio 5 – Mini "informe de pentest" interno

Por último, genera un pequeño informe interno de tu "pentest de laboratorio" en:

"04-Hacking-Etico-Semana_7_Informe_MiniPentest.md"

Estructura sugerida:

1. "Introducción"
   - Objetivo del ejercicio (pentest interno en el laboratorio).
   - Alcance (máquinas incluidas).

2. "Metodología"
   - Escaneo con nmap.
   - Enumeración con SSH/curl.
   - Análisis de superficie.

3. "Hallazgos" (aunque sean "inventados" para simular un caso real)
   - Ejemplo:
     - "SSH accesible con usuario 'usuario1' y password simple -> riesgo alto si estuviera en Internet."
   - "Servidor web muestra versión exacta de Apache."

4. "Riesgos potenciales"
   - Qué podría pasar si un atacante externo encontrara esos servicios.

5. "Recomendaciones"
   - Fortalecer contraseñas.
   - Restringir puertos.
   - Actualizar servicios.
   - Implementar logging y monitorización.

Aunque sea un informe sencillo, esto te acostumbra al formato profesional.

Consejo para hacerlo más real:

- Añade severidad (Baja/Media/Alta) por hallazgo.
- Añade evidencia mínima (qué lo demuestra).
- Añade recomendación clara y accionable.

**Prompt IA:**  
"Actúa como redactor de informe de pentest. Genera un mini-informe completo en Markdown con secciones: introducción, alcance, metodología, hallazgos (con severidad), evidencia, impacto, recomendaciones priorizadas, y conclusiones. Mantén el informe orientado a laboratorio y sin instrucciones ofensivas avanzadas. Pide como input la tabla de puertos/servicios y notas de enumeración."

---

## 8. Entregables de la Semana 7

Al finalizar la semana deberías tener:

1. "04-Hacking-Etico-Semana_7_Scope.md"  
   - Alcance definido de tu pentest de laboratorio.

2. "04-Hacking-Etico-Semana_7_nmap_Resultados.md"  
   - Resultados resumidos de escaneos nmap.
   - Tabla de servicios por máquina.
   - Comentarios sobre criticidad.

3. "04-Hacking-Etico-Semana_7_Enumeracion_Servicios.md"  
   - Detalles de enumeración de SSH, HTTP u otros servicios visibles.
   - Notas sobre banners, versiones, etc.

4. "04-Hacking-Etico-Semana_7_Modelado_Amenazas.md"  
   - Superficie de ataque de cada máquina.
   - Posibles amenazas.
   - Recomendaciones iniciales.

5. "04-Hacking-Etico-Semana_7_Informe_MiniPentest.md"  
   - Informe breve simulando un pentest interno.
   - Estructura similar a la de un informe profesional.

Recomendación de organización del repo:

- Crea una carpeta por semana o por bloque (si no lo haces ya).
- Asegúrate de que cada entregable tenga:
  - Fecha
  - Evidencia recortada
  - Conclusiones cortas y claras

**Prompt IA:**  
"Actúa como revisor de repositorio GitHub de un alumno. Propón mejoras concretas para que los entregables de la Semana 7 queden profesionales: estructura, naming, secciones mínimas, cómo incluir evidencias sin exponer datos sensibles, y cómo escribir recomendaciones accionables."

---

## 9. Checklist de cierre de la Semana 7

Marca como completado cuando:

- [ ] Puedes explicar qué es hacking ético y en qué se diferencia de un ataque ilegal.
- [ ] Entiendes las fases básicas de un test de intrusión (reconocimiento, escaneo, explotación, post-explotación, informe).
- [ ] Has utilizado nmap para hacer un escaneo estructurado de tu laboratorio.
- [ ] Has hecho una enumeración básica de servicios (SSH, HTTP, etc.) en Ubuntu y, si aplica, en Windows.
- [ ] Has razonado sobre amenazas y riesgos a partir de la superficie expuesta.
- [ ] Has creado un pequeño informe de "mini pentest" interno.
- [ ] Tienes todos los ficheros de la Semana 7 en tu repositorio, ordenados y entendidos.

Si has completado esta semana, ya no solo entiendes la seguridad desde la defensa, sino también desde el ataque "controlado y ético".  
Esta visión dual es la base del enfoque "Purple Team", que será muy útil cuando empecemos a conectar todo con detección, SIEM y respuesta ante incidentes en semanas posteriores.

**Prompt IA:**  
"Actúa como evaluador final de la Semana 7. Crea un checklist ampliado con criterios observables (qué evidencias debe haber en el repo). Añade 5 preguntas de reflexión orientadas a Blue Team: qué logs revisarías, qué alertas crearías y qué hardening aplicarías para los servicios detectados."

---

# Semana 2 – Redes en Profundidad 🌐

## 0. Introducción a la semana

En la Semana 1 construiste la base conceptual de la ciberseguridad: CIA, amenazas, vulnerabilidades, riesgo, Kill Chain, MITRE y roles.

En esta Semana 2 vamos a entrar en uno de los pilares técnicos más importantes para cualquier profesional de ciberseguridad: **las redes**.

Las redes son el “sistema circulatorio” de la tecnología moderna: por ellas viajan autenticaciones, accesos a APIs, navegación web, conexiones a servicios cloud, actualizaciones, telemetría de EDR/XDR, sincronización de correo, backups, etc. Por eso, en ciberseguridad, la red es a la vez:

- Un **vector de ataque** (por donde entra o se mueve el atacante).
- Una **fuente de evidencia** (lo que te permite investigar qué ocurrió).
- Un **punto de control** (donde puedes bloquear, filtrar o detectar).

Aunque trabajes en SOC, pentesting, ingeniería de seguridad o cloud, siempre vas a necesitar entender:

- Cómo se comunican los sistemas entre sí.
- Qué significa que un puerto esté abierto o cerrado.
- Qué es tráfico “normal” y qué podría ser sospechoso.
- Cómo “ver” lo que pasa por la red con herramientas como **Wireshark**.
- Cómo descubrir servicios con **nmap**.

No necesitas ser un ingeniero de redes experto, pero sí tener un mapa mental claro de cómo viajan los datos, qué protocolos se usan y dónde puede atacarse o defenderse. El objetivo es que dejes de ver la red como “algo abstracto” y empieces a verla como conversaciones observables, medibles y analizables.

**Prompt IA:**  
Actúa como mentor de ciberseguridad (nivel principiante) y explícame por qué entender redes es imprescindible para SOC, pentesting y cloud security. Usa ejemplos cotidianos (abrir una web, ver un vídeo, iniciar sesión en un servicio) y tradúcelos a conceptos de red (DNS, IP, puertos, TCP/UDP, TLS). Termina con una lista de 10 “señales de red” típicas en ataques reales y qué podrían significar.

---

## 1. Objetivos de aprendizaje de la Semana 2

Al finalizar esta semana deberías ser capaz de:

1. Explicar la diferencia básica entre los modelos **OSI** y **TCP/IP** y para qué sirven.
2. Describir qué hacen las capas más importantes desde el punto de vista de seguridad.
3. Entender conceptos básicos como: **IP, puerto, protocolo, DNS, HTTP/HTTPS, TLS**.
4. Distinguir entre dispositivos clave de red: **router, switch, firewall, proxy, WAF, IDS, IPS**.
5. Realizar un **escaneo básico con nmap** en tu laboratorio.
6. Capturar y analizar tráfico simple con **Wireshark** o **tcpdump**.
7. Identificar ejemplos de tráfico normal vs tráfico potencialmente sospechoso.
8. Documentar tus observaciones de red en tu repositorio.

Estos objetivos no están pensados para memorizar definiciones, sino para que puedas **razonar** con lo que ves. En un entorno real, muchas investigaciones se desbloquean cuando puedes responder preguntas como:

- “¿Esto ocurre en capa de red, transporte o aplicación?”
- “¿Es normal que este host hable con esa IP/dominio?”
- “¿Este puerto abierto es esperado en este servidor?”
- “¿Este patrón de conexiones se parece a un escaneo, a fuerza bruta o a C2?”

Si al final de la semana puedes explicar con tus palabras lo que has hecho en el laboratorio y por qué importa, entonces has cumplido el objetivo.

**Prompt IA:**  
Actúa como evaluador del aprendizaje de la Semana 2. Genera 12 preguntas de repaso (mezclando teoría y práctica) basadas en estos 8 objetivos. Para cada pregunta, incluye: respuesta modelo breve, explicación del razonamiento, y un “error común” que suele cometer un principiante.

---

## 2. Repaso rápido: ¿por qué las redes son tan importantes en ciberseguridad?

Casi todos los ataques, tarde o temprano, implican comunicación de red:

- Un phishing que lleva a una web maliciosa.
- Un malware que se conecta a un servidor de comando y control (C2).
- Un escaneo de puertos buscando servicios vulnerables.
- Un atacante moviéndose lateralmente entre máquinas.

La red está presente en fases muy distintas de un ataque:

- **Acceso inicial:** enlaces maliciosos, payloads descargados, conexiones a servicios expuestos.
- **Persistencia y control:** comunicaciones periódicas con infraestructura del atacante.
- **Movimiento lateral:** autenticaciones y accesos internos entre sistemas.
- **Impacto:** exfiltración de datos, cifrado coordinado, propagación a otros equipos.

Si entiendes la red:

- Puedes ver indicios tempranos de ataque (antes de que el incidente sea “obvio”).
- Comprendes mejor qué está pasando cuando ves una alerta (menos “ruido”, más contexto).
- Sabes dónde colocar controles (firewall, WAF, IDS/IPS, proxy, segmentación).
- Puedes diseñar laboratorios y escenarios de ataque/defensa más realistas.

Además, entender redes te ayuda a comunicarte mejor: en un SOC, muchas decisiones se basan en evidencias de red (“hubo conexiones a X”, “se observó un patrón Y”, “se intentaron N puertos”, etc.).

**Prompt IA:**  
Actúa como analista SOC. Dame 6 escenarios reales donde el tráfico de red es la clave para detectar o confirmar un incidente (phishing, C2, fuerza bruta, escaneo, movimiento lateral, exfiltración). Para cada escenario: qué patrón lo delata, qué logs lo evidencian (DNS/proxy/firewall/IDS), y qué preguntas debo hacer para diferenciar falso positivo vs actividad maliciosa.

---

## 3. Modelos OSI y TCP/IP: cómo se organizan las comunicaciones

### 3.1. ¿Qué es un modelo de referencia?

Los modelos **OSI** y **TCP/IP** son formas de **organizar mentalmente** cómo viaja la información desde una aplicación en un equipo hasta la aplicación en otro equipo, a través de una red.

No son leyes físicas, sino modelos conceptuales que nos ayudan a:

- Estructurar protocolos.
- Entender dónde ocurre un determinado problema.
- Saber qué controles de seguridad aplicar en cada “capa”.

En ciberseguridad, pensar por capas te ayuda a acotar problemas y también a aplicar controles adecuados. Por ejemplo:

- Si el problema es de **capa 3 (IP)**, no tiene sentido mirar primero la aplicación web.
- Si el problema es de **capa 7**, quizá el firewall de red no sea suficiente y necesites un WAF o revisión de lógica de la aplicación.

Otra ventaja práctica: cuando redactas documentación o informes, hablar de capas hace tus explicaciones más claras (“El fallo ocurre tras la resolución DNS, pero antes de completar el handshake TCP”, etc.).

### 3.2. Modelo OSI (7 capas)

El modelo OSI tiene 7 capas:

1. **Física** – Señales eléctricas, cables, radio (Wi-Fi), fibra.
2. **Enlace de datos** – Dirección MAC, switches, tramas.
3. **Red** – Direcciones IP, routing, routers.
4. **Transporte** – TCP, UDP, puertos, control de flujo.
5. **Sesión** – Gestión de sesiones (inicio, mantenimiento, cierre).
6. **Presentación** – Formato de datos, cifrado, compresión.
7. **Aplicación** – Protocolos como HTTP, DNS, SMTP, etc.

Aunque pueda parecer muy teórico, es útil para pensar:

- ¿El problema está en que no hay conectividad IP? (Capa 3)
- ¿Es un puerto que no responde? (Capa 4)
- ¿Es la aplicación web en sí misma? (Capa 7)

Ejemplos típicos para asentar la idea:

- Si hay conectividad IP pero no conectas por SSH: suele ser capa 4 (puerto 22 cerrado/bloqueado) o capa 7 (servicio SSH caído).
- Si el puerto abre pero falla el acceso por credenciales: capa 7 (autenticación) aunque la red funcione perfecto.
- Si todo parece “bien” pero hay lentitud extrema: puede ser capa 1/2 (problemas físicos o de enlace), capa 3 (rutas) o incluso saturación.

### 3.3. Modelo TCP/IP (4 capas)

Más cercano al uso real en Internet:

1. **Acceso a la red** – Equivalente a física + enlace.
2. **Internet** – Protocolos IP, routing.
3. **Transporte** – TCP, UDP, puertos.
4. **Aplicación** – HTTP, DNS, SSH, etc.

En seguridad, a menudo hablamos directamente de:

- Capa **Red (IP)**.
- Capa **Transporte (TCP/UDP)**.
- Capa **Aplicación (HTTP, DNS, etc.)**.

TCP/IP es el modelo que más “verás” en herramientas reales: capturas, logs de firewall, telemetría de EDR, etc. Por eso, dominar estas capas te da un salto importante en comprensión.

**Prompt IA:**  
Explícame OSI y TCP/IP con un ejemplo completo: “abrir una web HTTPS”. Describe qué pasa en cada capa (DNS, TCP, TLS, HTTP), qué fallos típicos pueden aparecer, y cómo los ubicarías por capas para diagnosticar más rápido.

---

## 4. Conceptos fundamentales: IP, puertos, protocolos

### 4.1. Dirección IP

Una dirección IP identifica un dispositivo en una red.

Ejemplo (IPv4):

- `192.168.56.10` (típica IP de red interna en host-only).
- `10.0.0.15`
- `172.16.0.5`

Cuando haces ping a otra máquina, estás probando conectividad a su IP (en capa 3).

Puntos clave para entender bien IP:

- **IP privada vs IP pública:** en laboratorio y redes internas trabajas con IP privadas. Para Internet se usan IP públicas.
- **NAT:** muchas veces “sales” a Internet con una IP pública común para varios equipos.
- **Más de una interfaz:** un host puede tener varias IP si está en varias redes (por ejemplo Host-Only + NAT).
- **La IP no siempre identifica al usuario real:** proxies, gateways y balanceadores pueden hacer que la IP que ves sea “intermedia”.

Como analista, siempre pregunta: “¿Esta IP es de un endpoint real o de un componente intermedio?”

### 4.2. Puertos

Un puerto es como una “puerta lógica” donde una aplicación escucha conexiones.

- Puerto 22 → SSH
- Puerto 80 → HTTP
- Puerto 443 → HTTPS
- Puerto 3389 → RDP

Un servicio puede estar escuchando en un puerto concreto, y un atacante puede:

- Escanear puertos para ver qué servicios están disponibles.
- Aprovecharse de servicios desactualizados o mal configurados.

Conceptos útiles alrededor de puertos:

- **Puertos “bien conocidos”** (como 80/443) y **puertos altos** (ephemeral), que suelen usarse como origen en conexiones salientes.
- Un puerto “abierto” no es automáticamente “vulnerable”, pero sí implica **exposición**: hay algo escuchando.
- La seguridad real depende de: versión, configuración, autenticación, segmentación y controles alrededor.

### 4.3. Protocolos

Un **protocolo** es un conjunto de reglas que define cómo se comunican dos sistemas.

Ejemplos:

- HTTP: transferencia de páginas web.
- HTTPS: HTTP cifrado sobre TLS.
- DNS: resolución de nombres (dominio → dirección IP).
- SSH: acceso remoto cifrado a sistemas.

Cada protocolo tiene su propia estructura de mensajes y también sus **posibles vulnerabilidades**.

Aprender protocolos no es solo teoría: te permite reconocer anomalías. Por ejemplo:

- DNS con consultas raras o muy frecuentes.
- HTTP con patrones típicos de explotación (parámetros extraños, rutas comunes de ataque).
- SSH con muchos fallos seguidos (fuerza bruta o cred stuffing en accesos internos).
- Conexiones repetitivas a destinos desconocidos (posible C2).

**Prompt IA:**  
Explica IP, puertos y protocolos como si enseñaras a un junior de SOC. Quiero definiciones claras y ejemplos prácticos, pero también detalle útil: diferencia entre TCP y UDP, qué es un socket, y cómo interpretar “puerto abierto” desde el punto de vista de exposición y riesgo. Incluye 10 señales sospechosas relacionadas con IP/puertos/protocolos.

---

## 5. DNS, HTTP/HTTPS y TLS: lo que pasa en casi todos los ataques

### 5.1. DNS (Domain Name System)

DNS es como la “agenda telefónica” de Internet.  
Traduce nombres como `www.ejemplo.com` a direcciones IP.

En seguridad:

- DNS puede usarse para filtrar dominios maliciosos.
- Algunos malware usan DNS para comunicarse de forma encubierta.
- Un fallo de DNS puede dejar inserviciosos muchos servicios.

DNS es especialmente valioso porque es una capa previa a muchas comunicaciones: aunque el tráfico final vaya cifrado, normalmente hay resolución de dominio antes. Por eso, muchos controles y detecciones se apoyan en DNS.

Señales de interés en DNS (a nivel conceptual):

- Picos de consultas.
- Dominios recién registrados o con reputación baja.
- Subdominios muy largos o aleatorios (a veces asociados a túneles o DGA).
- Consultas repetitivas a intervalos constantes.

### 5.2. HTTP

HTTP es el protocolo básico de la web.

Características:

- Texto plano.
- Fácil de inspeccionar.
- Fácil de manipular.

Un atacante puede:

- Interceptar tráfico HTTP sin cifrado.
- Modificar respuestas.
- Robar credenciales si viajan en claro.

Aunque hoy es común usar HTTPS, todavía existen escenarios donde HTTP aparece: servicios internos, entornos legacy, redirecciones mal implementadas, o aplicaciones que mezclan contenido.

Como defensores, HTTP es fácil de analizar, pero precisamente por eso es inseguro para datos sensibles.

### 5.3. HTTPS y TLS

HTTPS = HTTP + TLS (capa de cifrado).

- Proporciona confidencialidad: el contenido viaja cifrado.
- También autenticidad: mediante certificados digitales.

En seguridad:

- Es bueno que haya HTTPS, pero también complica la inspección de tráfico.
- Muchas soluciones de seguridad implementan mecanismos para inspeccionar tráfico cifrado (TLS inspection), con sus riesgos y complejidades.

Importante: aunque no puedas ver el contenido, sí puedes analizar metadatos:

- IP y puerto destino.
- Frecuencia y duración de conexiones.
- Tamaño aproximado de transferencias.
- Certificados (validez, emisor, rarezas).
- Dominios asociados (según visibilidad disponible).

**Prompt IA:**  
Actúa como analista SOC y explícale a un principiante cómo se usan DNS, HTTP y TLS en ataques reales. Para cada uno: qué señales se ven en logs, qué patrones pueden indicar phishing/C2/exfiltración, qué limitaciones introduce TLS, y cómo investigar igualmente usando metadatos (dominios, certificados, frecuencias, tamaños, reputación).

---

## 6. Dispositivos y componentes de red importantes

### 6.1. Router

- Dispositivo que conecta redes diferentes.
- Toma decisiones de encaminamiento (routing) basadas en direcciones IP.
- En casa, el router de tu ISP conecta tu red doméstica con Internet.

En empresa, “router” suele ser parte de una arquitectura con segmentación: subredes para usuarios, servidores, DMZ, administración, etc. Comprender routing te ayuda a entender por qué algo “no llega” o por qué un atacante podría moverse de un segmento a otro si la segmentación es débil.

### 6.2. Switch

- Dispositivo que conecta equipos dentro de una misma red local.
- Trabaja a nivel de MAC (enlace de datos).
- Reenvía tramas solo al puerto donde está el destinatario (no a todos como un hub).

Los switches permiten aplicar segmentación mediante VLANs. Eso reduce superficie de ataque y complica el movimiento lateral. En seguridad, la segmentación es un control fundamental: no todo debe hablar con todo.

### 6.3. Firewall

- Dispositivo o software que **filtra tráfico** según reglas.
- Puede estar en:
  - Perímetro (entre red interna e Internet).
  - Sistemas finales (firewall de Windows).
- Se define por reglas del tipo:
  - Permitir IP origen X a puerto 443 destino Y.
  - Bloquear todo tráfico entrante salvo X e Y.

Un firewall es un control de reducción de exposición. Bien aplicado, hace que muchos ataques no sean “posibles” directamente porque el tráfico ni siquiera llega.

También es una fuente de evidencia: logs de tráfico permitido/bloqueado, patrones de escaneo, conexiones anómalas, etc.

### 6.4. Proxy

- Servidor intermediario entre los clientes internos e Internet.
- Puede usarse para:
  - Filtrar contenido (web filtering).
  - Loguear peticiones (quién accede a qué).
  - Mejorar privacidad o rendimiento (caché).

El proxy es clave cuando quieres visibilidad y control de navegación: quién accede, a qué dominio, con qué URL, cuándo, y qué respuesta recibe.

### 6.5. WAF (Web Application Firewall)

- Firewall específico para aplicaciones web.
- No solo mira IPs y puertos, sino el contenido del tráfico HTTP/HTTPS.
- Capaz de detectar:
  - Inyección SQL.
  - XSS.
  - Patrones de ataque a formularios.

El WAF es útil cuando una aplicación web está expuesta y quieres mitigar ataques comunes, bots o abuso. No sustituye el desarrollo seguro, pero ayuda como capa adicional.

### 6.6. IDS / IPS

- **IDS (Intrusion Detection System):** sistema que detecta patrones de ataque, pero no bloquea por sí mismo (generar alertas).
- **IPS (Intrusion Prevention System):** similar al IDS, pero con capacidad de bloquear o modificar tráfico.

En un SOC real, estos dispositivos son fuentes de alertas que luego se investigan.

Importante: una alerta IDS/IPS no siempre significa “compromiso confirmado”. Significa “se observó un patrón que se parece a algo peligroso”. El trabajo del analista es validar contexto: origen, destino, volumen, si fue bloqueado, si hay evidencia en endpoints, etc.

**Prompt IA:**  
Explica router, switch, firewall, proxy, WAF e IDS/IPS con enfoque SOC: qué función cumple cada uno, qué tipo de logs genera, y qué incidentes ayuda a detectar o contener. Incluye un ejemplo de arquitectura simple (usuarios, servidores, DMZ) y cómo fluye el tráfico por esos componentes.

---

## 7. Herramientas clave: nmap, Wireshark, tcpdump, Burp Suite

### 7.1. nmap

**nmap** es una herramienta de escaneo de red y puertos.

Permite:

- Descubrir qué máquinas están activas.
- Ver qué puertos están abiertos.
- Identificar servicios y versiones.
- A veces, detectar sistema operativo.

Ejemplos de uso básicos (en Kali):

    nmap 192.168.56.0/24      # Escaneo de toda la red interna
    nmap -sV 192.168.56.101   # Detectar servicios y versiones en una máquina

A nivel de aprendizaje, nmap te enseña a pensar como atacante de manera controlada: “¿qué veo desde fuera?”. Y también te enseña a pensar como defensor: “¿qué debería exponer y qué no?”.

Para sacarle partido, interpreta cada resultado con mentalidad de “superficie de ataque”:

- **Host detectado:** confirma que es accesible desde tu punto de vista.
- **Puerto abierto:** existe un servicio escuchando, por tanto hay exposición.
- **Servicio/versión:** te orienta sobre riesgo potencial (software antiguo, configuración débil).
- **Puertos inesperados:** son los que más interesan, porque suelen revelar fallos de hardening o necesidades reales mal documentadas.

Una práctica útil para un principiante es documentar por cada host:

- Puertos esperados (por diseño).
- Puertos no esperados (posible riesgo).
- Qué comprobarías después (servicio, necesidad, restricción por firewall/segmentación).

### 7.2. Wireshark

Herramienta gráfica para capturar y analizar tráfico de red.

Permite:

- Ver paquetes individuales.
- Filtrar por IP, protocolo, puerto.
- Analizar protocolos como HTTP, DNS, TLS.

Es una de las herramientas más importantes para aprender a “ver” qué ocurre en la red.

Wireshark convierte teoría en realidad: de pronto ves que un ping no es “magia”, sino paquetes ICMP con origen y destino. Al principio, céntrate en:

- Capturar en la **interfaz correcta**.
- Aprender filtros básicos (por ejemplo: `icmp`, `dns`, `tcp`, `ip.addr == X`).
- Reconocer el concepto de “conversación” (request y response).

Tu objetivo esta semana no es dominar Wireshark, sino perderle el miedo y saber interpretar lo esencial.

### 7.3. tcpdump

Versión en línea de comandos de captura de tráfico.

Ejemplo:

    sudo tcpdump -i eth0

Se usa mucho en servidores donde no hay entorno gráfico.

tcpdump es perfecto cuando trabajas por terminal, o cuando necesitas verificar tráfico de forma rápida. Aporta mucha utilidad en troubleshooting y en investigación básica:

- Ver si “llega” tráfico a una interfaz.
- Ver si hay respuestas.
- Ver patrones repetitivos (intentos constantes, floods, etc.).

Más adelante, cuando tengas más experiencia, podrás usarlo como herramienta rápida para validar hipótesis antes de profundizar con análisis más pesado.

### 7.4. Burp Suite (Community Edition)

Proxy de interceptación para tráfico web.

Permite:

- Interceptar peticiones HTTP/HTTPS entre navegador y servidor.
- Modificar parámetros antes de enviarlos.
- Analizar vulnerabilidades en aplicaciones web.

En este curso lo usaremos más en la parte ofensiva, pero ya conviene conocerlo.

Burp te ayuda a comprender la “anatomía” real de una petición web:

- Cabeceras (headers) y su impacto (cookies, tokens, user-agent).
- Parámetros en URL vs cuerpo de petición.
- Códigos de estado (200, 302, 401, 403, 500…).
- Respuestas del servidor y qué revelan (errores, redirecciones, validaciones).

Incluso en un rol SOC, entender esto mejora tu capacidad de analizar alertas web y ataques a aplicaciones.

**Prompt IA:**  
Actúa como instructor práctico. Explica cuándo usar nmap, Wireshark, tcpdump y Burp Suite (objetivo, casos de uso típicos, qué evidencia generan, errores comunes). Después, crea un ejercicio integrador de laboratorio (solo en entorno controlado) donde use las 4 herramientas, indicando qué debería observar y cómo documentarlo.

---

## 8. Laboratorio de la Semana 2

Ahora vamos a aplicar todo esto en tu laboratorio.

El objetivo del laboratorio es aprender a “mirar” la red: comprobar conectividad, observar tráfico real y entender qué servicios están expuestos. Pero, sobre todo, entrenar el hábito de documentar lo que haces como si fuese trabajo real de un analista.

**Prompt IA:**  
Actúa como instructor de laboratorio. Guíame paso a paso en estos ejercicios de red y ayúdame a documentarlos en GitHub. Para cada ejercicio: qué debería ver si todo está correcto, qué problemas típicos pueden surgir (por ejemplo, firewall bloqueando ping o interfaz equivocada), cómo solucionarlos, y qué evidencias conviene guardar (salida de comandos, notas, capturas).

### 8.1. Ejercicio 1: Comprobando conectividad entre máquinas

Desde **Kali**, abre una terminal y:

1. Averigua tu IP:

    ip addr

   Además de la IP, fíjate en:
   - Interfaz que estás usando (te servirá en Wireshark/tcpdump).
   - Si la interfaz está activa (`UP`).
   - Si la IP pertenece a tu red Host-Only o a otra red.

2. Haz ping a la IP de tu máquina **Windows**:

    ping -c 4 IP_DE_WINDOWS

3. Haz ping a la IP de tu **Ubuntu Server**:

    ping -c 4 IP_DE_UBUNTU

Reflexiona:

- Si no hay respuesta, ¿es un problema de capa de red (IP) o de otro tipo?
- ¿Tienes bien configurado el adaptador Host-Only?
- ¿Puede haber un firewall local bloqueando ICMP (muy común)?
- ¿Has verificado que estás usando la IP correcta (y no la de otra interfaz)?

Documenta los resultados en un fichero:

`Semana2_Conectividad.md`

---

### 8.2. Ejercicio 2: Descubriendo servicios con nmap

Desde **Kali**:

1. Escanea la red Host-Only completa (ajusta la subred a la tuya real):

    nmap 192.168.56.0/24

2. Identifica:
   - Qué IP corresponde a cada máquina (Windows, Ubuntu, etc.).
   - Qué puertos abiertos detecta nmap en cada una.

3. Haz un escaneo un poco más profundo de Ubuntu:

    nmap -sV IP_DE_UBUNTU

4. Anota:
   - Servicios identificados.
   - Versiones (si las detecta).
   - Qué puertos están abiertos que esperabas (por ejemplo, 22 para SSH) y cuáles no.

Guarda tus observaciones en:

`Semana2_nmap_Resultados.md`

Este ejercicio comienza a introducir la mentalidad ofensiva: ver qué superficie de ataque expone cada máquina.

---

### 8.3. Ejercicio 3: Capturando tráfico con Wireshark o tcpdump

En **Kali** o **Ubuntu**, elige uno de los dos enfoques:

#### Opción A – Wireshark (modo gráfico)

1. Abre Wireshark.
2. Selecciona la interfaz que corresponde a la red Host-Only (por ejemplo, `eth0` o similar).
3. Inicia la captura.
4. Desde otra máquina (por ejemplo Windows), haz ping a Ubuntu.
5. Observa en Wireshark los paquetes ICMP (ping).
6. Filtra por protocolo `icmp`.

Reflexiona:

- ¿Qué información ves en cada paquete?
- ¿Puedes identificar IP origen y destino?
- ¿Ves la secuencia request/respuesta?
- ¿Qué diferencias observas si el ping falla (si puedes provocarlo)?

#### Opción B – tcpdump (modo terminal)

En Ubuntu:

    sudo tcpdump -i INTERFAZ icmp

Mientras tanto, desde otra máquina, haz ping a Ubuntu.

Verás cómo se registran las peticiones y respuestas ICMP en tiempo real.

Documenta lo que has visto en:

`Semana2_Trafico_Basico.md`

---

### 8.4. Ejercicio 4: Tráfico normal vs sospechoso (reflexivo)

Sin entrar todavía en ataques avanzados, piensa:

1. ¿Qué consideras tráfico normal en tu laboratorio?
   - Pings entre máquinas.
   - SSH de Kali a Ubuntu.
   - Navegación web desde una VM hacia Internet (si usas NAT).

2. ¿Qué podría empezar a parecer sospechoso?
   - Escaneos intensivos de nmap desde una máquina a todas las demás.
   - Una máquina que intenta conectar a muchos puertos de otra.
   - Tráfico frecuente a puertos poco habituales (por ejemplo, muchos intentos al puerto 23/Telnet).

Puedes añadir criterios típicos de SOC para justificar sospecha:

- Frecuencia anormal (picos repentinos).
- Horarios raros (actividad fuera de jornada).
- Destinos no esperados.
- Reintentos masivos y persistentes.
- Conexiones periódicas (patrones de “beaconing”).

Escribe estos ejemplos en:

`Semana2_Trafico_Normal_vs_Sospechoso.md`

Este tipo de reflexión es justamente lo que hace un analista SOC al mirar un dashboard de red.

---

## 9. Entregables de la Semana 2

Al terminar la semana deberías haber creado o actualizado:

1. `Semana2_Conectividad.md`  
   - Resultados de ping entre máquinas.
   - Problemas encontrados y cómo los has resuelto.

   Recomendación: añade contexto de red (Host-Only/NAT), IPs de cada VM y cualquier detalle que permita reproducir tu setup.

2. `Semana2_nmap_Resultados.md`  
   - Resultados de escaneos nmap.
   - Servicios y puertos detectados.
   - Primeras conclusiones sobre superficie de ataque.

   Recomendación: incluye una mini-conclusión por host indicando qué puertos son esperados y cuáles revisarías si esto fuese un entorno real.

3. `Semana2_Trafico_Basico.md`  
   - Observaciones de capturas con Wireshark o tcpdump.
   - Descripción de lo que has visto.

   Recomendación: explica qué filtro usaste, qué buscabas y qué aprendiste de la captura.

4. `Semana2_Trafico_Normal_vs_Sospechoso.md`  
   - Tu visión de tráfico esperable vs potencialmente sospechoso en el laboratorio.

   Recomendación: no solo pongas ejemplos, explica por qué consideras cada uno normal o sospechoso.

Además, puedes ampliar tu `Lab_Setup_Report.md` con:

- Un pequeño diagrama de red con IPs.
- Notas sobre las interfaces de red de cada VM.

**Prompt IA:**  
Actúa como revisor de repositorios técnicos. Diseña una plantilla “perfecta” para cada entregable de la Semana 2, con secciones sugeridas, ejemplos de redacción y criterios de calidad. Incluye qué evidencias aportan más valor (salidas, capturas, tablas, conclusiones y próximos pasos).

---

## 10. Checklist de cierre de la Semana 2

Marca como completado cuando:

- [ ] Eres capaz de explicar, de forma sencilla, la diferencia entre OSI y TCP/IP.
- [ ] Puedes describir qué son IP, puerto y protocolo con ejemplos concretos.
- [ ] Entiendes qué hacen un router, un switch y un firewall en líneas generales.
- [ ] Sabes qué es un WAF y qué diferencia hay con un firewall tradicional.
- [ ] Has hecho al menos un escaneo básico con nmap en tu laboratorio.
- [ ] Has capturado tráfico de red (ping, por ejemplo) con Wireshark o tcpdump.
- [ ] Puedes dar ejemplos de tráfico normal y tráfico sospechoso en tu entorno de laboratorio.
- [ ] Has generado los ficheros de la Semana 2 y los has subido a tu repositorio.

Si has completado estos puntos, has dado un paso clave:  
ya no ves la red como “algo abstracto” sino como un conjunto de comunicaciones que puedes **observar, medir y analizar**.

En la **Semana 3** utilizaremos esta base de redes para profundizar en **sistemas operativos (Linux y Windows) orientados a seguridad**, que serán tus piezas básicas tanto para ofensiva como para defensiva.

**Prompt IA:**  
Actúa como evaluador final. Diseña un mini-examen de cierre de Semana 2 con: (1) tareas prácticas (conectividad, escaneo, captura), (2) resultados esperados, (3) 10 preguntas de interpretación, y (4) criterios para decidir si el alumno está listo para pasar a la Semana 3 o necesita repasar.

---

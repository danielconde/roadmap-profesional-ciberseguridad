# Semana 2 – Redes en Profundidad 🌐

## 0. Introducción a la semana

En la Semana 1 construiste la base conceptual de la ciberseguridad: CIA, amenazas, vulnerabilidades, riesgo, Kill Chain, MITRE y roles.

En esta Semana 2 vamos a entrar en uno de los pilares técnicos más importantes para cualquier profesional de ciberseguridad: **las redes**.

Aunque trabajes en SOC, pentesting, ingeniería de seguridad o cloud, siempre vas a necesitar entender:

- Cómo se comunican los sistemas entre sí.
- Qué significa que un puerto esté abierto o cerrado.
- Qué es tráfico “normal” y qué podría ser sospechoso.
- Cómo “ver” lo que pasa por la red con herramientas como **Wireshark**.
- Cómo descubrir servicios con **nmap**.

No necesitas ser un ingeniero de redes experto, pero sí tener un mapa mental claro de cómo viajan los datos, qué protocolos se usan y dónde puede atacarse o defenderse.

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

---

## 2. Repaso rápido: ¿por qué las redes son tan importantes en ciberseguridad?

Casi todos los ataques, tarde o temprano, implican comunicación de red:

- Un phishing que lleva a una web maliciosa.
- Un malware que se conecta a un servidor de comando y control (C2).
- Un escaneo de puertos buscando servicios vulnerables.
- Un atacante moviéndose lateralmente entre máquinas.

Si entiendes la red:

- Puedes ver indicios tempranos de ataque.
- Comprendes mejor qué está pasando cuando ves una alerta.
- Sabes dónde colocar controles (firewalls, WAF, IDS/IPS).
- Puedes diseñar laboratorios y escenarios de ataque/defensa más realistas.

---

## 3. Modelos OSI y TCP/IP: cómo se organizan las comunicaciones

### 3.1. ¿Qué es un modelo de referencia?

Los modelos **OSI** y **TCP/IP** son formas de **organizar mentalmente** cómo viaja la información desde una aplicación en un equipo hasta la aplicación en otro equipo, a través de una red.

No son leyes físicas, sino modelos conceptuales que nos ayudan a:

- Estructurar protocolos.
- Entender dónde ocurre un determinado problema.
- Saber qué controles de seguridad aplicar en cada “capa”.

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

---

## 4. Conceptos fundamentales: IP, puertos, protocolos

### 4.1. Dirección IP

Una dirección IP identifica un dispositivo en una red.

Ejemplo (IPv4):

- `192.168.56.10` (típica IP de red interna en host-only).
- `10.0.0.15`
- `172.16.0.5`

Cuando haces ping a otra máquina, estás probando conectividad a su IP (en capa 3).

### 4.2. Puertos

Un puerto es como una “puerta lógica” donde una aplicación escucha conexiones.

- Puerto 22 → SSH
- Puerto 80 → HTTP
- Puerto 443 → HTTPS
- Puerto 3389 → RDP

Un servicio puede estar escuchando en un puerto concreto, y un atacante puede:

- Escanear puertos para ver qué servicios están disponibles.
- Aprovecharse de servicios desactualizados o mal configurados.

### 4.3. Protocolos

Un **protocolo** es un conjunto de reglas que define cómo se comunican dos sistemas.

Ejemplos:

- HTTP: transferencia de páginas web.
- HTTPS: HTTP cifrado sobre TLS.
- DNS: resolución de nombres (dominio → dirección IP).
- SSH: acceso remoto cifrado a sistemas.

Cada protocolo tiene su propia estructura de mensajes y también sus **posibles vulnerabilidades**.

---

## 5. DNS, HTTP/HTTPS y TLS: lo que pasa en casi todos los ataques

### 5.1. DNS (Domain Name System)

DNS es como la “agenda telefónica” de Internet.  
Traduce nombres como `www.ejemplo.com` a direcciones IP.

En seguridad:

- DNS puede usarse para filtrar dominios maliciosos.
- Algunos malware usan DNS para comunicarse de forma encubierta.
- Un fallo de DNS puede dejar inserviciosos muchos servicios.

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

### 5.3. HTTPS y TLS

HTTPS = HTTP + TLS (capa de cifrado).

- Proporciona confidencialidad: el contenido viaja cifrado.
- También autenticidad: mediante certificados digitales.

En seguridad:

- Es bueno que haya HTTPS, pero también complica la inspección de tráfico.
- Muchas soluciones de seguridad implementan mecanismos para inspeccionar tráfico cifrado (TLS inspection), con sus riesgos y complejidades.

---

## 6. Dispositivos y componentes de red importantes

### 6.1. Router

- Dispositivo que conecta redes diferentes.
- Toma decisiones de encaminamiento (routing) basadas en direcciones IP.
- En casa, el router de tu ISP conecta tu red doméstica con Internet.

### 6.2. Switch

- Dispositivo que conecta equipos dentro de una misma red local.
- Trabaja a nivel de MAC (enlace de datos).
- Reenvía tramas solo al puerto donde está el destinatario (no a todos como un hub).

### 6.3. Firewall

- Dispositivo o software que **filtra tráfico** según reglas.
- Puede estar en:
  - Perímetro (entre red interna e Internet).
  - Sistemas finales (firewall de Windows).
- Se define por reglas del tipo:
  - Permitir IP origen X a puerto 443 destino Y.
  - Bloquear todo tráfico entrante salvo X e Y.

### 6.4. Proxy

- Servidor intermediario entre los clientes internos e Internet.
- Puede usarse para:
  - Filtrar contenido (web filtering).
  - Loguear peticiones (quién accede a qué).
  - Mejorar privacidad o rendimiento (caché).

### 6.5. WAF (Web Application Firewall)

- Firewall específico para aplicaciones web.
- No solo mira IPs y puertos, sino el contenido del tráfico HTTP/HTTPS.
- Capaz de detectar:
  - Inyección SQL.
  - XSS.
  - Patrones de ataque a formularios.

### 6.6. IDS / IPS

- **IDS (Intrusion Detection System):** sistema que detecta patrones de ataque, pero no bloquea por sí mismo (generar alertas).
- **IPS (Intrusion Prevention System):** similar al IDS, pero con capacidad de bloquear o modificar tráfico.

En un SOC real, estos dispositivos son fuentes de alertas que luego se investigan.

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

```bash
nmap 192.168.56.0/24      # Escaneo de toda la red interna
nmap -sV 192.168.56.101   # Detectar servicios y versiones en una máquina
```

### 7.2. Wireshark

Herramienta gráfica para capturar y analizar tráfico de red.

Permite:

- Ver paquetes individuales.
- Filtrar por IP, protocolo, puerto.
- Analizar protocolos como HTTP, DNS, TLS.

Es una de las herramientas más importantes para aprender a “ver” qué ocurre en la red.

### 7.3. tcpdump

Versión en línea de comandos de captura de tráfico.

Ejemplo:

```bash
sudo tcpdump -i eth0
```

Se usa mucho en servidores donde no hay entorno gráfico.

### 7.4. Burp Suite (Community Edition)

Proxy de interceptación para tráfico web.

Permite:

- Interceptar peticiones HTTP/HTTPS entre navegador y servidor.
- Modificar parámetros antes de enviarlos.
- Analizar vulnerabilidades en aplicaciones web.

En este curso lo usaremos más en la parte ofensiva, pero ya conviene conocerlo.

---

## 8. Laboratorio de la Semana 2

Ahora vamos a aplicar todo esto en tu laboratorio.

### 8.1. Ejercicio 1: Comprobando conectividad entre máquinas

Desde **Kali**, abre una terminal y:

1. Averigua tu IP:

   ```bash
   ip addr
   ```

2. Haz ping a la IP de tu máquina **Windows**:

   ```bash
   ping -c 4 IP_DE_WINDOWS
   ```

3. Haz ping a la IP de tu **Ubuntu Server**:

   ```bash
   ping -c 4 IP_DE_UBUNTU
   ```

Reflexiona:

- Si no hay respuesta, ¿es un problema de capa de red (IP) o de otro tipo?
- ¿Tienes bien configurado el adaptador Host-Only?

Documenta los resultados en un fichero:

`Semana2_Conectividad.md`

---

### 8.2. Ejercicio 2: Descubriendo servicios con nmap

Desde **Kali**:

1. Escanea la red Host-Only completa (ajusta la subred a la tuya real):

   ```bash
   nmap 192.168.56.0/24
   ```

2. Identifica:
   - Qué IP corresponde a cada máquina (Windows, Ubuntu, etc.).
   - Qué puertos abiertos detecta nmap en cada una.

3. Haz un escaneo un poco más profundo de Ubuntu:

   ```bash
   nmap -sV IP_DE_UBUNTU
   ```

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

#### Opción B – tcpdump (modo terminal)

En Ubuntu:

```bash
sudo tcpdump -i INTERFAZ icmp
```

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

Escribe estos ejemplos en:

`Semana2_Trafico_Normal_vs_Sospechoso.md`

Este tipo de reflexión es justamente lo que hace un analista SOC al mirar un dashboard de red.

---

## 9. Entregables de la Semana 2

Al terminar la semana deberías haber creado o actualizado:

1. `Semana2_Conectividad.md`  
   - Resultados de ping entre máquinas.
   - Problemas encontrados y cómo los has resuelto.

2. `Semana2_nmap_Resultados.md`  
   - Resultados de escaneos nmap.
   - Servicios y puertos detectados.
   - Primeras conclusiones sobre superficie de ataque.

3. `Semana2_Trafico_Basico.md`  
   - Observaciones de capturas con Wireshark o tcpdump.
   - Descripción de lo que has visto.

4. `Semana2_Trafico_Normal_vs_Sospechoso.md`  
   - Tu visión de tráfico esperable vs potencialmente sospechoso en el laboratorio.

Además, puedes ampliar tu `Lab_Setup_Report.md` con:

- Un pequeño diagrama de red con IPs.
- Notas sobre las interfaces de red de cada VM.

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

---

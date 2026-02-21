# 00 – Entorno de Laboratorio Profesional 🧪

## 0. Introducción: por qué necesitas un laboratorio

La ciberseguridad no se domina leyendo definiciones, sino viendo cómo se comportan los sistemas cuando los atacas, los configuras mal o los monitorizas.

Un laboratorio es:

- Un conjunto de máquinas virtuales (VMs) que simulan un entorno de empresa en pequeño.
- Una red interna donde puedes lanzar ataques sin afectar a nadie.
- Un espacio donde puedes equivocarte, romper cosas y volver atrás con un clic.

En este módulo vas a construir:

- Una máquina atacante (Kali Linux).
- Una máquina víctima de usuario (Windows).
- Un servidor Linux (Ubuntu Server).
- Una red interna aislada.
- Un primer recurso en la nube (Azure / AWS / GCP).

Este será el escenario donde harás todos los ejercicios del curso. Si saltas este módulo, el resto tendrá mucha teoría y poca práctica real.

---

## 1. Requisitos técnicos: ¿qué necesita tu PC y por qué?

Antes de empezar a crear máquinas virtuales, es importante saber si tu equipo puede soportarlas.

### 1.1. Requisitos recomendados

- **Memoria RAM:** 16 GB recomendados (8 GB es el mínimo útil).
- **Almacenamiento:** al menos 100–150 GB libres en disco.
- **Procesador:** compatible con virtualización por hardware (Intel VT-x o AMD-V).
- **Disco:** mejor si es SSD, porque las VMs leen y escriben mucho.
- **Sistema operativo del host:** Windows 10/11, cualquier Linux moderno o macOS.

### 1.2. ¿Por qué esto es importante?

Cada máquina virtual se comporta como un ordenador completo:

- Reserva su propia RAM.
- Usa espacio de disco para el sistema y los datos.
- Consume CPU cuando está encendida.

Si, por ejemplo, tienes 8 GB de RAM y creas tres VMs asignando 4 GB a cada una, el sistema no será estable. Por eso, si tu máquina es más limitada, podrás seguir el curso, pero quizá debas tener solo dos VMs encendidas a la vez y ajustar las RAM.

La virtualización por hardware (Intel VT-x / AMD-V) permite que las VMs funcionen de forma más fluida y con instrucciones específicas de CPU pensadas para este uso. Si no está activada en la BIOS/UEFI, las máquinas irán lentas o incluso no arrancarán.

---

## 2. ¿Qué es un virtualizador y qué papel tiene?

Un virtualizador (hipervisor de tipo 2 en nuestro caso) es un programa que te permite crear “ordenadores dentro de tu ordenador”.

Ejemplos:

- **VirtualBox** (gratuito, muy usado en formación).
- **VMware Workstation Player** (gratuito para uso personal).
- **VMware Workstation Pro** (de pago, usado en entornos más profesionales).

En este curso usaremos **VirtualBox** como referencia porque:

- Es gratuito.
- Funciona en Windows, Linux y macOS.
- Hay mucha documentación y soporte en Internet.

Cuando instalas VirtualBox:

- Tu sistema real (el portátil o PC) se llama **host**.
- Las máquinas virtuales que creas dentro se llaman **guests** (invitadas).

Cada guest tiene su propio sistema operativo, su disco virtual y su configuración de red, pero en realidad todo se ejecuta sobre el hardware del host.

---

## 3. Diseño de red del laboratorio: pensar como una empresa pequeña

En una empresa real, las redes no son “todo conectado a todo”.  
Hay segmentación: zonas más internas, zonas expuestas, servidores, estaciones de trabajo, etc.

En nuestro laboratorio vamos a simular algo sencillo:

- Una **red interna aislada**, que represente la red interna de la empresa.
- Varias máquinas dentro de esa red:
  - Una máquina **Kali** que simula a un atacante interno, un pentester o un equipo de Red Team.
  - Una máquina **Windows**, que simula el ordenador de un empleado.
  - Una máquina **Ubuntu Server**, que simula un servidor interno.

Más adelante, añadiremos una máquina en la nube para entender la parte de Cloud.

### 3.1. Modos de red en VirtualBox

VirtualBox permite varios modos de conexión de red. Los más importantes para nosotros son:

#### NAT (Network Address Translation)

- La máquina virtual puede salir a Internet, usando la conexión del host.
- Desde fuera, nadie puede conectarse de forma directa a la VM.
- Es útil para que la VM se actualice, descargue herramientas, etc.

#### Host-Only (modo clave para este laboratorio)

- Crea una red privada entre:
  - el host (tu máquina real)
  - y las máquinas virtuales.
- Las VMs pueden hablar entre ellas y con el host, pero **no están directamente conectadas a tu red doméstica ni a Internet**.
- Es el modo más seguro para hacer pruebas ofensivas, porque lo que hagas se queda en esa red interna.

#### Bridge (NO recomendado para este curso)

- La máquina virtual se conecta a la red real como si fuera otro dispositivo más (como tu móvil o tu portátil).
- Si lanzas escaneos de red, podrías escanear equipos reales de tu casa, oficina o vecinos.
- Tiene riesgos legales y éticos si no se controla bien.

Por este motivo, **no utilizaremos modo Bridge** en ejercicios ofensivos.

---

## 4. Máquina atacante: Kali Linux

**Kali Linux** es una distribución basada en Debian, diseñada específicamente para pruebas de seguridad. No es “un Linux normal con cuatro cosas”, sino un entorno preparado con:

- Escáneres de puertos y servicios (nmap).
- Marco de explotación (Metasploit).
- Proxies de interceptación web (Burp Suite Community).
- Herramientas de fuerza bruta.
- Herramientas para enumeración de redes y sistemas.

### 4.1. ¿Por qué usar Kali y no cualquier Linux?

Podrías instalar las mismas herramientas en un Ubuntu normal, pero Kali te da:

- Un sistema ya pensado para hacking ético.
- Una estructura de menús orientada a categorías de ataque.
- Soporte para muchas herramientas “de libro de pentesting” sin tener que instalarlas una a una.

### 4.2. Configuración recomendada de Kali

A la hora de crear la VM en VirtualBox:

- CPU: 2 núcleos (si tu host lo permite).
- RAM: 4 GB (puede ir con 2 GB, pero limitado).
- Disco: 40 GB (dinámico, crecerá según uso).
- Red:
  - Adaptador 1 → **Host-Only** (red interna del laboratorio).
  - Opcionalmente, un segundo adaptador NAT para actualizaciones, que se puede activar solo cuando lo necesites.

Después de instalar Kali, es recomendable:

- Actualizar el sistema (`sudo apt update && sudo apt upgrade`).
- Confirmar que puedes hacer ping a las otras VMs del laboratorio (cuando existan).
- Anotar la IP de Kali dentro de la red Host-Only.

---

## 5. Máquina víctima de usuario: Windows 10/11

En los entornos empresariales, la mayoría de usuarios trabajan con **Windows**. Por eso, si quieres entender ataques reales, es fundamental ver:

- Cómo se comporta una máquina Windows ante un ataque.
- Qué eventos se generan.
- Cómo se ve el sistema cuando hay persistencia.

### 5.1. Rol de esta máquina en el laboratorio

Esta máquina representará:

- El PC de un empleado normal.
- Un equipo con el que el atacante intenta interactuar.
- Un origen de logs de eventos de seguridad.

En ejercicios posteriores, la usaremos para:

- Lanzar procesos sospechosos.
- Simular malware.
- Analizar eventos en el Visor de Eventos.
- Ver qué hace Windows cuando se ejecutan ciertas acciones.

### 5.2. Configuración recomendada de la VM Windows

- CPU: 2 núcleos.
- RAM: 4 GB mínimo para que Windows vaya razonablemente fluido.
- Disco: 50 GB.
- Red:
  - Host-Only (para que pueda comunicarse con Kali y Ubuntu, pero no con tu red real).

Tras instalar Windows:

- Crea al menos un **usuario estándar** (no administrador) para simular el uso real.
- Familiarízate con:
  - **Administrador de tareas (Task Manager)** para ver procesos.
  - **Visor de eventos (Event Viewer)** para revisar logs de seguridad.
  - **PowerShell**, porque será clave para automatización y para entender actividades sospechosas.

---

## 6. Servidor Ubuntu: tu primer servidor Linux “de empresa”

En muchas organizaciones, los servicios críticos (web, bases de datos, aplicaciones internas) corren sobre Linux.  
Por eso, vamos a usar una VM de **Ubuntu Server** para simular un servidor interno.

### 6.1. Función de este servidor

Servirá, por ejemplo, para:

- Instalar un servidor web como Apache o Nginx.
- Simular una aplicación interna.
- Probar técnicas de enumeración de puertos y servicios.
- Ver logs de acceso.
- Practicar hardening básico (desactivar servicios, revisar permisos, etc.).

### 6.2. Configuración recomendada

- CPU: 2 núcleos.
- RAM: entre 2 y 4 GB.
- Disco: 20 GB.
- Red:
  - Host-Only.

Durante la instalación:

- Asegúrate de activar **OpenSSH Server**, para poder conectarte por SSH desde Kali más adelante.
- Anota la IP de la máquina, porque la usarás en casi todos los ejercicios ofensivos y de análisis.

---

## 7. Logging desde el principio: ver qué pasa “por dentro”

Aunque todavía no estés en la parte “Blue Team”, es importante que desde el módulo 0 empieces a pensar en registros (logs).

### 7.1. ¿Por qué los logs son tan importantes?

Los logs son la memoria de lo que ha pasado en el sistema:

- Inicios de sesión.
- Errores.
- Instalación de servicios.
- Fallos de autenticación.
- Elevaciones de privilegios.

Un analista de ciberseguridad pasa buena parte de su día leyendo, filtrando y correlando logs. Cuanto antes te acostumbres a revisarlos, mejor.

### 7.2. Dónde ver logs en cada sistema

**Windows:**

- Abre el **Visor de eventos**.
- Explora sobre todo:
  - `Registros de Windows → Seguridad`.
  - `Registros de Windows → Sistema`.
- Observa cómo se registran:
  - Inicios de sesión correctos e incorrectos.
  - Instalaciones de software.
  - Cambios de configuración.

**Linux (Ubuntu y también Kali):**

- Muchos logs se guardan en `/var/log/`.
- Por ejemplo:
  - `/var/log/auth.log` → autenticaciones, accesos sudo.
  - `journalctl` → sistema unificado de logs (en sistemas con `systemd`).
- Puedes usar comandos como:
  - `sudo tail -f /var/log/auth.log` para ver en tiempo real nuevos eventos.

No hace falta que lo entiendas todo desde el primer día; lo importante es que empieces a familiarizarte con **dónde mirar** cuando pase algo.

---

## 8. Snapshots: tu “botón de rebobinar”

Un snapshot es como una fotografía de la máquina virtual en un momento concreto:

- Guarda el estado del disco.
- Guarda el estado de la memoria (si lo configuras así).
- Permite volver exactamente a ese punto.

### 8.1. ¿Por qué son imprescindibles?

En este curso vas a:

- Ejecutar comandos que pueden romper servicios.
- Instalar cosas que quizá luego no necesitas.
- Probar scripts generados por IA.

Si algo sale mal:

- No necesitas reinstalar todo.
- Basta con restaurar el snapshot.

### 8.2. Buenas prácticas con snapshots

- Crea un snapshot justo después de instalar y actualizar cada VM.  
  Por ejemplo, llámalo `Base_Limpia`.
- Antes de un laboratorio “agresivo” (explotación, cambios fuertes), crea otro snapshot.
- No acumules decenas de snapshots sin sentido; mantén los importantes y borra los que ya no necesites.

Este hábito se parece mucho a lo que se hace en entornos de pruebas profesionales.

---

## 9. Primer contacto con la nube: Azure, AWS y Google Cloud

Hoy en día, casi ninguna empresa vive solo “on-premise”. La mayoría:

- Tiene servicios en Azure, AWS o Google Cloud.
- Combina entornos híbridos (local + nube).
- Expone aplicaciones a Internet desde la nube.

En este módulo no vas a administrar la nube en profundidad, pero sí:

- Crear una cuenta gratuita (o trial).
- Desplegar una máquina virtual pequeña.
- Configurar sus reglas de acceso.

### 9.1. Conceptos clave en cualquier proveedor Cloud

Aunque el nombre cambie según el proveedor, verás conceptos comunes:

- **Máquinas virtuales (VM/EC2/Compute Engine):** servidores en la nube.
- **Grupos de seguridad / Security Groups / NSG:** reglas que controlan qué puertos están abiertos y desde dónde.
- **Storage / Buckets / Blob:** almacenamiento de ficheros u objetos.
- **IAM (Identity and Access Management):** gestión de usuarios, roles y permisos.

### 9.2. Modelo de responsabilidad compartida (explicado simple)

La nube funciona así:

- El proveedor (Azure, AWS, GCP) se encarga de la **seguridad de la infraestructura física**: centros de datos, hardware, refrigeración, electricidad, etc.
- Tú, como cliente, eres responsable de:
  - Qué puertos abres.
  - Qué usuarios creas.
  - Cómo configuras las máquinas.
  - Qué datos subes y cómo los proteges.

Esto significa que si abres el puerto 22 de una VM a todo Internet con usuario y contraseña débiles, el problema no es del proveedor, es de la configuración.

En este módulo solo haremos un despliegue básico para que entiendas esa relación.

---

## 10. IA, vibecoding y scripts en un entorno controlado

A lo largo del curso, usarás IA para generar:

- Pequeños scripts en Bash.
- Scripts en PowerShell para Windows.
- Fragmentos de Python para analizar logs o automatizar tareas.

A esto lo llamamos, de forma informal, **vibecoding con IA**: tú describes lo que quieres hacer, la IA genera el esqueleto del código, y luego tú:

- Lo revisas.
- Lo entiendes.
- Lo adaptas.
- Lo pruebas en tu laboratorio.

### 10.1. Regla profesional

Nunca ejecutes en producción (o en sistemas reales) un script:

- Que no entiendes lo que hace.
- Que no has probado antes en este laboratorio.
- Que no has revisado mínimamente línea a línea.

El laboratorio existe precisamente para que puedas equivocarte sin consecuencias graves.

---

## 11. Documentación del laboratorio: tu primer activo profesional

La documentación no es un extra; es parte del trabajo de un profesional de ciberseguridad.

Te propongo crear un fichero en tu repo:

`Lab_Setup_Report.md`

Este documento debería incluir:

- Un diagrama sencillo de la red (puede ser en ASCII o hecho a mano y subido como imagen).
- La IP de cada máquina dentro de la red Host-Only.
- Capturas de pantalla de:
  - Configuración de red de las VMs.
  - Snapshots creados.
- Listado de logs principales que has localizado en cada sistema.
- Una breve reflexión:
  - Qué has aprendido configurando el laboratorio.
  - Qué dificultades has tenido.
  - Qué mejorarías si tuvieras más recursos (más RAM, más máquinas, etc.).

Con el tiempo, este tipo de documentación se convierte en parte de tu **portfolio técnico**, algo que puedes enseñar en entrevistas.

---

## 12. Checklist final del módulo 0

Antes de pasar a la Semana 1, revisa que:

- [ ] Tienes **Kali Linux** instalado y arrancando sin errores.
- [ ] Tienes **Windows** instalado y puedes iniciar sesión con un usuario estándar.
- [ ] Tienes **Ubuntu Server** instalado y puedes conectarte por SSH desde otra máquina del laboratorio.
- [ ] Todas las VMs están dentro de la misma red **Host-Only** y pueden hacerse ping entre sí.
- [ ] Has creado al menos un snapshot llamado `Base_Limpia` en cada VM.
- [ ] Has desplegado una VM básica en algún proveedor Cloud (o al menos has revisado el panel y entendido cómo se haría).
- [ ] Has creado y rellenado tu documento `Lab_Setup_Report.md`.

Si todo esto está hecho, has dado un paso que mucha gente nunca llega a completar:  
has montado tu propio “mini-entorno corporativo” para aprender ciberseguridad de verdad.

---

## 13. Próximo paso

Cuando termines este módulo, continúa con:

`01_Fundamentos/Semana_1.md`

En la Semana 1 empezarás a poner nombres y estructura a los conceptos de ciberseguridad que luego verás en acción dentro de este laboratorio.

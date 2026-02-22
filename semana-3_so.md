# Semana 3 – Linux y Windows orientados a Seguridad 🐧🪟

## 0. Introducción a la semana

En las semanas anteriores:

- Has montado tu laboratorio (Semana 0).
- Has entendido los fundamentos de ciberseguridad (Semana 1).
- Has empezado a ver la red como algo observable y medible (Semana 2).

Ahora toca entrar en otro pilar clave: **los sistemas operativos**.

En ciberseguridad trabajarás constantemente con:

- **Linux** (sobre todo en servidores, infra de seguridad, herramientas, contenedores…).
- **Windows** (principalmente en estaciones de trabajo y también en servidores).

Si no entiendes cómo funcionan estos sistemas “por dentro”:

- Te costará interpretar logs.
- Te costará entender alertas del SOC.
- Te costará moverte en un pentest más allá del “sigo un comando de Internet”.

Esta semana no pretende convertirte en admin senior de Linux o Windows, pero sí darte:

- Un mapa claro de cómo están organizados.
- Dónde se guardan cosas importantes (configuraciones, logs, usuarios).
- Qué comandos y herramientas son básicos desde la perspectiva de seguridad.
- Cómo se vería, de forma muy simple, una acción sospechosa.

---

## 1. Objetivos de aprendizaje de la Semana 3

Al finalizar esta semana deberías ser capaz de:

1. Explicar a grandes rasgos la arquitectura de Linux (kernel, procesos, servicios, sistema de archivos).
2. Explicar la estructura básica de Windows (procesos, servicios, registro, cuentas de usuario).
3. Navegar por el sistema de archivos en Linux y localizar directorios importantes para seguridad (/etc, /var/log, /home…).
4. Usar comandos básicos en Linux para listar archivos, ver procesos, buscar texto y revisar logs.
5. Usar herramientas básicas en Windows (Task Manager, Event Viewer, services.msc) para ver qué se está ejecutando y qué ha ocurrido.
6. Comprender, a alto nivel, qué es Active Directory y por qué es tan relevante en seguridad.
7. Identificar dónde se generan y guardan los logs más importantes en ambos sistemas.
8. Realizar pequeños laboratorios prácticos que te acerquen a la visión Blue Team y ofensiva.

---

## 2. ¿Por qué es crítico entender los sistemas operativos?

En un incidente real, casi todo lo que ocurre se materializa en:

- Procesos que se ejecutan.
- Archivos que se crean/modifican.
- Servicios que se inician/detienen.
- Conexiones de red que se establecen.
- Entradas de log que se registran.

Aunque utilices SIEM, EDR, escáneres o cualquier herramienta “bonita”, en el fondo todos ellos:

- Recogen datos del sistema operativo.
- Los procesan.
- Los muestran de forma más cómoda.

Si entiendes el sistema de base:

- No dependes ciegamente de la herramienta.
- Puedes validar si una alerta tiene sentido.
- Puedes investigar manualmente cuando haga falta.

---

## 3. Linux para ciberseguridad: estructura mental

### 3.1. Arquitectura básica

De forma muy simplificada, Linux se compone de:

- **Kernel:** el “corazón” del sistema. Gestiona memoria, procesos, hardware, etc.
- **Espacio de usuario:** donde se ejecutan programas, servicios y shells (bash, zsh…).
- **Servicios y demonios:** procesos que corren en segundo plano (por ejemplo, servidor SSH).
- **Sistema de archivos:** organización de ficheros y directorios.

A diferencia de Windows, Linux agrupa casi todo en un árbol de directorios único que cuelga de `/` (la raíz). No verás C:, D:, etc.

### 3.2. Directorios importantes desde el punto de vista de seguridad

Algunos directorios clave:

- `/etc` → archivos de configuración del sistema y servicios.
- `/var/log` → logs del sistema y servicios.
- `/home` → directorios de usuarios.
- `/tmp` → directorio temporal (a menudo usado en ataques para dejar ficheros).
- `/bin`, `/usr/bin` → binarios ejecutables de programas.
- `/root` → directorio personal del usuario root (administrador).

Es importante familiarizarse con ellos, porque muchas veces:

- Las configuraciones inseguras están en `/etc`.
- Las evidencias están en `/var/log`.
- Los datos del usuario atacado están en `/home/usuario`.

### 3.3. Usuarios, grupos y permisos

En Linux todo gira en torno a:

- **Usuarios** (por ejemplo: `root`, `dani`, `www-data`).
- **Grupos** (colecciones de usuarios con permisos comunes).
- **Permisos de archivos y directorios** (lectura, escritura, ejecución).

Un archivo típico puede tener permisos como:

```bash
-rw-r--r-- 1 dani users 1234 feb 21  file.txt
```

Interpretación simplificada:

- Propietario: `dani`
- Grupo: `users`
- Permisos:
  - Propietario: lectura, escritura (`rw-`)
  - Grupo: solo lectura (`r--`)
  - Otros: solo lectura (`r--`)

Desde seguridad, esto es importante porque:

- Permisos demasiado abiertos (`777`) pueden permitir a cualquiera modificar o ejecutar algo.
- Archivos sensibles deberían estar restringidos.

### 3.4. Comandos básicos como herramientas de análisis

Algunos comandos muy usados:

- `ls`, `cd`, `pwd` → navegar y ver ficheros.
- `cat`, `less`, `tail` → leer ficheros.
- `tail -f` → ver logs en tiempo real.
- `grep` → buscar texto dentro de ficheros (ideal para filtrar logs).
- `ps`, `top`, `htop` → ver procesos en ejecución.
- `journalctl` → ver logs del sistema cuando se usa systemd.
- `systemctl` → gestionar servicios (arrancar, parar, estado).

En seguridad:

- Puedes usar `ps`/`top` para ver procesos sospechosos.
- Puedes usar `grep` + `/var/log/auth.log` para ver intentos de login.
- Puedes revisar servicios que hayan aparecido de repente con `systemctl list-units --type=service`.

### 3.5. Redes en Linux: comandos esenciales

Para conectar la visión de redes de la Semana 2 con Linux:

- `ip addr` → ver direcciones IP asignadas.
- `ip route` → ver tabla de rutas.
- `ss -tulpn` (o `netstat -tulpn` en algunos sistemas) → ver puertos abiertos y qué proceso los usa.
- `ping`, `traceroute`, `curl` → comprobar conectividad y servicios.

Desde el punto de vista de un analista o pentester:

- `ss -tulpn` te permite ver qué servicios escuchan en la máquina (superficie de ataque interna).
- `curl` te permite probar respuestas HTTP sin navegador.

### 3.6. Logs importantes en Linux

Depende de la distribución, pero típicamente tienes:

- `/var/log/auth.log` → autenticaciones (ssh, sudo, etc.).
- `/var/log/syslog` → mensajes generales del sistema.
- `/var/log/nginx/*` o `/var/log/apache2/*` → si tienes servidores web.
- `journalctl` → vista unificada de logs.

EJEMPLO:

Si alguien intenta hacer un login SSH fallido, verás algo como:

```text
Feb 21 10:15:32 ubuntu sshd[1234]: Failed password for invalid user test from 192.168.56.50 port 54321 ssh2
```

Esto es justo el tipo de línea que un SOC ingestaría para crear alertas.

---

## 4. Windows para ciberseguridad: estructura mental

### 4.1. Arquitectura básica de Windows

Windows también se puede ver por capas:

- **Kernel**: gestión de recursos y bajo nivel.
- **Servicios**: procesos en segundo plano que proporcionan funcionalidades (ejemplo: servicio de actualizaciones).
- **Procesos de usuario**: programas que abres (navegador, office, etc.).
- **Registro (Registry)**: base de datos jerárquica donde Windows guarda configuraciones del sistema, aplicaciones, drivers, etc.

Cuando hablamos de seguridad en Windows, solemos prestar atención a:

- Procesos que se ejecutan (normales vs anómalos).
- Servicios (nuevos servicios sospechosos, cambios en servicios existentes).
- Claves de registro relacionadas con persistencia.
- Cuentas de usuario y grupos locales.
- Políticas de seguridad (GPO en entornos de dominio).

### 4.2. Usuarios, grupos y UAC

En Windows existen:

- Cuentas de usuario locales.
- Grupos (Administradores, Usuarios, Invitados, etc.).
- Cuentas de dominio (cuando estamos en un entorno con Active Directory, que veremos en otro módulo).

El **Control de cuentas de usuario (UAC)** es el mecanismo que hace saltar la ventana de “¿Desea permitir que esta aplicación realice cambios en el dispositivo?”. Su función es:

- Evitar que cualquier programa pueda obtener privilegios elevados sin que el usuario lo sepa.
- Aunque no es infalible, añade una capa más de protección.

### 4.3. Herramientas básicas para análisis en Windows

#### Task Manager (Administrador de tareas)

- Permite ver procesos en ejecución.
- Uso de CPU, memoria, disco, red.
- Puedes ordenar por uso de recursos y detectar procesos anómalos.

#### Event Viewer (Visor de eventos)

- Herramienta crítica para seguridad.
- Registros principales:
  - **Application**
  - **System**
  - **Security** (especialmente importante para ciberseguridad).

En **Security** verás:

- Inicios de sesión correctos e incorrectos.
- Cambios de permisos.
- Eventos de auditoría.

#### services.msc

- Lista y permite gestionar los servicios de Windows.
- Desde aquí puedes ver:
  - Servicios automáticos.
  - Servicios deshabilitados.
  - Servicios en ejecución que podrían ser sospechosos si no deberían estar ahí.

#### msconfig / Configuración de inicio

- Permite ver qué programas se inician con el sistema.
- Útil para identificar mecanismos de persistencia básicos.

### 4.4. Comandos útiles en Windows (CMD y PowerShell)

Algunos comandos relevantes:

En **CMD**:

- `whoami` → usuario actual.
- `ipconfig` → configuración de red.
- `netstat -ano` → conexiones de red y puertos abiertos.
- `tasklist` → lista de procesos.

En **PowerShell**:

- `Get-Process` → procesos en ejecución.
- `Get-Service` → servicios.
- `Get-EventLog` o `Get-WinEvent` → acceso a eventos.
- `Test-Connection` → similar a ping.

Para un analista, poder moverme entre interfaz gráfica y terminal de Windows es muy útil para automatizar tareas y para análisis más avanzados.

### 4.5. Introducción muy breve a Active Directory

Active Directory (AD) es el servicio de directorio de Microsoft utilizado en:

- Empresas medianas y grandes.
- Entornos donde hay muchos usuarios y equipos.

AD permite:

- Gestionar usuarios y grupos de forma centralizada.
- Aplicar políticas (GPO) a equipos o usuarios.
- Gestionar permisos en recursos compartidos.

A nivel de seguridad:

- AD es una joya para un atacante: si compromete una cuenta con privilegios elevados, puede controlar todo el dominio.
- Muchos ataques avanzados se centran en moverse dentro de AD y escalar privilegios.

En este curso, más adelante, verás conceptos básicos de ataques a AD. Por ahora, quédate con:

> AD = el “cerebro de identidad” de muchos entornos Windows empresariales.

---

## 5. Enlace con seguridad: ¿qué buscan los atacantes en sistemas?

Tanto en Linux como en Windows, los atacantes buscan:

- **Ejecutar código** (lanzar procesos, scripts, binarios).
- **Mantener persistencia** (que su acceso no se pierda al reiniciar).
- **Escalar privilegios** (pasar de usuario normal a admin/root).
- **Moverse lateralmente** (saltar a otras máquinas).
- **Borrar rastros** (modificar o eliminar logs).

Ejemplos:

- En Linux: añadir una clave SSH maliciosa en `~/.ssh/authorized_keys`.
- En Windows: crear un servicio que ejecuta un binario malicioso al inicio.
- En ambos: programar tareas que ejecutan scripts a intervalos.

Por eso es tan importante saber:

- Dónde se guardan las llaves (configuraciones, usuarios, tareas programadas).
- Dónde se quedan las huellas (logs).

---

## 6. Laboratorio de la Semana 3

Vamos ahora a usar tu laboratorio:

- Ubuntu Server y/o Kali para la parte Linux.
- Windows para la parte Windows.

### 6.1. Ejercicio 1 – Explorando el sistema de archivos en Linux

En tu Ubuntu Server (o en Kali si lo prefieres):

1. Abre una terminal.
2. Navega por los siguientes directorios y describe qué ves:

   ```bash
   cd /
   ls
   cd /etc
   ls
   cd /var/log
   ls
   cd /home
   ls
   ```

3. Crea un archivo de prueba en tu home:

   ```bash
   cd ~
   echo "prueba seguridad" > fichero_seguridad.txt
   ls -l fichero_seguridad.txt
   ```

4. Observa los permisos y el propietario.
5. Cambia los permisos para que otros usuarios no puedan leer el archivo:

   ```bash
   chmod 600 fichero_seguridad.txt
   ls -l fichero_seguridad.txt
   ```

Documenta:

- Qué directorios te han parecido más “sensibles” desde el punto de vista de seguridad.
- Cómo han cambiado los permisos con `chmod`.

Guárdalo en:

`Semana3_Linux_SistemaArchivos.md`

---

### 6.2. Ejercicio 2 – Usuarios, sudo y logs de autenticación

En Ubuntu:

1. Crea un nuevo usuario (por ejemplo `analista`):

   ```bash
   sudo adduser analista
   ```

2. Intenta cambiar a ese usuario:

   ```bash
   su - analista
   ```

3. Desde ese usuario, intenta ejecutar un comando con `sudo` (fallará si no tiene permisos):

   ```bash
   sudo ls /root
   ```

4. Vuelve a tu usuario original.
5. Consulta el log de autenticación:

   ```bash
   sudo grep analista /var/log/auth.log | tail -n 20
   ```

Observa:

- Cómo se ha registrado el intento de uso de `sudo`.
- Qué información aparece (usuario, hora, comando, etc.).

Esto es exactamente el tipo de evento que un SIEM podría usar para crear una alerta de “intento de escalar privilegios”.

Documenta en:

`Semana3_Linux_Usuarios_y_Logs.md`

---

### 6.3. Ejercicio 3 – Procesos y servicios en Linux

En tu Ubuntu o Kali:

1. Lista los procesos con:

   ```bash
   ps aux | head
   top
   ```

2. Observa:
   - Qué procesos consumen más CPU o memoria.
   - Qué procesos pertenecen a qué usuarios.

3. Lista servicios con systemd:

   ```bash
   systemctl list-units --type=service | head -n 20
   ```

4. Opcional: Si tienes un servidor web (nginx, apache) instalado:
   - Comprueba si está activo (`systemctl status nginx` por ejemplo).

Reflexiona:

- ¿Cómo podría ayudarte esto a detectar algo raro? (por ejemplo, un servicio que no conoces, un proceso sospechoso).

Añádelo a:

`Semana3_Linux_Procesos_Servicios.md`

---

### 6.4. Ejercicio 4 – Procesos y eventos en Windows

En la máquina Windows:

1. Abre el **Administrador de tareas (Task Manager)**.
2. Ve a la pestaña de **Procesos**.
3. Localiza:
   - El navegador si lo tienes abierto.
   - Algún proceso de sistema conocido (por ejemplo, `explorer.exe`).

4. Abre el **Visor de eventos (Event Viewer)**.
5. Navega a:
   - `Registros de Windows → Seguridad`.
6. Inicia y cierra sesión en tu usuario.
7. Vuelve al Visor de eventos y filtra por los eventos de los últimos minutos.

Reflexiona:

- ¿Qué tipo de eventos ves?
- ¿Puedes identificar eventos de inicio/cierre de sesión?

Documenta capturas y comentarios en:

`Semana3_Windows_Procesos_y_Eventos.md`

---

### 6.5. Ejercicio 5 – Primer “mini-hunting” casero

Objetivo: ver cómo podría “cantar” algo sospechoso en Windows.

1. En Windows, abre PowerShell.
2. Ejecuta un comando poco habitual para un usuario normal, por ejemplo:

   ```powershell
   powershell -NoProfile -WindowStyle Hidden -Command "ping 8.8.8.8 -n 3"
   ```

   (Puedes lanzarlo desde `cmd` llamando a `powershell` con esos parámetros.)

3. Observa en el Administrador de tareas:
   - ¿Ves algún proceso de PowerShell?
   - ¿Cuánto tiempo permanece?

4. Después, explora el Visor de eventos:
   - Busca eventos relacionados con PowerShell (en `Application` o `Windows PowerShell` / `Microsoft-Windows-PowerShell/Operational` si está habilitado).

Este ejercicio es muy simple, pero te ayuda a pensar:

> “Si un malware lanzara PowerShell de forma oculta, ¿cómo podría verlo?”

Anota tus observaciones en:

`Semana3_Windows_MiniHunting.md`

---

## 7. Entregables de la Semana 3

Al finalizar la semana deberías haber generado:

1. `Semana3_Linux_SistemaArchivos.md`  
   - Descripción de directorios importantes.
   - Experimento con permisos usando `chmod`.

2. `Semana3_Linux_Usuarios_y_Logs.md`  
   - Creación de usuario.
   - Intentos de uso de `sudo`.
   - Extracto de logs de `/var/log/auth.log`.

3. `Semana3_Linux_Procesos_Servicios.md`  
   - Observaciones sobre procesos (`ps`, `top`).
   - Lista de servicios (`systemctl`).
   - Reflexión sobre cómo detectar algo raro.

4. `Semana3_Windows_Procesos_y_Eventos.md`  
   - Capturas o descripciones de Task Manager.
   - Eventos de inicio/cierre de sesión o similares en Event Viewer.

5. `Semana3_Windows_MiniHunting.md`  
   - Observaciones al ejecutar PowerShell con parámetros poco habituales.
   - Posibles ideas de detección.

Y, si quieres ser muy ordenado, puedes ampliar tu `Lab_Setup_Report.md` con:

- Sección “Puntos de log importantes” para cada sistema.
- Resumen de las rutas de logs que te interesan más.

---

## 8. Checklist de cierre de la Semana 3

Marca como completado cuando:

- [ ] Puedes explicar cómo está organizado el sistema de archivos de Linux y qué directorios son críticos para seguridad.
- [ ] Sabes crear un usuario en Linux y has visto cómo se registra en logs un intento de usar `sudo`.
- [ ] Entiendes cómo ver procesos y servicios en Linux y tienes una idea de cómo detectar actividades anómalas.
- [ ] Puedes moverte por Windows utilizando Task Manager y Visor de eventos sin perderte.
- [ ] Has visto en directo cómo tus acciones (iniciar sesión, ejecutar comandos) se reflejan en los logs de Windows y Linux.
- [ ] Has creado todos los ficheros de la Semana 3 y los has añadido a tu repositorio.

Si has completado todo esto, ya no ves Linux y Windows como “cajas negras”, sino como sistemas donde puedes:

- Navegar.
- Observar.
- Correlacionar acciones con evidencias.

En la **Semana 4**, daremos un paso más y nos centraremos en **almacenamiento, datos, copias de seguridad, ransomware y gestión de identidad**, que te conectará muy bien con escenarios reales de pérdida de datos y ataques a información crítica.

---

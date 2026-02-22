
# Semana 1 – Fundamentos de Ciberseguridad 🔐

## 0. Introducción a la semana

En esta primera semana vas a construir la base conceptual que vas a usar durante todo el curso y, de hecho, durante toda tu carrera en ciberseguridad.

El objetivo no es que memorices siglas, sino que entiendas:

- Qué protege realmente la ciberseguridad.
- De qué tipo de amenazas estamos hablando.
- Cómo se estructura un ataque moderno.
- Qué roles existen en un equipo de seguridad.
- Qué ocurre cuando hay un incidente y cómo se gestiona.

Tu laboratorio de la **Semana 0** ya debería estar listo.  
En esta semana ya vamos a utilizarlo de forma ligera (para ejemplos sencillos y algún ejercicio de reflexión), pero el foco principal estará en asentar conceptos.

---

## 1. Objetivos de aprendizaje de la Semana 1

Al finalizar esta semana deberías ser capaz de:

1. Explicar con tus palabras qué es la ciberseguridad y qué protege.
2. Describir la triada **CIA** (Confidencialidad, Integridad y Disponibilidad) con ejemplos concretos.
3. Diferenciar entre **activo**, **amenaza**, **vulnerabilidad** y **riesgo**.
4. Reconocer los tipos de ataques más comunes (phishing, malware, ransomware, fuerza bruta…).
5. Describir el ciclo de vida de un ataque (Kill Chain) a alto nivel.
6. Entender qué es **MITRE ATT&CK** y por qué es tan usado en la industria.
7. Identificar los principales roles en ciberseguridad (SOC, pentesting, ingeniería de seguridad, etc.).
8. Comprender las fases básicas de gestión de un incidente de seguridad.

---

## 2. ¿Qué es ciberseguridad realmente?

### 2.1. Definición sencilla

La ciberseguridad es el conjunto de **personas, procesos y tecnologías** que se utilizan para proteger:

- Sistemas
- Redes
- Aplicaciones
- Datos

frente a accesos no autorizados, manipulación, destrucción o interrupción.

No es solo “instalar un antivirus” ni “configurar un firewall”.  
Implica entender **qué es importante para la organización** (sus activos críticos) y cómo podrían verse comprometidos.

---

## 3. La triada CIA: Confidencialidad, Integridad, Disponibilidad

La triada **CIA** es uno de los pilares fundamentales de la seguridad.

### 3.1. Confidencialidad

La confidencialidad busca que **solo las personas autorizadas puedan acceder a la información**.

Ejemplos:

- Historias clínicas de un hospital: solo el personal sanitario autorizado debería verlas.
- Nóminas de empleados: no deberían ser visibles para cualquiera.

Controles típicos:

- Autenticación (usuario y contraseña, MFA).
- Autorización (permisos, roles).
- Cifrado (en disco, en tránsito).

### 3.2. Integridad

La integridad busca que la información **no sea alterada de forma no autorizada**, es decir, que los datos sean correctos y completos.

Ejemplos:

- Un atacante que cambia el número de cuenta en una factura.
- Un log de seguridad que se modifica para ocultar rastros.

Controles típicos:

- Checksums y hashes.
- Firmas digitales.
- Control de versiones.
- Registros de auditoría.

### 3.3. Disponibilidad

La disponibilidad asegura que los sistemas y datos **están accesibles cuando se necesitan**.

Ejemplos:

- Una web de comercio electrónico que cae el día de más ventas.
- Un ataque DDoS que deja inoperativa una API crítica.

Controles típicos:

- Redundancia de sistemas.
- Copias de seguridad.
- Alta disponibilidad.
- Planes de recuperación ante desastres.

### 3.4. Ejemplo integrador

Piensa en una aplicación bancaria:

- **Confidencialidad:** nadie debe ver el saldo de otra persona.
- **Integridad:** el saldo, transferencias y movimientos deben ser correctos.
- **Disponibilidad:** el usuario debe poder acceder al servicio cuando lo necesite.

Cualquier ataque que afecte a la ciberseguridad golpeará, de una forma u otra, a uno o varios de estos tres pilares.

---

## 4. Activos, amenazas, vulnerabilidades y riesgo

Antes de hablar de ataques, hay que tener clara esta terminología básica.

### 4.1. Activo

Un activo es **algo de valor para la organización**.

Puede ser:

- Un servidor.
- Una base de datos.
- Una aplicación web.
- Credenciales de administrador.
- Información de clientes.
- Incluso una persona clave (conocimiento, acceso).

### 4.2. Amenaza

Una amenaza es **cualquier cosa que pueda causar daño a un activo**.

Ejemplos:

- Un atacante externo.
- Un empleado descontento.
- Un fallo eléctrico.
- Un malware.
- Un error humano (borrar datos por accidente).

### 4.3. Vulnerabilidad

Una vulnerabilidad es una **debilidad** que puede ser explotada por una amenaza.

Ejemplos:

- Un sistema sin parches.
- Una contraseña débil.
- Un puerto expuesto innecesario.
- Una mala configuración de permisos.

### 4.4. Riesgo

El riesgo combina:

- **Probabilidad** de que una amenaza explote una vulnerabilidad
- con el **impacto** que tendría sobre el activo.

En términos simples:

> Riesgo = (Probabilidad de que ocurra algo malo) x (Impacto si ocurre)

En la práctica, el trabajo de ciberseguridad consiste en **reducir el riesgo** a un nivel aceptable para la organización, no en eliminarlo completamente (eso es imposible).

---

## 5. Tipos de ataques más comunes

No hay que memorizar una lista infinita, pero sí entender las ideas básicas.

### 5.1. Phishing

Ataques que intentan engañar al usuario para que:

- Entregue sus credenciales.
- Haga clic en un enlace malicioso.
- Abra un adjunto infectado.

Ejemplo típico:

> Un correo que imita a tu banco y te pide “verificar tu cuenta” a través de un enlace.

### 5.2. Malware

“Malicious software”:

- Virus
- Gusanos
- Troyanos
- Keyloggers
- Backdoors

Se instalan en el sistema para realizar acciones no deseadas: robar información, cifrar datos, unirse a una botnet, etc.

### 5.3. Ransomware

Tipo de malware que **cifra los datos** de la víctima y exige un pago (ransom) para recuperarlos.

Impacta especialmente en:

- Integridad (se alteran los ficheros).
- Disponibilidad (no se pueden usar).

### 5.4. Ataques de fuerza bruta

Intentos de adivinar contraseñas probando muchas combinaciones posibles, a menudo de forma automatizada.

Mitigación típica:

- Limitar intentos.
- MFA.
- Políticas de complejidad de contraseñas.

### 5.5. Ingeniería social

Ataques que se centran más en las personas que en la tecnología:

- Llamadas telefónicas fingiendo ser soporte técnico.
- Mensajes urgentes para que el usuario actúe sin pensar.

Muchas brechas de seguridad empiezan con un buen ataque de ingeniería social.

---

## 6. Ciclo de vida de un ataque (Kill Chain)

Para entender cómo piensa un atacante, se suele usar el concepto de **Kill Chain**, que describe las fases típicas de un ataque.

Distintas fuentes definen fases ligeramente diferentes, pero una versión simplificada puede ser:

1. Reconocimiento
2. Preparación / Armamento
3. Entrega
4. Explotación
5. Instalación
6. Comando y Control (C2)
7. Acciones sobre el objetivo

### 6.1. Reconocimiento

El atacante recopila información:

- ¿Qué dominios tiene la empresa?
- ¿Qué servicios tienen expuestos?
- ¿Qué correos de empleados aparecen en LinkedIn?

### 6.2. Preparación / Armamento

El atacante prepara la herramienta:

- Crea un documento malicioso.
- Configura un exploit kit.
- Prepara un servidor de C2.

### 6.3. Entrega

Es el momento en el que la amenaza llega al entorno de la víctima:

- Correo con adjunto.
- Enlace malicioso.
- Paquetes maliciosos enviados a un servicio expuesto.

### 6.4. Explotación

Aprovecha una vulnerabilidad:

- Ejecución del adjunto.
- Explotar un servicio web vulnerable.
- Fallo de validación de entrada.

### 6.5. Instalación

El atacante instala malware o crea persistencia:

- Servicio que se reinicia con el sistema.
- Tareas programadas.
- Usuarios ocultos.

### 6.6. Comando y Control (C2)

La máquina comprometida se comunica con el atacante:

- Conexiones salientes a IP o dominio del atacante.
- Uso de túneles cifrados.
- Proxies o servicios intermedios.

### 6.7. Acciones sobre el objetivo

Una vez dentro, el atacante hace lo que busca:

- Robar información.
- Cifrar datos (ransomware).
- Moverse a otros sistemas (movimiento lateral).
- Destruir o alterar datos.

Entender estas fases te ayudará a:

- Pensar como atacante (Pentesting / Red Team).
- Pensar como defensor (Blue Team / SOC).

---

## 7. Introducción a MITRE ATT&CK

**MITRE ATT&CK** es un marco (framework) que describe:

- Tácticas: el “por qué” y el “para qué” de las acciones del atacante.
- Técnicas: el “cómo” se logran esas tácticas.

Por ejemplo:

- Táctica: **Ejecución** (el atacante quiere ejecutar código malicioso).
- Técnica: **PowerShell** (utiliza scripts de PowerShell para ejecutar comandos).

MITRE ATT&CK se ha convertido en un lenguaje común entre:

- SOC
- Threat Hunters
- Red Team
- Blue Team
- Fabricantes de herramientas de seguridad

En secciones posteriores del curso lo usarás para:

- Clasificar actividades de ataque.
- Diseñar reglas de detección.
- Planificar pruebas de seguridad.

Por ahora, quédate con la idea de que MITRE ATT&CK es un catálogo muy organizado de **cómo atacan los adversarios en el mundo real**.

---

## 8. Roles en ciberseguridad: dónde encaja todo esto

La ciberseguridad no es un único trabajo. Hay muchos roles, por ejemplo:

### 8.1. Analista SOC (Blue Team)

- Monitoriza alertas de seguridad.
- Revisa logs en un SIEM.
- Investiga comportamiento sospechoso.
- Escribe informes de incidentes.

Este curso te prepara para tener:

- Base de redes.
- Base de sistemas.
- Entendimiento de ataques.
- Primer contacto con detección.

### 8.2. Pentester / Hacking Ético (Red Team orientado a test)

- Realiza auditorías de seguridad pactadas.
- Busca vulnerabilidades.
- Intenta explotarlas de forma controlada.
- Documenta hallazgos y propone soluciones.

Este roadmap incluye:

- Fundamentos ofensivos.
- Laboratorios de explotación.
- Tareas de post-explotación básicas.

### 8.3. Ingeniero de Seguridad

- Diseña arquitecturas seguras.
- Define controles técnicos (firewalls, WAF, segmentación).
- Automatiza tareas de seguridad.
- Trabaja muy cerca de equipos de IT y desarrollo.

Este curso te da:

- Visión general de infraestructura y cloud.
- Introducción a automatización (Python, PowerShell, Bash).
- Conciencia de logs y detección.

### 8.4. Purple Team (Colaboración Red + Blue)

- Mezcla mentalidad ofensiva y defensiva.
- Repite ataques controlados mientras el Blue Team intenta detectarlos.
- Mejora la detección de forma iterativa.

La idea del curso es que:

> No seas “solo” ofensivo o defensivo, sino que entiendas ambos lados.

---

## 9. Ciclo de gestión de incidentes

Cuando ocurre algo sospechoso en una empresa, no basta con “apagar el servidor y ya está”. Hay procedimientos.

Un modelo común (inspirado en NIST) incluye:

1. Preparación
2. Detección y análisis
3. Contención
4. Erradicación
5. Recuperación
6. Lecciones aprendidas

### 9.1. Preparación

- Formación del equipo.
- Herramientas de monitorización.
- Procedimientos definidos.
- Copias de seguridad probadas.

### 9.2. Detección y análisis

- Se recibe una alerta (SIEM, EDR, IDS, usuario).
- Se analiza si es un falso positivo o un incidente real.
- Se determina el alcance: sistemas afectados, usuarios, datos.

### 9.3. Contención

- Frenar el impacto.
- Aislar equipos comprometidos.
- Bloquear usuarios o accesos maliciosos.

Aquí hay decisiones difíciles: contener rápido vs no destruir evidencias.

### 9.4. Erradicación

- Eliminar malware.
- Cerrar vulnerabilidades explotadas.
- Cambiar credenciales comprometidas.

### 9.5. Recuperación

- Restaurar servicios.
- Validar que todo funciona.
- Asegurar que no hay persistencia residual.

### 9.6. Lecciones aprendidas

- ¿Qué falló?
- ¿Qué se podría haber detectado antes?
- ¿Qué controles se deben mejorar?
- ¿Qué formación necesita el personal?

En el proyecto final del curso tendrás que aplicar este ciclo.

---

## 10. Laboratorio de la Semana 1

Aunque esta semana es muy conceptual, ya podemos hacer algunos ejercicios sencillos en tu laboratorio.

### 10.1. Ejercicio 1: Identificar activos y riesgos en tu propio laboratorio

1. Abre tu documento `Lab_Setup_Report.md`.
2. Añade una sección llamada `Activos y Riesgos`.
3. Para cada máquina (Kali, Windows, Ubuntu), responde:
   - ¿Qué activos importantes tiene? (por ejemplo, credenciales, logs, servicios).
   - ¿Qué amenazas puedes imaginar? (malware, ataque desde otra VM, malas configuraciones).
   - ¿Qué vulnerabilidades potenciales tendría si:
     - No la actualizas.
     - Usas contraseñas débiles.
     - Expones puertos innecesarios.
4. Intenta escribir una frase de **riesgo** para cada máquina.  
   Ejemplo:

   > “Si la máquina Ubuntu Server no se actualiza y mantiene servicios innecesarios abiertos, un atacante interno (Kali) podría explotar una vulnerabilidad para tomar el control del sistema, comprometiendo la integridad y confidencialidad de los datos.”

Este ejercicio te obliga a pensar como analista y no solo como “usuario de la VM”.

---

### 10.2. Ejercicio 2: CIA en acción

En tu laboratorio:

1. Piensa en un ejemplo de **confidencialidad**:
   - ¿Qué pasaría si cualquiera pudiera leer los logs de autenticación de Ubuntu?
   - ¿Qué información sensible contienen?
2. Piensa en un ejemplo de **integridad**:
   - ¿Qué ocurriría si alguien modificara esos logs para borrar evidencias?
3. Piensa en un ejemplo de **disponibilidad**:
   - ¿Qué pasaría si Windows dejara de arrancar por un fallo deliberado?

Anota estos ejemplos en un fichero llamado:

`Semana1_CIA_Ejemplos.md`

No hace falta que hagas nada técnico todavía, solo que empieces a ver los sistemas con estas “gafas de seguridad”.

---

### 10.3. Ejercicio 3: Primer vistazo a logs

En Ubuntu Server:

1. Abre una terminal.
2. Ejecuta:

   ```bash
   whoami
   ```

3. Después, intenta acceder por SSH desde Kali (aunque sea solo para hacer login y logout).
4. En Ubuntu, revisa:

   ```bash
   sudo tail -n 50 /var/log/auth.log
   ```

Observa:

- Cómo queda registrado tu intento de conexión.
- Qué campos aparecen (usuario, IP de origen, hora, etc.).

No es necesario entender cada línea, pero sí ver que:

- Tus acciones dejan rastro.
- Esos rastros son la materia prima de un SOC.

---

## 11. Entregables de la Semana 1

Esta semana deberías generar:

1. `Semana1_Resumen_Conceptos.md`  
   Un documento corto donde expliques con tus palabras:
   - Qué es ciberseguridad.
   - Qué es CIA.
   - Diferencia entre amenaza, vulnerabilidad y riesgo.
   - Qué es la Kill Chain.
   - Qué es MITRE ATT&CK (a alto nivel).

2. `Semana1_CIA_Ejemplos.md`  
   Con los ejemplos que has pensado en el laboratorio.

3. Ampliación de `Lab_Setup_Report.md`  
   Añadiendo la sección de Activos y Riesgos.

---

## 12. Checklist de cierre de la Semana 1

Marca como completado cuando:

- [ ] Puedes explicar la triada CIA con ejemplos propios.
- [ ] Eres capaz de definir amenaza, vulnerabilidad, activo y riesgo sin mirar apuntes.
- [ ] Conoces los principales tipos de ataques (phishing, malware, ransomware, fuerza bruta).
- [ ] Has entendido el ciclo de vida de un ataque (Kill Chain) a grandes rasgos.
- [ ] Sabes qué es MITRE ATT&CK y para qué se usa, aunque no lo domines.
- [ ] Diferencias entre rol de analista SOC, pentester e ingeniero de seguridad.
- [ ] Has creado los archivos `Semana1_Resumen_Conceptos.md` y `Semana1_CIA_Ejemplos.md`.
- [ ] Has ampliado tu `Lab_Setup_Report.md` con el análisis de activos y riesgos.

Si todo esto está hecho, has completado una base conceptual sólida.  
A partir de la Semana 2 empezaremos a profundizar en **redes**, que es el terreno donde muchos ataques y defensas cobran sentido práctico.

---
````



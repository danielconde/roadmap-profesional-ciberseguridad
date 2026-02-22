# Semana 1 – Fundamentos de Ciberseguridad 🔐

---

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

### 0.1. ¿Por qué empezar por fundamentos?

Antes de ver comandos, herramientas o entornos complejos, necesitas un “mapa mental” de cómo funciona el mundo de la ciberseguridad. Si intentas aprender directamente hacking, logs o cloud sin este mapa, todo se convierte en una lista caótica de cosas que no sabes muy bien cómo encajan.

A lo largo de esta semana vas a:

- Familiarizarte con vocabulario que escucharás constantemente en cualquier trabajo de seguridad.
- Entender cómo piensa un atacante y cómo debería pensar un defensor.
- Ver la ciberseguridad no solo como algo técnico, sino como algo que afecta directamente al negocio.

Piensa esta semana como aprender a leer antes de escribir: no vas a hacer cosas “espectaculares” todavía, pero todo lo espectacular que hagas después dependerá de que esto esté claro.

### 0.2. Qué se espera de ti al terminar la semana

No se espera que seas experto, pero sí que:

- Puedas explicar con tus palabras los conceptos básicos sin necesidad de mirar siempre apuntes.
- Seas capaz de leer una noticia de ciberseguridad y entender al menos de qué va a nivel general.
- Puedas seguir una conversación técnica básica sin perderte completamente.

```bash
**Prompt IA:**  
Actúa como mentor de ciberseguridad para principiantes.  
Explícame por qué es tan importante empezar por fundamentos (CIA, riesgo, tipos de ataques, roles, incidentes) antes de pasar a temas más avanzados como hacking, malware o cloud.  
Quiero una explicación motivadora, con ejemplos sencillos y comparaciones del día a día, orientada a alguien que está empezando desde cero.
```
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

### 1.1. Cómo leer estos objetivos

Cada objetivo está pensado para que tengas una habilidad concreta, no solo “saber definir algo”:

- Cuando expliques qué es ciberseguridad, deberías poder adaptarlo:
  - A un amigo no técnico.
  - A un jefe de negocio.
  - A otro técnico.
- Cuando hables de CIA, deberías poder acompañarlo siempre de un ejemplo real o cercano.
- Cuando escuches “ataque de fuerza bruta”, “phishing” o “ransomware”, deberías visualizar mentalmente qué está ocurriendo y qué impacto puede tener en la empresa.

### 1.2. Ejemplos de uso real de estos objetivos

- En una entrevista para un puesto junior en un SOC, es muy probable que te pregunten:
  - “Explícame con tus palabras qué es ciberseguridad”.
  - “¿Qué es un riesgo? ¿Y una vulnerabilidad?”.
  - “¿Qué es la triada CIA?”.
- En tu trabajo, cuando tengas que escribir un informe:
  - Tendrás que indicar si un incidente afecta a confidencialidad, integridad o disponibilidad.
  - Te pedirán que describas “el tipo de ataque” asociado a una alerta.

Dominar esta lista no es algo teórico: es la base sobre la que vas a comunicarte y trabajar.

```bash
**Prompt IA:**  
Quiero que actúes como entrevistador técnico de un puesto junior SOC.  
Genera 10 preguntas basadas en los objetivos de aprendizaje de esta semana (definición de ciberseguridad, CIA, riesgo, tipos de ataque, Kill Chain, MITRE, roles, incidentes) y, para cada pregunta, una respuesta modelo en lenguaje claro, como lo diría un candidato preparado pero junior.  
Lo usaré como guía de autoevaluación al final de la semana.
```
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

### 2.2. Tres dimensiones de la ciberseguridad

Para entender de verdad qué es ciberseguridad, es útil verla desde tres perspectivas distintas: técnica, empresarial y estratégica.

#### 2.2.1. Perspectiva Técnica

Desde un enfoque técnico, la ciberseguridad es la implementación de controles proactivos y reactivos sobre la infraestructura lógica. Eso incluye:

- Endurecer (hardening) sistemas: desactivar servicios innecesarios, limitar usuarios, aplicar parches.
- Segmentar redes: separar entornos de usuarios, servidores, producción, administración…
- Gestionar el ciclo de vida de vulnerabilidades: descubrir, evaluar, priorizar y remediar fallos.

El foco está en la **superficie de ataque** (todo aquello que un atacante podría intentar explotar) y en la **telemetría**: la capacidad de recolectar logs de firewalls, EDRs y servidores para convertirlos en eventos accionables.

Técnicamente, lo que buscamos es **aumentar el coste del ataque** para el adversario: que tenga que usar técnicas más complejas, más ruidosas, más fáciles de detectar en nuestro SIEM.

Ejemplo:

- Un servidor expuesto con RDP abierto y sin MFA → ataque barato para el atacante.  
- El mismo servidor tras endurecimiento, con MFA, detrás de VPN y monitorizado → el atacante necesita más tiempo, más recursos y probablemente será detectado.

#### 2.2.2. Perspectiva Empresarial

Desde el punto de vista del negocio, la ciberseguridad no se mide en “número de alertas” sino en **riesgo y continuidad operativa**.

La pregunta clave del negocio no es “¿cuántos incidentes hemos tenido?”, sino:

- “¿Podemos seguir operando si pasa X?”.  
- “¿Cuál sería el impacto económico si paramos 4 horas?”.  
- “¿Podemos cumplir con las regulaciones (GDPR, NIS2, etc.)?”.

Para el negocio, la ciberseguridad se traduce en:

- Métricas de resiliencia.  
- SLAs (tiempos de respuesta y recuperación).  
- MTTD (Mean Time to Detect).  
- MTTR (Mean Time to Respond).

El analista de seguridad no solo “bloquea malware”, sino que contribuye a mitigar el impacto financiero y reputacional de una brecha. Su trabajo ayuda a que los activos críticos —las “joyas de la corona”— permanezcan íntegros y disponibles.

#### 2.2.3. Perspectiva Estratégica

La perspectiva estratégica mira más allá del incidente puntual y se centra en **cómo queremos defendernos como organización** a medio y largo plazo.

Aquí hablamos de:

- Alinear la ciberseguridad con los objetivos de la empresa.  
- Priorizar qué se protege primero.  
- Adoptar frameworks como MITRE ATT&CK o NIST CSF para tener un lenguaje común.

La seguridad estratégica pasa de una defensa puramente reactiva (“apagamos fuegos”) a una basada en inteligencia de amenazas:

- ¿Qué actores atacan a nuestro sector?  
- ¿Qué técnicas utilizan?  
- ¿Qué brechas públicas han sufrido empresas similares?

No se trata de detectar “malware” de forma genérica, sino de **anticipar vectores de ataque específicos** mediante Threat Hunting y análisis de inteligencia.

Ejemplo:

- Una empresa del sector sanitario se preocupa especialmente por campañas de ransomware dirigidas a hospitales.  
- Una empresa financiera se preocupará por ataques a banca online, fraude y exfiltración de datos sensibles.

### 2.3. Taxonomía: Seguridad de la Información, Seguridad Informática y Ciberseguridad

En muchos sitios verás estos términos mezclados o usados como sinónimos, pero no lo son.

- **Seguridad de la Información**  
  Es el concepto paraguas. Su objetivo es proteger la Tríada CIA (Confidencialidad, Integridad y Disponibilidad) de los datos en cualquier estado:
  - Físico (papel, documentos en carpetas).  
  - Digital (bases de datos, ficheros).  
  - Conversacional (reuniones, llamadas).

- **Seguridad Informática**  
  Se centra en la protección de los activos físicos y lógicos dentro de un perímetro controlado (CPD, oficinas, servidores on-premise).  
  Se ocupa de:
  - Parcheo de servidores.  
  - Protección de sistemas operativos.  
  - Seguridad física del CPD (accesos, cámaras, control de entrada).

- **Ciberseguridad**  
  Disciplina de la Seguridad de la Información enfocada específicamente en la protección de activos digitales que están interconectados y expuestos a través del ciberespacio.  
  Su dominio natural es:
  - La red.  
  - El tráfico que fluye por ella.  
  - Los servicios expuestos (web, APIs, VPN, etc.).
```bash
**Prompt IA:**  
Explícame con mucho detalle:  
1) Las tres dimensiones de la ciberseguridad (técnica, empresarial y estratégica) con ejemplos prácticos.  
2) La diferencia entre Seguridad de la Información, Seguridad Informática y Ciberseguridad.  
3) Cómo se relacionan estos conceptos en el día a día de una empresa.  
Usa lenguaje claro pero técnico, pensado para alguien que está empezando en ciberseguridad y quiere construir una base sólida.
```
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

#### 3.1.1. Confidencialidad en la práctica

Imagina un portal interno de recursos humanos donde los empleados pueden ver su nómina.  
Para garantizar la confidencialidad:

- Solo el empleado y, quizá, el departamento de RRHH deben poder ver esos datos.  
- El acceso se controla mediante credenciales personales (usuario + contraseña) y, idealmente, MFA.  
- La comunicación entre el navegador y el servidor debe ir cifrada (HTTPS) para que nadie pueda leer los datos “por el camino”.

Cuando se rompe la confidencialidad:

- Se filtran bases de datos de clientes.  
- Se publican contraseñas.  
- Se accede a historias clínicas sin permiso.

Este tipo de incidentes suele tener un fuerte impacto legal y reputacional.

---

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

#### 3.2.1. Integridad en la práctica

En un entorno de logs, preservar la integridad es crítico.  
Si un atacante puede borrar o modificar logs, puede ocultar su rastro.

Por eso se utilizan mecanismos como:

- Envío de logs a un sistema centralizado (SIEM).  
- Firmado de logs o almacenamiento inmutable.  
- Controles de auditoría que registran quién cambia qué y cuándo.

Otro ejemplo:

- Una transferencia bancaria de 1000 € que, en el proceso, se convierte en 10 000 € por manipulación de datos.  
  La integridad se ha roto, aunque los datos sigan siendo “confidenciales”.

---

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

#### 3.3.1. Disponibilidad en la práctica

La disponibilidad tiene una dimensión técnica y otra de negocio:

- Técnica: el sistema está en marcha, responde, no se cuelga.  
- Negocio: el sistema está en marcha cuando es importante que lo esté (horarios, campañas, picos de tráfico).

Un ataque de denegación de servicio (DDoS) puede no robar datos ni modificarlos, pero puede dejar un servicio crítico inutilizable durante horas. Para muchas empresas, eso es tan grave como una brecha de datos.

---

### 3.4. Ejemplo integrador

Piensa en una aplicación bancaria:

- **Confidencialidad:** nadie debe ver el saldo de otra persona.  
- **Integridad:** el saldo, transferencias y movimientos deben ser correctos.  
- **Disponibilidad:** el usuario debe poder acceder al servicio cuando lo necesite.

Cualquier ataque que afecte a la ciberseguridad golpeará, de una forma u otra, a uno o varios de estos tres pilares.

#### 3.4.1. Cómo usar CIA en tu día a día

Cuando analices un incidente, acostúmbrate a preguntarte:

- ¿Qué se ha visto afectado principalmente?
  - ¿Se han filtrado datos (C)?  
  - ¿Se han manipulado datos (I)?  
  - ¿Se ha caído el servicio (D)?

Esta simple clasificación te ayudará a estructurar informes y a priorizar incidentes.
```bash
**Prompt IA:**  
Explícame en profundidad la triada CIA (Confidencialidad, Integridad y Disponibilidad).  
Quiero definiciones claras, ejemplos prácticos de cada pilar en sanidad, banca y ecommerce, explicación de qué controles técnicos protegen cada pilar y ejemplos de incidentes famosos indicando qué pilar(es) se vieron afectados.  
Nivel principiante orientado a alguien que quiere trabajar en un SOC.
```
---

## 4. Activos, amenazas, vulnerabilidades y riesgo

Antes de hablar de ataques, hay que tener clara esta terminología básica. Es el lenguaje que se usa en cualquier análisis, informe o reunión de seguridad.

### 4.1. Activo

Un activo es **algo de valor para la organización**.  
No solo son “ordenadores” o “servidores”, sino cualquier elemento cuyo compromiso pueda afectar al negocio.

Puede ser:

- Un servidor.  
- Una base de datos.  
- Una aplicación web.  
- Credenciales de administrador.  
- Información de clientes.  
- Equipos de usuario (portátiles, móviles).  
- Infraestructura cloud (máquinas virtuales, buckets, bases de datos gestionadas).  
- Incluso una persona clave (conocimiento, acceso, toma de decisiones).

#### 4.1.1. Ejemplos concretos de activos

- En un **ecommerce**:
  - La base de datos de pedidos y clientes.  
  - La pasarela de pago.  
  - El panel de administración.
- En un **hospital**:
  - El sistema de historias clínicas.  
  - Los sistemas de radiología.  
  - La red que conecta equipos médicos.
- En una **pyme**:
  - El servidor de ficheros compartidos.  
  - Las cuentas de correo corporativo.  
  - Las hojas de cálculo con contabilidad.

No todos los activos tienen el mismo peso. Algunos son críticos (si se caen, la empresa “se para”) y otros son de soporte (molesto que fallen, pero no mortal). En ciberseguridad se intenta identificar las **“joyas de la corona”**: aquellos activos cuya pérdida o compromiso tendría un impacto severo.

---

### 4.2. Amenaza

Una amenaza es **cualquier cosa que pueda causar daño a un activo**.  
No tiene por qué ser siempre un “hacker en la oscuridad”. También puede ser algo interno, accidental o incluso natural.

Ejemplos:

- Un atacante externo que escanea puertos buscando vulnerabilidades.  
- Un empleado descontento que copia información confidencial.  
- Un fallo eléctrico que apaga un CPD sin sistemas de respaldo.  
- Un malware que cifra archivos.  
- Un error humano: borrar una carpeta de datos por accidente.

Podemos agrupar las amenazas en:

- **Intencionales (maliciosas)**: cibercriminales, grupos APT, insiders maliciosos.  
- **No intencionales (accidentales)**: errores de configuración, borrado accidental, pérdida de dispositivos.  
- **Ambientales / físicas**: incendio, inundación, corte eléctrico, robo físico de equipos.

---

### 4.3. Vulnerabilidad

Una vulnerabilidad es una **debilidad** que puede ser explotada por una amenaza para causar daño a un activo.

Ejemplos:

- Un sistema sin parches con una vulnerabilidad crítica conocida (CVE).  
- Un usuario con permisos excesivos que no necesita.  
- Una contraseña débil o reutilizada.  
- Un puerto de administración expuesto a Internet.  
- Un bucket S3 o similar sin restricciones de acceso.

Podemos hablar de vulnerabilidades:

- **Técnicas**: bugs de software, servicios sin cifrar, protocolos obsoletos.  
- **De proceso**: falta de revisión de accesos, falta de doble factor para operaciones críticas.  
- **Humanas**: falta de formación, ausencia de concienciación, tendencia a compartir contraseñas.

---

### 4.4. Riesgo

El riesgo es la combinación de:

- La **probabilidad** de que una amenaza explote una vulnerabilidad  
- y el **impacto** que eso tendría sobre el activo.

En términos simples:

> Riesgo = Probabilidad x Impacto

La gestión de ciberseguridad, en la práctica, es una forma de **gestión de riesgos**: no se trata de “eliminar” todos los riesgos (imposible), sino de reducirlos a un nivel aceptable para la organización.

#### 4.4.1. Ejemplo práctico de riesgo

Caso simple:

- Activo: servidor de base de datos de clientes.  
- Amenaza: atacante externo.  
- Vulnerabilidad: el servidor expone un puerto de administración con una vulnerabilidad crítica sin parchear.  
- Impacto: robo de todos los datos de clientes, sanciones legales, pérdida de reputación.

En este escenario, el riesgo es claramente **alto** y debería ser prioridad máxima.

Otro caso:

- Activo: PC de pruebas de laboratorio sin datos reales.  
- Amenaza: malware genérico.  
- Vulnerabilidad: sistema sin parches.  
- Impacto: bajo, porque no hay datos sensibles ni conexión a sistemas críticos.

Aquí el riesgo existe, pero es **medio/bajo**, y la prioridad para mitigarlo puede ser inferior a otros.

---

### 4.5. Cómo se usan estos conceptos en el día a día

En informes, reuniones y playbooks verás constantemente frases como:

- “Riesgo residual alto”.  
- “Exposición de activo crítico”.  
- “Vulnerabilidad explotable de forma remota”.  
- “Amenaza interna no maliciosa (error humano)”.

Cuando empieces a trabajar en un SOC o equipo de ciber, te pedirán que valores casos según este lenguaje. Por eso es importante que lo asimiles desde el principio.
```bash
**Prompt IA:**  
Explícame con mucho detalle la relación entre activo, amenaza, vulnerabilidad y riesgo.  
Incluye definiciones sencillas, ejemplos prácticos para una pequeña empresa y para una gran organización, y un mini-ejercicio con 3 escenarios donde haya que identificar activo, amenaza, vulnerabilidad y riesgo.  
Orientado a alguien junior que quiere aprender a pensar como gestor de riesgos.
```
---

## 5. Tipos de ataques más comunes

No hay que memorizar una lista infinita de ataques, pero sí entender **las familias principales** y qué buscan. Casi todas las noticias de ciberseguridad que leas se pueden encajar en unas pocas categorías bien entendidas.

### 5.1. Phishing

Ataques que intentan engañar al usuario para que:

- Entregue sus credenciales.  
- Haga clic en un enlace malicioso.  
- Abra un adjunto infectado.  
- Apruebe una operación que no debería (transferencias, cambios de cuenta, etc.).

Ejemplo típico:

> Un correo que imita a tu banco y te pide “verificar tu cuenta” a través de un enlace que lleva a una web falsa.

#### 5.1.1. Variantes de phishing

- **Phishing genérico**: misma campaña para miles de personas, sin personalización.  
- **Spear phishing**: mensajes personalizados hacia una persona o grupo concreto (por ejemplo, empleados de finanzas).  
- **Whaling**: objetivo son directivos o altos cargos (“las ballenas”).  
- **Smishing**: a través de SMS.  
- **Vishing**: a través de llamadas de voz.

---

### 5.2. Malware

“Malicious software”: cualquier tipo de software diseñado con intención maliciosa.

Tipos frecuentes:

- **Virus**: se adjuntan a archivos y se propagan cuando estos se ejecutan.  
- **Gusanos (worms)**: se propagan automáticamente a través de redes, sin interacción humana.  
- **Troyanos**: se disfrazan de software legítimo o útil, pero realizan acciones ocultas.  
- **Keyloggers**: registran pulsaciones de teclado (para robar contraseñas, por ejemplo).  
- **Backdoors / RATs**: brindan acceso remoto no autorizado al atacante.

#### 5.2.1. Ejemplo de infección con troyano

Un empleado descarga un programa de “conversión de PDFs” desde una web no oficial.  
El programa funciona, pero incluye un troyano que:

- Se instala en segundo plano.  
- Se comunica con un servidor de comando y control.  
- Permite al atacante ejecutar comandos en el equipo.

Desde ese equipo, el atacante puede intentar moverse a otros sistemas internos de la empresa.

---

### 5.3. Ransomware

Tipo de malware que **cifra los datos** de la víctima y exige un pago (ransom) para recuperarlos.  
En las campañas modernas, además del cifrado, suele haber **robo de datos** (doble extorsión): amenazan con publicar la información si no se paga.

Impacta especialmente en:

- **Disponibilidad**: los sistemas quedan inutilizados.  
- **Integridad**: los ficheros son alterados (cifrados).  
- Posible impacto en **confidencialidad**: si también exfiltran datos.

#### 5.3.1. Fases típicas de un ataque de ransomware moderno

1. Acceso inicial (phishing, RDP expuesto, vulnerabilidad en VPN, etc.).  
2. Descubrimiento interno de la red (qué servidores hay, dónde están los datos).  
3. Movimiento lateral a servidores clave.  
4. Exfiltración de datos a infraestructura del atacante.  
5. Cifrado masivo de sistemas.  
6. Presentación de nota de rescate.

---

### 5.4. Ataques de fuerza bruta

Intentos de adivinar contraseñas probando muchas combinaciones posibles, a menudo de forma automatizada con listas de contraseñas.

Tipos:

- **Cred stuffing**: probar combinaciones usuario/contraseña filtradas de otros servicios.  
- **Brute force clásico**: probar muchas contraseñas sobre una misma cuenta.  
- **Password spraying**: probar pocas contraseñas comunes contra muchas cuentas (para evitar bloqueos por intentos fallidos en una sola cuenta).

Mitigaciones típicas:

- Limitar intentos de login.  
- MFA obligatorio.  
- Políticas de contraseñas robustas.  
- Monitorización de accesos fallidos en SIEM.

---

### 5.5. Ingeniería social

Ataques que se centran en la **psicología** y no tanto en la tecnología.  
Buscan que la víctima:

- Confíe en el atacante.  
- Actúe por miedo o urgencia.  
- No verifique la legitimidad de la petición.

Ejemplos:

- Llamada que se hace pasar por soporte IT pidiendo la contraseña.  
- Mensaje urgente del “CEO” solicitando una transferencia inmediata.  
- Técnico falso que se presenta en la oficina diciendo que viene a “revisar el servidor”.

Muchas brechas graves empiezan con:

> “Solo fue un clic donde no debía” o “solo fue una llamada que parecía legítima”.

---

### 5.6. Cómo se combinan estos ataques en campañas reales

En el mundo real, los ataques raramente se presentan aislados. Lo habitual es una **cadena de ataque** donde se mezclan varios elementos:

- Phishing → robo de credenciales → acceso VPN → despliegue de malware → ransomware.  
- Ingeniería social → acceso a panel de administración → cambios en cuentas bancarias de proveedores.  
- Malware → backdoor silencioso → movimiento lateral → exfiltración de datos.

Como analista, tu trabajo será identificar **piezas del puzzle** en logs y alertas para reconstruir la historia.
```bash
**Prompt IA:**  
Explícame en detalle los principales tipos de ataques comunes: phishing (y variantes), malware, ransomware, fuerza bruta e ingeniería social.  
Para cada uno, indica: objetivo del atacante, cómo suele ejecutarse, qué señales podría ver un usuario normal, qué señales podría ver un analista SOC en logs o alertas y un ejemplo práctico realista.  
Enfoque práctico para Blue Team.
```
---

## 6. Ciclo de vida de un ataque (Kill Chain)

Para entender cómo piensa un atacante, se suele usar el concepto de **Kill Chain**, que describe las fases típicas de un ataque. Es una forma de “contar la historia” del ataque desde el punto de vista del atacante.

Distintas fuentes definen fases ligeramente diferentes, pero una versión simplificada puede ser:

1. Reconocimiento  
2. Preparación / Armamento  
3. Entrega  
4. Explotación  
5. Instalación  
6. Comando y Control (C2)  
7. Acciones sobre el objetivo  

### 6.1. Reconocimiento

El atacante recopila información sobre el objetivo:

- ¿Qué dominios tiene la empresa?  
- ¿Qué servicios tienen expuestos en Internet?  
- ¿Qué tecnologías usan (Apache, IIS, VPN X, etc.)?  
- ¿Qué correos de empleados aparecen en LinkedIn?  
- ¿Qué información sensible hay en fuentes públicas (OSINT)?

Desde el punto de vista defensivo, esta fase suele ser silenciosa, pero se pueden detectar:

- Escaneos masivos de puertos.  
- Peticiones inusuales hacia directorios o endpoints específicos.

---

### 6.2. Preparación / Armamento

El atacante prepara las herramientas que usará:

- Crea un documento malicioso (por ejemplo, un PDF con exploit).  
- Configura un kit de explotación (exploit kit).  
- Desarrolla o configura malware para la campaña.  
- Prepara la infraestructura de C2 (dominios, servidores, proxies).

Para el defensor, esta fase ocurre “fuera” de su entorno; es difícil de ver, pero se puede monitorizar:

- Dominios maliciosos identificados por inteligencia de amenazas.  
- Patrones de infraestructura asociados a grupos concretos.

---

### 6.3. Entrega

Es el momento en el que la amenaza llega al entorno de la víctima:

- Envío de correo con adjunto malicioso.  
- Enlace en un mensaje de phishing.  
- Paquetes maliciosos dirigidos a un servicio expuesto (por ejemplo, a una VPN vulnerable).  
- Descarga de software malicioso desde una web comprometida.

Aquí empezamos a ver señales en:

- Gateways de correo (correos sospechosos, adjuntos bloqueados).  
- Proxies web (descargas extrañas).  
- Firewalls y WAF.

---

### 6.4. Explotación

Se aprovecha una vulnerabilidad para ejecutar código o realizar una acción no autorizada:

- El usuario abre un adjunto y se ejecuta un exploit.  
- El atacante envía una petición específica que explota una vulnerabilidad en una aplicación web.  
- El usuario introduce sus credenciales en una web falsa de phishing.

Como defensor, aquí suelen aparecer:

- Alertas del EDR (comportamiento sospechoso en un proceso).  
- Logs de errores o patrones extraños en aplicaciones web.

---

### 6.5. Instalación

El atacante instala algo que le permita **mantener acceso** al sistema:

- Malware que se ejecuta al iniciar el sistema.  
- Creación de un usuario local oculto.  
- Tareas programadas o servicios persistentes.  
- Modificación de claves de registro en Windows.

Es la fase en la que la intrusión se convierte en “presencia persistente”.  
En logs y herramientas verás:

- Nuevos servicios.  
- Nuevas tareas programadas.  
- Creación de cuentas.  
- Procesos que vuelven a aparecer tras reinicio.

---

### 6.6. Comando y Control (C2)

La máquina comprometida se comunica con el atacante:

- Establece conexiones salientes a una IP o dominio controlado por el atacante.  
- Usa canales cifrados o túneles (HTTPS, DNS tunneling, etc.).  
- Puede utilizar servicios legítimos (por ejemplo, plataformas cloud) para camuflar tráfico.

A nivel SOC, se pueden detectar:

- Conexiones salientes a dominios poco habituales.  
- Patrones de tráfico constantes a destinos desconocidos.  
- Tráfico cifrado hacia ubicaciones no esperadas.

---

### 6.7. Acciones sobre el objetivo

Una vez consolidado el acceso, el atacante ejecuta su objetivo real:

- Robar información y exfiltrarla.  
- Cifrar sistemas (ransomware).  
- Destruir datos.  
- Utilizar la infraestructura para atacar a otros (botnet, spam, etc.).

En esta fase se ve el impacto más claro en negocio:

- Sistemas que se caen.  
- Datos cifrados.  
- Fugas de información.

---

### 6.8. Kill Chain desde la perspectiva del SOC

La Kill Chain es una herramienta mental muy útil para un analista:

- Le ayuda a **clasificar eventos** según la fase del ataque.  
- Permite ver dónde se han tenido controles efectivos (por ejemplo, se detectó en fase de entrega).  
- Permite identificar dónde faltan controles (por ejemplo, no hay visibilidad en C2).

Cuando redactes informes, podrás indicar:

> “El ataque fue detectado en fase de entrega (Kill Chain) gracias al filtro de correo. No llegó a fase de explotación”.
```bash
**Prompt IA:**  
Explícame el ciclo de vida de un ataque (Kill Chain) paso a paso.  
Quiero descripción clara de cada fase, ejemplo de un ataque completo (por ejemplo ransomware) que recorra todas las fases e indicadores que podría ver un Blue Team en cada fase (logs, alertas, comportamientos anómalos).  
Nivel principiante, muy práctico para SOC.
```
---

## 7. Introducción a MITRE ATT&CK

**MITRE ATT&CK** es un marco (framework) que describe las **tácticas y técnicas** que usan los atacantes en el mundo real.  
Se ha convertido en un lenguaje estándar en la industria.

- **Tácticas**: el objetivo de alto nivel del atacante (por ejemplo, Persistencia, Ejecución, Exfiltración).  
- **Técnicas**: la forma concreta de lograr ese objetivo (por ejemplo, uso de PowerShell, ejecución de scripts, inyección de procesos).

Por ejemplo:

- Táctica: **Ejecución** (el atacante quiere ejecutar código en un sistema).  
- Técnica: **PowerShell** (T1059.001): utiliza scripts de PowerShell para ejecutar comandos.

MITRE ATT&CK se organiza en matrices según el entorno:

- Enterprise (Windows, Linux, macOS, AD, etc.).  
- Mobile.  
- ICS (entornos industriales).

### 7.1. Cómo se ve MITRE ATT&CK en la práctica

Cada técnica MITRE tiene:

- Un identificador único (ej. T1059).  
- Una descripción de qué hace y por qué.  
- Ejemplos de uso por parte de grupos reales de amenazas.  
- Posibles mitigaciones.  
- Ideas de detección.

Ejemplo simplificado:

- Técnica: T1059 – Command and Scripting Interpreter.  
- Subtécnica: T1059.001 – PowerShell.  
- Uso típico: ejecución de scripts para descargar payloads, desactivar defensas, moverse lateralmente.

Un analista puede decir:

> “Hemos detectado uso anómalo de PowerShell que encaja con la técnica T1059.001 (MITRE ATT&CK)”.

Eso permite que otros analistas o equipos entiendan rápidamente de qué tipo de comportamiento estamos hablando.

---

### 7.2. ¿Por qué es útil para un SOC junior?

Aunque al principio parezca mucha información, MITRE ATT&CK te ayuda a:

- Poner “etiqueta” a lo que ves en los logs.  
- Entender que no estás solo: hay un marco reconocible para describir ataques.  
- Alinear tu detección con técnicas concretas (por ejemplo, “queremos mejorar detecciones de T1059 y T1055”).

En la práctica, muchos SIEM/EDR ya vienen con reglas mapeadas a MITRE:

- Una alerta puede indicar directamente: “posible T1059.001”.  
- Un informe de incidente suele incluir una lista de técnicas MITRE detectadas.
```bash
**Prompt IA:**  
Explícame qué es MITRE ATT&CK como si fuera una guía para alguien que empieza.  
Incluye qué son tácticas y técnicas, cómo se organiza la matriz Enterprise ATT&CK, cómo lo usaría un analista SOC en su día a día (investigar, etiquetar y mejorar detecciones) y un ejemplo práctico con una técnica concreta como T1059.  
Nivel principiante con curiosidad por threat hunting.
```
---

## 8. Roles en ciberseguridad: dónde encaja todo esto

La ciberseguridad no es un único trabajo. Es un ecosistema de roles que se complementan. Entenderlos te ayuda a decidir hacia dónde quieres orientar tu carrera.

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

#### 8.1.1. Día típico de un analista SOC junior

- Revisión de las alertas pendientes desde el turno anterior.  
- Clasificación: qué es ruido, qué es relevante.  
- Investigación de eventos:
  - ¿Esta IP es maliciosa?  
  - ¿Este proceso es normal en este equipo?  
  - ¿Este usuario suele conectarse desde este país?
- Escalado:
  - Si parece algo serio, se pasa a niveles superiores o a un equipo de respuesta a incidentes.
- Documentación en la herramienta:
  - Resumen del caso.  
  - Acciones realizadas.  
  - Recomendaciones.

---

### 8.2. Pentester / Hacking Ético (Red Team orientado a test)

- Realiza auditorías de seguridad acordadas con el cliente.  
- Busca vulnerabilidades en sistemas, aplicaciones y redes.  
- Intenta explotarlas de forma controlada (sin dañar el negocio).  
- Documenta hallazgos y propone soluciones.

Aunque muchas personas se sienten atraídas primero por el pentesting, es importante tener:

- Fundamentos sólidos (como los de esta semana).  
- Conocimientos de redes, sistemas, aplicaciones web.  
- Mentalidad estructurada (metodologías de test).

---

### 8.3. Ingeniero de Seguridad

- Diseña arquitecturas seguras (on-prem, cloud, híbrido).  
- Define y configura controles técnicos (firewalls, WAF, proxies, EDR, etc.).  
- Automatiza tareas de seguridad (scripts, playbooks de SOAR).  
- Trabaja muy cerca de equipos de IT, redes y desarrollo.

Este rol se apoya mucho en:

- Conocimientos de infraestructura.  
- Buen entendimiento de flujos de datos.  
- Capacidad de traducir requisitos de negocio en controles técnicos.

---

### 8.4. Purple Team (Colaboración Red + Blue)

- Mezcla mentalidad ofensiva (Red Team) y defensiva (Blue Team).  
- Realiza ejercicios donde:
  - Red Team ejecuta técnicas específicas.  
  - Blue Team intenta detectarlas y responder.
- Ajusta y mejora reglas de detección basadas en esos ejercicios.

El objetivo del Purple Team no es “ganar” sino **mejorar la defensa**: si algo no se detectó, se diseña cómo debería detectarse.

---

### 8.5. Otros roles que verás más adelante

Aunque esta semana no profundizamos, deberías saber que existen otros perfiles:

- Threat Intelligence (análisis de amenazas y actores).  
- DFIR (Digital Forensics and Incident Response).  
- AppSec (seguridad de aplicaciones).  
- Cloud Security Engineer.  
- GRC (gobierno, riesgo y cumplimiento).

Todos ellos comparten los fundamentos que estás viendo ahora.
```bash
**Prompt IA:**  
Explícame con detalle los roles de Analista SOC, Pentester, Ingeniero de Seguridad y Purple Team.  
Para cada rol, indica: qué tareas hace en el día a día, qué conocimientos técnicos necesita, qué tipo de herramientas suele usar y un ejemplo de proyecto o situación real.  
Nivel principiante que quiere decidir hacia qué rol orientarse.
```
---

## 9. Ciclo de gestión de incidentes

Cuando ocurre algo sospechoso en una empresa, no basta con “apagar el servidor y ya está”. Hay procedimientos formales para gestionar incidentes de seguridad.  
Muchos se basan en el modelo de NIST, que incluye:

1. Preparación  
2. Detección y análisis  
3. Contención  
4. Erradicación  
5. Recuperación  
6. Lecciones aprendidas  

Además, en entornos profesionales se utilizan **playbooks**: guías paso a paso sobre cómo actuar en determinados tipos de incidentes.

### 9.1. Preparación

En esta fase se construyen las capacidades necesarias para poder responder cuando algo ocurra:

- Formación del equipo (SOC, IT, responsables).  
- Implementación de herramientas (SIEM, EDR, sistemas de tickets).  
- Definición de procedimientos y responsabilidades:
  - ¿Quién decide aislar un servidor?  
  - ¿Quién habla con el cliente?  
  - ¿Quién coordina con legal o comunicación?
- Copias de seguridad probadas (no basta con “tener backups”; hay que validar que se pueden restaurar).  
- Definición de **playbooks**:
  - Plantillas de actuación para incidentes típicos (ransomware, phishing masivo, compromiso de cuenta, etc.).

Un **playbook** suele incluir:

- Desencadenante (qué tipo de alerta lo activa).  
- Pasos de análisis inicial.  
- Decisiones a tomar según los hallazgos.  
- Acciones de contención recomendadas.  
- Pasos de escalado (a quién y cuándo escalar).  
- Modelo de informe para el cierre.

---

### 9.2. Detección y análisis

Aquí es donde entra con fuerza el trabajo del SOC:

- Se recibe una alerta:
  - Desde el SIEM (correlación de eventos).  
  - Desde el EDR/XDR.  
  - Desde un firewall, WAF, IDS.  
  - Desde un usuario (sospecha de phishing, comportamiento extraño).
- Se analiza la alerta:
  - ¿Es un falso positivo?  
  - ¿Es algo ya conocido o algo nuevo?  
  - ¿Cuál es el alcance (cuántos equipos, usuarios, sistemas)?
- Se clasifica el incidente:
  - Criticidad (alta, media, baja).  
  - Tipo (ransomware, compromiso de cuenta, exfiltración, etc.).  
  - Relación con campañas conocidas o actores de amenaza.

En esta fase el analista utiliza:

- Playbooks de análisis: listas de comprobación (checklists) que indican qué revisar, dónde buscar, qué evidencias recopilar.  
- Herramientas de consulta (OSINT, reputación de IPs y dominios).  
- Logs de sistemas concretos (servidores, firewalls, etc.).

---

### 9.3. Contención

Una vez confirmado el incidente, el siguiente paso es **frenar el impacto**:

- Aislar equipos comprometidos de la red.  
- Revocar sesiones activas de usuarios afectados.  
- Bloquear IPs o dominios maliciosos en firewall/proxy.  
- Deshabilitar cuentas comprometidas.

Aquí hay que encontrar un equilibrio:

- Si contienes demasiado pronto, puedes perder evidencias o alertar al atacante.  
- Si tardas demasiado, el daño puede ser mayor (más máquinas cifradas, más datos exfiltrados).

Los **playbooks de contención** ayudan a:

- Establecer criterios claros (“si se confirma X, entonces aislar Y”).  
- Coordinar acciones entre SOC, IT y negocio.  
- Evitar improvisar en momentos de estrés.

---

### 9.4. Erradicación

En esta fase se busca eliminar por completo la amenaza del entorno:

- Borrar malware y backdoors.  
- Cerrar la vulnerabilidad explotada (parches, cambios de configuración).  
- Revocar y regenerar credenciales comprometidas (usuarios, claves API, certificados).  
- Verificar que no quedan mecanismos de persistencia activos.

En muchos casos, se combina:

- Análisis forense técnico.  
- Consultas profundas en logs históricos.  
- Barridos adicionales para asegurarse de que no quedan “restos”.

---

### 9.5. Recuperación

Una vez erradicada la amenaza, hay que restaurar el entorno a un estado funcional y seguro:

- Restaurar sistemas a partir de backups limpios.  
- Reincorporar equipos a la red de forma controlada.  
- Monitorizar de forma intensiva durante un periodo para detectar posibles rebrotes.

Desde el punto de vista del negocio, esta fase está muy ligada a:

- Volver a operar.  
- Cumplir SLAs de disponibilidad.  
- Comunicar a clientes y usuarios que el servicio se ha restablecido.

---

### 9.6. Lecciones aprendidas

Es una fase que muchas organizaciones descuidan, pero es clave para madurar:

- Se revisa:
  - ¿Cómo empezó el incidente?  
  - ¿Podríamos haberlo detectado antes?  
  - ¿Fueron efectivos los playbooks?  
  - ¿Qué falló a nivel técnico, de proceso o humano?
- Se decide:
  - Mejorar reglas de detección.  
  - Ajustar playbooks.  
  - Cambiar procesos.  
  - Reforzar formación.

Es aquí donde el incidente se convierte en una **oportunidad de mejora**.  
Un buen SOC documenta estos aprendizajes y, poco a poco, se vuelve más eficaz.

---

### 9.7. Relación entre ciclo de incidentes y playbooks en el día a día

En un SOC real, no se empieza de cero cada vez que llega una alerta.  
Para los incidentes más habituales, se diseñan playbooks específicos, por ejemplo:

- **Playbook de phishing**:
  - Verificar cabeceras del correo.  
  - Comprobar URLs.  
  - Revisar si otros usuarios han recibido el mismo mensaje.  
  - Bloquear remitente/dominio si procede.  
  - Educar al usuario afectado.

- **Playbook de posible ransomware**:
  - Verificar tipo de alerta (EDR/SIEM).  
  - Aislar host sospechoso.  
  - Comprobar otros equipos con indicadores similares.  
  - Coordinar con IT para backups y recuperación.  
  - Escalar a nivel de incidente mayor si hay más de X equipos afectados.

Estos playbooks se apoyan directamente en el ciclo de gestión de incidentes, pero traducidos a pasos concretos para el analista.
```bash
**Prompt IA:**  
Explícame el ciclo de gestión de incidentes de seguridad según el modelo de NIST.  
Quiero descripción clara de cada fase, un ejemplo práctico de un incidente (por ejemplo, ransomware en un servidor) recorriendo todas las fases, qué rol tendría un analista SOC en cada fase y qué es un playbook de respuesta a incidentes y cómo se usa en un SOC.  
Nivel principiante orientado a alguien que trabajará en un SOC.
```
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

    sudo tail -n 50 /var/log/auth.log

Observa:

- Cómo queda registrado tu intento de conexión.  
- Qué campos aparecen (usuario, IP de origen, hora, etc.).

No es necesario entender cada línea, pero sí ver que:

- Tus acciones dejan rastro.  
- Esos rastros son la materia prima de un SOC.
```bash
**Prompt IA:**  
Actúa como instructor de laboratorio de ciberseguridad.  
Ayúdame a:  
- Identificar activos, amenazas, vulnerabilidades y riesgos en un laboratorio con máquinas Kali, Windows y Ubuntu.  
- Proponer ejemplos concretos para el documento `Semana1_CIA_Ejemplos.md`.  
- Explicar qué debo fijarme al revisar `/var/log/auth.log` por primera vez.  
Respuesta muy detallada, orientada a alguien que nunca ha trabajado con logs.
```
---

### 11. Entregables de la Semana 1

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
```bash
**Prompt IA:**  
Ayúdame a redactar el contenido de los siguientes archivos de la Semana 1:  
1) `Semana1_Resumen_Conceptos.md`: resumen de ciberseguridad, CIA, diferencia entre amenaza/vulnerabilidad/riesgo, Kill Chain y MITRE ATT&CK.  
2) `Semana1_CIA_Ejemplos.md`: ejemplos de confidencialidad, integridad y disponibilidad aplicados a mi laboratorio (máquinas Kali, Windows y Ubuntu).  
3) Sugerencias de contenido para la sección `Activos y Riesgos` de `Lab_Setup_Report.md`.  
Quiero textos en lenguaje claro pero técnico, pensados para alguien que está empezando.
```
---

### 12. Checklist de cierre de la Semana 1

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
```bash
**Prompt IA:**  
Actúa como evaluador de mi Semana 1 de fundamentos de ciberseguridad.  
1) Hazme entre 8 y 12 preguntas de repaso sobre: definición de ciberseguridad, CIA, activo/amenaza/vulnerabilidad/riesgo, tipos de ataque, Kill Chain, MITRE ATT&CK, roles y gestión de incidentes.  
2) Evalúa mis respuestas (que escribiré yo después) indicando si son correctas o incompletas y qué conceptos debería reforzar.  
Prepáralo como si fuera un pequeño examen de cierre de la Semana 1.
```

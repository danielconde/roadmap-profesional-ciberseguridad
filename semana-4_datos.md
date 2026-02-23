# Semana 4 – Datos, Almacenamiento e Identidad 💾🧑‍💻

## 0. Introducción a la semana

En las semanas anteriores has construido una base muy sólida:

- Semana 0 → Laboratorio montado (Kali, Windows, Ubuntu, red interna, algo de cloud).
- Semana 1 → Fundamentos de ciberseguridad (CIA, amenazas, riesgo, Kill Chain, MITRE, roles).
- Semana 2 → Redes en profundidad (IP, puertos, protocolos, nmap, Wireshark).
- Semana 3 → Linux y Windows orientados a seguridad (procesos, servicios, logs, estructura del SO).

Ahora vamos a entrar en un eje crítico que suele estar en el centro de casi todos los incidentes:  
**los datos** y **la identidad de los usuarios que acceden a esos datos**.

En la práctica, la mayoría de las empresas no se preocupan tanto por "el servidor en sí" sino por:

- Los **datos** que contiene (bases de datos, ficheros, documentos, correos).
- Quién **puede acceder** a esos datos y en qué condiciones.
- Qué pasa si esos datos se pierden, se cifran o se filtran.

Aquí aparece un punto clave para un analista de seguridad: los incidentes rara vez son "técnicos por deporte". Son incidentes porque impactan en algo valioso. Y en casi todas las organizaciones, lo valioso es:

- Información sensible (clientes, nóminas, contratos, propiedad intelectual).
- Servicios críticos (ERP, correo, ficheros compartidos, bases de datos).
- Identidades (cuentas que permiten acceder a todo lo anterior).

En esta Semana 4 vamos a tratar dos temas muy conectados:

1. **Almacenamiento y copias de seguridad** (y su relación con ransomware).
2. **Identidad y acceso** (IAM, cuentas, permisos, rol de Active Directory).

Vas a ver que muchas investigaciones de SOC terminan siendo preguntas como:

- "Qué usuario accedió a qué archivo"
- "Con qué permisos"
- "Dónde estaba almacenado"
- "Había backup"
- "Cuándo fue el último punto de recuperación"

**Prompt IA:**  
"Actúa como instructor de ciberseguridad orientado a Blue Team. Introduce el tema de "datos e identidad" como núcleo de incidentes. Explica con ejemplos reales por qué ransomware, exfiltración y abuso de credenciales giran alrededor de almacenamiento y permisos. Dame 10 preguntas que un analista SOC debería hacerse cuando un cliente reporta 'nos han cifrado archivos' o 'creemos que han accedido a documentos sensibles'."

---

## 1. Objetivos de aprendizaje de la Semana 4

Al finalizar esta semana deberías ser capaz de:

1. Explicar los diferentes tipos de almacenamiento (local, NAS, SAN, nube) y en qué contextos se usan.
2. Entender qué son los sistemas de archivos (NTFS, ext4) y por qué los permisos son tan importantes.
3. Comprender qué son las copias de seguridad, snapshots, RPO y RTO, y cómo se relacionan con incidentes de ransomware.
4. Describir a alto nivel cómo funciona un recurso compartido de red (network share) y qué problemas aparecen cuando se configura mal.
5. Entender la idea de **identidad digital** en una organización: usuarios, grupos, roles.
6. Conectar estos conceptos con Active Directory e IAM (on-premise y cloud) de forma comprensible.
7. Realizar pequeños laboratorios para:
   - Crear y proteger datos en tus VMs.
   - Simular pérdida de datos y "recuperación".
   - Jugar con permisos básicos.
8. Documentar tus conclusiones de forma clara para tu repositorio.

Una lectura práctica de estos objetivos es: aprender a responder "quién puede acceder a qué" y "qué pasa si lo perdemos". Estas dos preguntas determinan:

- Si un ataque puede escalar.
- Cuánto duele el impacto.
- Cuánto tarda la recuperación.
- Qué medidas preventivas tienen sentido.

**Prompt IA:**  
"Actúa como evaluador de la Semana 4. Crea 12 preguntas de repaso basadas en los 8 objetivos. Para cada pregunta incluye: respuesta esperada, explicación breve, y un error típico de principiante. Añade 3 casos prácticos tipo 'mini incidente' que obliguen a aplicar RPO, RTO, permisos y recurso compartido."

---

## 2. Tipos de almacenamiento: local, NAS, SAN y nube

En una empresa no todo está "en el disco C:". Existen diferentes modelos de almacenamiento, cada uno con sus ventajas, riesgos y uso típico.

Un punto importante: el tipo de almacenamiento cambia el "radio de impacto" de un incidente. Si un ransomware cifra un disco local, afecta a una máquina. Si cifra un NAS con permisos amplios, puede afectar a media empresa.

### 2.1. Almacenamiento local

Es el más sencillo: el disco interno de una máquina.

Ejemplos:

- El SSD de tu portátil.
- El disco del servidor Windows donde se guardan ficheros.
- El disco virtual de una VM en tu laboratorio.

Ventajas:

- Rápido.
- Fácil de gestionar en entornos pequeños.
- Menos dependencias externas: si la red se cae, el disco local sigue disponible.

Inconvenientes:

- Si el equipo se rompe, puedes perder los datos.
- Es difícil de compartir de forma centralizada si tienes muchos usuarios.
- Las copias de seguridad dependen de que alguien las configure adecuadamente.
- Puede convertirse en un "silo" invisible: datos importantes que nadie sabía que existían, sin backup.

Desde ciberseguridad, es fácil olvidarse de estos discos "locales" y que se conviertan en puntos de pérdida de datos. En incidentes reales, esto se ve cuando:

- Un usuario guarda datos críticos en su escritorio o documentos.
- La organización cree que "todo está en el servidor".
- Y el día del incidente se descubre que no.

### 2.2. NAS (Network Attached Storage)

Un NAS es un dispositivo de almacenamiento conectado a la red, que ofrece **recursos compartidos** (carpetas de red) a varios usuarios o servidores.

Características típicas:

- Se accede a través de protocolos como SMB (Windows) o NFS (Linux).
- Suele tener varias bahías de discos en RAID.
- Puede incluir servicios adicionales (copias de seguridad, snapshots, replicación, antivirus).
- Suelen existir carpetas por departamentos, proyectos o perfiles de usuario.

Ventajas:

- Centraliza archivos de usuarios.
- Facilita aplicar políticas de permisos en red.
- Facilita recuperar archivos eliminados si tiene snapshots.
- Es común que permita cuotas, versiones o papelera de reciclaje.

Riesgos:

- Si se configura con permisos demasiado amplios, un ransomware puede cifrar enormes volúmenes de datos.
- Si el NAS no está bien securizado, puede ser accesible desde fuera de la red interna.
- Si las credenciales de un usuario con mucha escritura se comprometen, el atacante hereda ese alcance.
- Un NAS sin segmentación ni control de acceso puede convertirse en un "single point of failure".

Concepto clave para SOC: el NAS suele ser el "impacto visible". Cuando un usuario dice "se han cifrado los ficheros del departamento", muchas veces se refiere a shares en NAS o servidores de ficheros.

### 2.3. SAN (Storage Area Network)

Una SAN es un sistema de almacenamiento de alto rendimiento utilizado sobre todo en entornos de:

- Servidores virtualizados.
- Bases de datos críticas.
- Grandes centros de datos.

Se basa en tecnologías como:

- Fibre Channel.
- iSCSI.

Su gestión es más compleja y suele estar en manos de equipos de infraestructura especializados.

Desde la perspectiva de un junior de ciberseguridad, lo importante es entender:

- Es almacenamiento compartido de alto rendimiento para servidores.
- Si se cae o se daña, puede afectar a muchos servicios críticos a la vez.
- Su compromiso o indisponibilidad puede tumbar múltiples máquinas virtuales, y por tanto múltiples aplicaciones.

En incidentes, la SAN aparece cuando hablamos de "la infraestructura compartida que sostiene todo". No siempre es el primer objetivo del atacante, pero sí puede ser un amplificador de impacto si algo la afecta.

### 2.4. Almacenamiento en la nube

Servicios como:

- Azure Blob Storage.
- Amazon S3.
- Google Cloud Storage.

Permiten guardar objetos (ficheros, backups, logs) en la nube.

Ventajas:

- Alta disponibilidad.
- Escalabilidad.
- Integración con otros servicios cloud.
- Opciones de cifrado, control de acceso y auditoría, si se configuran bien.
- Posibilidad de versionado y retención para reducir impacto de borrado o cifrado.

Riesgos:

- Buckets o contenedores mal configurados (públicos sin querer).
- Claves de acceso expuestas.
- Falta de cifrado en datos sensibles.
- Permisos excesivos en identidades cloud.
- Falta de logging o de alertas ante accesos anómalos.

Aquí vuelve a aparecer el concepto de **responsabilidad compartida**: el proveedor te da la herramienta, pero tú decides si la usas de forma segura o no.

**Prompt IA:**  
"Actúa como arquitecto de seguridad. Explica local, NAS, SAN y cloud storage con ejemplos de uso típicos. Para cada tipo: 5 riesgos de seguridad comunes, 5 controles recomendados y 3 señales de alerta que podría ver un SOC (por ejemplo, patrones de acceso, cambios de permisos, borrados masivos, cifrados)."

---

## 3. Sistemas de archivos y permisos: NTFS y ext4

Los sistemas de archivos no solo sirven para guardar datos. También definen:

- Cómo se organizan.
- Qué permisos aplican.
- Qué metadatos existen (propietario, timestamps).
- Qué capacidades adicionales pueden ayudar o complicar un incidente (auditoría, cifrado, ACLs).

### 3.1. NTFS (Windows)

En Windows, el sistema de archivos más común es NTFS.

Sus características relevantes para seguridad incluyen:

- Permisos a nivel de archivo y carpeta (ACL – Access Control Lists).
- Posibilidad de cifrado (EFS – Encrypting File System).
- Soporte para auditoría (registro de accesos a ficheros si se configura).
- Herencia de permisos (una carpeta padre puede pasar permisos a subcarpetas).
- Diferenciación entre permisos de share (red) y permisos NTFS (disco).

A alto nivel, puedes:

- Asignar permisos a usuarios y grupos.
- Establecer permisos heredados.
- Restringir escritura a grupos concretos.
- Auditar accesos a rutas críticas, si se habilita.

Problema típico:

- "Todos tienen permiso de escritura en una carpeta compartida crítica".
- Si entra un ransomware, cifrará todo lo que pueda escribir.

También hay un problema de "complejidad": en Windows pueden coexistir:

- Permisos de recurso compartido (SMB).
- Permisos NTFS (ACL).
- Y el efectivo es el resultado de combinar ambos.

Esto a veces crea escenarios confusos donde se cree que una carpeta está protegida, pero en realidad un grupo tiene permisos heredados que no se habían revisado.

### 3.2. ext4 (Linux)

En muchas distribuciones Linux modernas se utiliza ext4.

Se diferencia de NTFS, pero desde la perspectiva de ciberseguridad junior:

- Lo que más te interesa es comprender cómo funcionan los permisos clásicos de Linux (usuario, grupo, otros).
- Puedes combinarlos con ACLs extendidas en sistemas más avanzados.
- Los permisos mal configurados pueden habilitar modificación o ejecución no autorizada.

Ejemplo:

Un directorio "/compartido" con permisos demasiado abiertos ("777") es una invitación a:

- Modificación no autorizada.
- Eliminación de ficheros.
- Persistencia o escalada si hay scripts que se ejecutan desde rutas escribibles.

El aprendizaje importante es que permisos no son un detalle administrativo. Son la frontera entre:

- "un usuario puede leer"
- "un usuario puede modificar"
- "un usuario puede ejecutar"
- "un usuario puede tomar control"

**Prompt IA:**  
"Actúa como instructor de sistemas. Explica NTFS y ext4 con foco en permisos y seguridad. Incluye: qué es una ACL, qué es herencia, qué es permiso efectivo, y ejemplos de malas configuraciones típicas. Luego dame 10 reglas prácticas de 'mínimo privilegio' aplicadas a carpetas compartidas y directorios en Linux."

---

## 4. Copias de seguridad, snapshots, RPO y RTO

En muchos incidentes, especialmente con ransomware, la diferencia entre:

- **Un susto serio**
- y **una catástrofe**

es la existencia (o no) de **buenas copias de seguridad** y de una estrategia de recuperación bien diseñada.

Una idea clave: backup no es solo "tener una copia". Es poder restaurar:

- En tiempo razonable (RTO).
- Con pérdida aceptable (RPO).
- Sin que el atacante también haya destruido o cifrado el backup.

### 4.1. Copias de seguridad (backups)

Una copia de seguridad es una copia de los datos (y a veces de sistemas completos) con el fin de poder recuperarlos en caso de:

- Borrado accidental.
- Fallo de hardware.
- Ataque (ransomware, borrado malicioso).
- Corrupción de datos.

Tipos:

- Copias completas.
- Incrementales.
- Diferenciales.
- Offline (desconectadas de la red) vs online.

Buenas prácticas que verás mucho en entornos profesionales:

- Separación de credenciales: que el atacante no pueda borrar backups con las mismas cuentas.
- Retención e inmutabilidad: copias que no se pueden modificar durante un tiempo.
- Pruebas de restauración: si nunca se prueba, no hay certeza de que funcione.
- Copias en diferente ubicación: otro sistema, otra red, o cloud con controles robustos.

En incidentes reales, el problema típico no es "no hay backup", sino:

- "hay backup, pero está incompleto"
- "hay backup, pero está cifrado también"
- "hay backup, pero restaurar tarda días"
- "hay backup, pero nadie sabe cómo restaurar bien"

### 4.2. Snapshots

Los snapshots son "fotos" del estado de un sistema en un momento concreto.

Pueden existir:

- A nivel de máquina virtual (como en tu laboratorio).
- A nivel de almacenamiento (snapshots en NAS, SAN o cloud).
- A nivel de sistema de archivos.

Ventajas:

- Son rápidos de crear y restaurar.
- Permiten volver atrás tras una actualización o cambio arriesgado.
- Son muy útiles para recuperación rápida ante errores operativos.

Limitaciones:

- No sustituyen siempre a un backup externo.
- Si el atacante obtiene control del sistema de snapshots, puede borrarlos.
- Si se almacenan en el mismo hardware, no protegen de desastres físicos.
- Algunos ransomware intentan específicamente localizar y destruir snapshots.

Por eso, snapshots suelen ser "primera línea" para recuperación rápida, pero no la única defensa.

### 4.3. RPO (Recovery Point Objective)

El RPO responde a:

> "Cuánta pérdida de datos, en tiempo, es aceptable"

Ejemplos:

- RPO de 24 horas → como máximo perderemos los cambios del último día.
- RPO de 1 hora → se aceptan pérdidas de hasta una hora de trabajo.

Si tienes copias de seguridad una vez al día, el RPO es aproximadamente 24 horas.

RPO no es teoría: es una decisión de negocio. Define cuánto trabajo se puede perder. En departamentos como finanzas o logística, un RPO grande puede ser inaceptable.

### 4.4. RTO (Recovery Time Objective)

El RTO responde a:

> "Cuánto tiempo puede estar un servicio caído antes de que sea inaceptable"

Ejemplos:

- RTO de 8 horas → el servicio debe estar restaurado antes de 8 horas.
- RTO de 1 hora → recuperación muy rápida, requiere alta inversión en redundancia.

En un incidente de ransomware, se mira:

- Hasta qué punto podemos recuperar datos (RPO).
- Cuánto tardaremos (RTO).

Si no hay backups o están cifrados, el impacto puede ser devastador.

Un punto práctico: RTO depende de recursos y procesos:

- Disponibilidad de personal para restaurar.
- Documentación y runbooks.
- Ancho de banda y tiempos de copia.
- Prioridad de servicios: no todo se restaura a la vez.

**Prompt IA:**  
"Actúa como consultor de continuidad de negocio. Explica backups, snapshots, RPO y RTO con ejemplos concretos y casos de ransomware. Incluye un ejemplo numérico sencillo: una empresa con backups diarios y semanales, qué RPO y RTO implican. Luego propone un plan de mejora en 3 niveles (básico, intermedio, avanzado) con controles y prioridades."

---

## 5. Identidad digital, permisos y recursos compartidos

Hasta ahora hemos hablado de datos, pero los datos siempre se acceden mediante **identidades**:

- Cuentas de usuario.
- Cuentas de servicio.
- Grupos.
- Roles.

Un mensaje importante: en incidentes modernos, muchas veces no "rompen" la puerta, usan llaves robadas. Si controlan identidad, controlan acceso.

### 5.1. Identidad en entornos Windows y Linux

En tu laboratorio ya tienes:

- Usuarios de Linux (en Ubuntu y Kali).
- Usuarios de Windows.

A nivel conceptual:

- Una **identidad** es alguien (o algo) que puede autenticarse (demostrar quién es).
- Una vez autenticado, el sistema decide **qué puede hacer** (autorización) en función de:
  - A qué grupo pertenece.
  - Qué permisos tienen los recursos.

Conceptos que conviene tener claros:

- Autenticación: "quién eres".
- Autorización: "qué puedes hacer".
- Principio de mínimo privilegio: solo los permisos necesarios, ni uno más.
- Separación de funciones: no usar la misma identidad para tareas normales y administrativas.

En investigación, la identidad suele ser la pieza que conecta eventos:

- Login del usuario.
- Acceso a share.
- Modificación masiva de archivos.
- Conexión a un servidor remoto.
- Creación de persistencia.

### 5.2. Recursos compartidos (network shares)

Un recurso compartido típico:

- Carpeta de red en un servidor Windows o NAS.
- Accesible para varios usuarios ("Recursos humanos", "Finanzas", "Proyectos").

Relación con seguridad:

- Si la carpeta está compartida con "Todos: Control total", cualquiera puede ver y modificar.
- Un usuario comprometido puede actuar en nombre del departamento.
- Un ransomware ejecutado desde un usuario con acceso de escritura cifrará todos esos ficheros.

Por eso se intenta:

- Definir permisos por grupos (departamento, rol).
- Evitar permisos excesivos ("Everyone" o "Todos" con escritura).
- Implementar separación de funciones.
- Registrar accesos a carpetas críticas en entornos donde sea viable.

Un patrón clásico de incidente:

- Cuenta de usuario comprometida.
- Acceso a shares con escritura amplia.
- Cifrado masivo.
- Impacto rápido y visible.

Esto es uno de los motivos por los que se insiste tanto en permisos bien diseñados: son una barrera directa frente al radio de daño.

**Prompt IA:**  
"Actúa como analista SOC. Explícame cómo una identidad comprometida puede provocar un incidente de cifrado o exfiltración en recursos compartidos. Dame una checklist de 12 puntos para revisar permisos y accesos en una carpeta compartida, y 8 señales en logs que indicarían abuso de un share."

---

## 6. Active Directory, IAM y visión Cloud (alto nivel)

### 6.1. Active Directory (AD)

Ya has visto una primera mención de AD en la Semana 3.

Recuerda:

- Es un servicio de directorio.
- Gestiona usuarios, grupos, equipos y políticas en entornos Windows de empresa.
- Permite autenticación centralizada.

En AD:

- Un usuario inicia sesión en su PC con credenciales de dominio.
- Sus permisos en recursos compartidos dependen de los grupos AD a los que pertenece.
- Las políticas de seguridad (GPO) se aplican de forma centralizada (por ejemplo: complejidad de contraseñas, bloqueo de USB).

Desde ciberseguridad:

- AD es objetivo prioritario para atacantes.
- Un fallo en AD puede comprometer toda la organización.

En el mundo real, el riesgo no es solo "que exista AD", sino:

- Cuentas con privilegios excesivos.
- Delegaciones mal diseñadas.
- Grupos anidados difíciles de entender.
- Políticas débiles de contraseñas o bloqueo.
- Falta de monitorización de eventos relevantes (cambios de grupo, creación de cuentas, autenticaciones sospechosas).

### 6.2. IAM en la nube (Azure, AWS, GCP)

En la nube, la identidad se gestiona a través de servicios IAM:

- Azure AD / Entra ID (hoy en día).
- AWS IAM (usuarios, roles, políticas).
- Google Cloud IAM.

Funcionan con principios similares:

- Usuarios y grupos.
- Roles y permisos.
- Políticas que definen qué acciones se pueden realizar sobre qué recursos.

Errores típicos:

- Dar permisos excesivos ("Administrator", "Owner") a usuarios que no lo necesitan.
- Usar claves de acceso estáticas sin rotación.
- Exponer accidentalmente identidades en repos públicos (credenciales hardcodeadas).
- No activar logs o no revisarlos (sin visibilidad, no hay detección).

Un concepto clave en cloud: muchos accesos se hacen por tokens y roles temporales. Eso aporta control, pero también requiere disciplina:

- Revisar quién puede asumir roles.
- Limitar permisos por recurso.
- Aplicar MFA y conditional access cuando exista.

**Prompt IA:**  
"Actúa como instructor de identidad. Explica Active Directory e IAM cloud a alto nivel, comparando conceptos equivalentes (usuarios, grupos, roles, políticas). Luego crea una lista de 12 malas prácticas de identidad que suelen causar incidentes, y cómo mitigarlas con controles de mínimo privilegio, MFA, logging y revisión periódica."

---

## 7. Laboratorio de la Semana 4

Vamos a hacer ejercicios sencillos pero muy útiles para aterrizar estos conceptos.

El objetivo no es crear una infraestructura perfecta, sino comprender "qué ocurre" cuando:

- Un dato existe.
- Un permiso lo protege o lo expone.
- Un backup se diseña bien o mal.
- Una identidad determina el alcance del daño.

**Prompt IA:**  
"Actúa como guía de laboratorio. Para cada ejercicio de la Semana 4, dime qué objetivo de seguridad entrena, qué evidencia debería capturar (salidas, capturas, listados), y qué conclusiones debería escribir en el entregable. Incluye 5 problemas típicos que podrían ocurrir y cómo solucionarlos."

### 7.1. Ejercicio 1 – Simulando datos importantes y "backup pobre"

En tu máquina Windows:

1. Crea una carpeta en el Escritorio llamada "Datos_Proyecto".
2. Dentro, crea varios ficheros de texto ("informe1.txt", "clientes.txt", etc.) con contenido de prueba.
3. Haz una copia de esa carpeta en otro lugar, por ejemplo en "C:\Backups\Datos_Proyecto".

Ahora imagina:

- "Datos_Proyecto" es la carpeta que se cifra en un ataque de ransomware.
- "C:\Backups\Datos_Proyecto" es tu "copia de seguridad".

Reflexiona:

- Qué pasa si el ransomware también cifra "C:\Backups" porque está accesible.
- Si crees que este "backup" es robusto.
- Qué mejorarías (por ejemplo, backups offline, en otra máquina, en la nube, con versiones, con inmutabilidad).

Añade también estas ideas:

- Si el backup está en el mismo equipo y con permisos del mismo usuario, el atacante suele poder alcanzarlo.
- Si el backup no tiene versiones, un borrado o cifrado puede dejarte sin punto bueno.
- Si el backup no se prueba, puede fallar el día que más lo necesitas.

Documenta tu análisis en:

"01-Datos-Identidad-Semana_4_Backup_Windows.md"

---

### 7.2. Ejercicio 2 – Permisos en un directorio Linux

En tu Ubuntu Server:

1. Crea un directorio "/datos_criticos":

   sudo mkdir /datos_criticos

2. Crea un fichero dentro:

   echo "informacion sensible" | sudo tee /datos_criticos/secret.txt

3. Observa sus permisos:

   ls -l /datos_criticos

4. Crea un usuario "usuario1" y "usuario2" si no los tienes:

   sudo adduser usuario1
   sudo adduser usuario2

5. Cambia el propietario del directorio a "usuario1":

   sudo chown -R usuario1:usuario1 /datos_criticos

6. Ajusta permisos para que solo "usuario1" pueda leer y escribir:

   sudo chmod 700 /datos_criticos

7. Intenta acceder con "usuario2":

   su - usuario2
   ls /datos_criticos

Observa:

- Cómo Linux usa permisos para proteger datos.
- Qué pasaría si en lugar de "700" tuvieras "777".
- Qué diferencia hay entre no poder listar un directorio y no poder leer un archivo.

Consejo de aprendizaje: intenta explicar con tus palabras qué significa "700" y a quién afecta. Eso te entrena a justificar permisos en una empresa.

Documenta este experimento en:

"01-Datos-Identidad-Semana_4_Permisos_Linux.md"

---

### 7.3. Ejercicio 3 – RPO/RTO en tu propio laboratorio

Piensa en tu laboratorio:

1. Elige un "servicio crítico" de tu lab (por ejemplo, el servidor Ubuntu con un servicio web, o una carpeta en Windows con documentación de práctica).

2. Define:

- Si ese servicio fallara, cuánto tiempo podrías estar sin él sin bloquear tu aprendizaje  
  → Ese sería tu **RTO** aproximado.
- Si perdieras todos los datos relacionados (scripts, informes), cuántas horas o días de trabajo estarías dispuesto a perder  
  → Ese sería tu **RPO** aproximado.

3. Anota:

- Si haces alguna copia de seguridad de tu repositorio o de tus máquinas.
- Si deberías empezar a versionar tus ejercicios en GitHub como forma de backup.

Añade un extra útil:

- Qué restaurarías primero si tu lab se rompiera (prioridades).
- Qué datos son irrecuperables si no están en GitHub (por ejemplo, notas locales no subidas).

Escribe estas reflexiones en:

"01-Datos-Identidad-Semana_4_RPO_RTO.md"

---

### 7.4. Ejercicio 4 – Identidad y permisos en Windows

En tu máquina Windows:

1. Crea un nuevo usuario local (no administrador), por ejemplo "UsuarioDatos".
2. Cierra sesión en tu usuario habitual e inicia sesión con "UsuarioDatos".
3. Crea una carpeta "C:\DatosUsuarios".
4. Desde el usuario "UsuarioDatos", crea un archivo dentro.
5. Vuelve a tu usuario principal (si es administrador).
6. Haz clic derecho en la carpeta "C:\DatosUsuarios" → Propiedades → Seguridad.
7. Observa:
- Qué usuarios y grupos tienen permisos.
- Qué tipo de permisos (lectura, escritura, control total).

Reflexiona:

- Si tu usuario principal se viera comprometido por un atacante, qué datos podría modificar.
- Qué pasaría si esta carpeta estuviera compartida en red con permisos de escritura para muchos usuarios.
- Cómo influye el grupo "Administradores" en el permiso efectivo.

Añade tu análisis a:

"01-Datos-Identidad-Semana_4_Permisos_Windows.md"

---

## 8. Entregables de la Semana 4

Al completar esta semana, deberías tener en tu repositorio:

1. "01-Datos-Identidad-Semana_4_Backup_Windows.md"  
- Descripción de la simulación de backup.
- Reflexión sobre por qué ese backup es débil o fuerte.
- Propuestas de mejora.

2. "01-Datos-Identidad-Semana_4_Permisos_Linux.md"  
- Comandos utilizados.
- Resultados de "ls -l".
- Explicación de cómo los permisos han protegido o no el contenido.

3. "01-Datos-Identidad-Semana_4_RPO_RTO.md"  
- RPO y RTO aproximados de tu entorno de laboratorio.
- Ideas para mejorar tu resiliencia (por ejemplo: uso sistemático de GitHub).

4. "01-Datos-Identidad-Semana_4_Permisos_Windows.md"  
- Usuarios y permisos observados.
- Reflexión sobre riesgos si un usuario se ve comprometido.

Opcionalmente, puedes ampliar tu "Lab_Setup_Report.md":

- Añadiendo una sección sobre "Datos críticos del laboratorio y cómo los protejo".
- Un listado corto de "rutas críticas" o "carpetas sensibles" en tus VMs.
- Una mini-política personal de backups (qué, dónde, cada cuánto).

**Prompt IA:**  
"Actúa como revisor de entregables. Crea una plantilla en Markdown para cada uno de los 4 entregables de la Semana 4, con secciones: contexto, pasos realizados, evidencia, resultados, análisis de riesgo, conclusiones y mejoras. Incluye un ejemplo de redacción corta y clara para cada sección."

---

## 9. Checklist de cierre de la Semana 4

Marca como completado cuando:

- [ ] Entiendes las diferencias entre almacenamiento local, NAS, SAN y almacenamiento en la nube.
- [ ] Puedes explicar qué es NTFS y ext4 y por qué los permisos son clave en ambos mundos.
- [ ] Comprendes qué son RPO y RTO y cómo se aplican en un incidente (por ejemplo, ransomware).
- [ ] Has simulado, aunque de forma sencilla, un escenario de backup en Windows.
- [ ] Has experimentado con permisos en un directorio Linux y has visto cómo afectan al acceso.
- [ ] Has reflexionado sobre la identidad (usuarios) y el acceso a datos en Windows.
- [ ] Has creado todos los ficheros de la Semana 4 y los has añadido a tu repositorio.

Si has llegado hasta aquí, ya no ves los datos como "ficheros sueltos", sino como:

- Información con valor.
- Sujeta a riesgos.
- Protegida (o no) por mecanismos de almacenamiento y permisos.
- Dependiente de identidades y de estrategias de copia y recuperación.

Un indicador de que has interiorizado esta semana es que puedes explicar con calma:

- Por qué un backup local no es suficiente.
- Por qué un permiso mal puesto puede multiplicar el impacto.
- Por qué un usuario comprometido no es solo "una cuenta", sino una puerta a datos.
- Por qué RPO y RTO son decisiones que afectan al negocio.

En el siguiente bloque del curso daremos el salto hacia:

- **Cloud de forma más estructurada (Azure, AWS, GCP)**.
- **Automatización básica (Python, PowerShell, Bash)**.
- Y comenzaremos a tocar con más fuerza la parte ofensiva y defensiva aplicadas.

**Prompt IA:**  
"Actúa como evaluador final. Crea un mini-examen de cierre de la Semana 4 con: 10 preguntas tipo test, 6 preguntas abiertas, y 3 casos prácticos (ransomware, share mal configurado, bucket cloud expuesto). Para cada caso, pide definir impacto, qué evidencias buscar y qué controles aplicar. Incluye una rúbrica simple de puntuación."

---

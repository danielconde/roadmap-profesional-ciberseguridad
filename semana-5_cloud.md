# Semana 5 – Fundamentos de Cloud y Seguridad en la Nube ☁️🔐

## 0. Introducción a la semana

Hasta ahora has trabajado sobre todo en entornos "on-premise" simulados:

- Máquinas locales (Kali, Windows, Ubuntu) en tu laboratorio.
- Redes internas, procesos, servicios, permisos, datos.

Pero la realidad actual es que **la mayoría de empresas usan servicios en la nube**:

- Infraestructura en Azure, AWS o Google Cloud.
- Aplicaciones desplegadas en máquinas virtuales, contenedores o servicios gestionados.
- Almacenamiento de datos críticos en buckets, blobs, bases de datos cloud, etc.

En esta Semana 5, el objetivo es que entiendas:

- Qué es "cloud computing" en términos prácticos.
- Qué partes gestiona el proveedor y qué partes son responsabilidad del cliente.
- Cómo se amplían los conceptos que ya conoces (red, identidad, datos) al mundo cloud.
- Cuáles son los errores de configuración más habituales que llevan a incidentes de seguridad.
- Cómo desplegar (o al menos entender cómo se desplegaría) una máquina virtual en Azure, AWS o GCP y qué implica abrir un puerto.

La nube no cambia la física de la seguridad, cambia el "tablero":

- En on-premise compras y montas todo.
- En cloud lo tienes ya disponible, pero configurarlo mal es muy fácil, muy rápido, y a veces muy visible desde Internet.

Por eso cloud security tiene una mezcla peculiar:

- Mucho de "buenas prácticas" (mismo mínimo privilegio, mismos permisos, misma segmentación).
- Mucho de "configuración" (un click equivocado y dejas un recurso público).
- Mucho de "identidad" (todo pasa por roles, tokens, llaves, permisos en APIs).

No buscamos que seas arquitecto cloud todavía, sino que tengas una **base sólida** para poder hablar de seguridad en la nube con criterio, entender alertas y revisar configuraciones básicas sin ir a ciegas.

**Prompt IA:**  
"Actúa como instructor de ciberseguridad con foco en cloud. Introduce la nube a un alumno que ya entiende redes, sistemas y permisos. Explica por qué cloud amplifica el impacto de configuraciones incorrectas. Incluye 8 ejemplos de errores de cloud que acaban en incidentes y conéctalos con conceptos previos (puertos, identidades, datos, permisos)."

---

## 1. Objetivos de aprendizaje de la Semana 5

Al finalizar esta semana deberías ser capaz de:

1. Explicar qué es la computación en la nube y los principales modelos de servicio (IaaS, PaaS, SaaS).
2. Describir el **modelo de responsabilidad compartida** entre proveedor Cloud y cliente.
3. Identificar los componentes básicos de infraestructura en Azure, AWS y Google Cloud (VMs, redes, grupos de seguridad, almacenamiento).
4. Comprender cómo se aplican los conceptos de red (IP, puertos, firewall) dentro de un entorno cloud.
5. Entender qué es un Security Group / NSG y qué papel juega en seguridad.
6. Reconocer los riesgos más típicos de mala configuración (buckets públicos, puertos abiertos a todo Internet, credenciales expuestas).
7. Realizar un pequeño laboratorio de despliegue o exploración de una VM cloud y analizar su superficie de exposición.
8. Documentar tus observaciones de forma clara para tu repositorio.

Si traduces estos objetivos a "vida real", significan que podrás:

- Interpretar una arquitectura cloud básica cuando la veas en un diagrama o en un portal.
- Entender qué se está exponiendo a Internet y por qué.
- Identificar dónde suelen estar los fallos que generan incidentes: identidad, reglas de red, almacenamiento, logs.
- Tener criterio para decir "esto está demasiado abierto" o "esto necesita restricciones".

**Prompt IA:**  
"Actúa como evaluador de aprendizaje de la Semana 5. Genera 12 preguntas de repaso alineadas a los 8 objetivos. Para cada pregunta añade: respuesta esperada, explicación breve, y un error típico de principiante. Incluye 2 mini-casos: VM expuesta por RDP/SSH y bucket público accidental."

---

## 2. ¿Qué es la computación en la nube? (visión práctica)

### 2.1. Definición aterrizada

La computación en la nube consiste en **usar recursos de computación (servidores, almacenamiento, redes, bases de datos, etc.) proporcionados por un tercero**, a través de Internet, en lugar de gestionarlos tú mismo en tu propio CPD.

En vez de comprar y mantener servidores físicos:

- "Alquilas" recursos a **Azure**, **AWS** o **Google Cloud**.
- Pagas en función del uso (tiempo de CPU, almacenamiento, transferencia de datos).
- Puedes escalar hacia arriba o hacia abajo cuando lo necesites.

La nube aporta dos cambios prácticos muy importantes:

- Velocidad: puedes desplegar recursos en minutos.
- Elasticidad: puedes crecer o reducir según demanda sin cambiar hardware.

Desde seguridad, eso también significa:

- Un error se puede replicar en minutos (por ejemplo, desplegar 20 VMs con la misma configuración insegura).
- La superficie de ataque puede crecer sin que nadie lo note si no hay gobernanza y visibilidad.
- La identidad se vuelve central, porque casi todo se gestiona por API y permisos.

### 2.2. Modelos de servicio: IaaS, PaaS, SaaS

Es importante entender al menos estos tres modelos:

- **IaaS (Infrastructure as a Service)**  
  El proveedor ofrece infraestructura básica:
  - Máquinas virtuales.
  - Redes.
  - Almacenamiento.
  Tú te encargas de instalar y mantener:
  - Sistema operativo.
  - Aplicaciones.
  - Configuración de seguridad interna.

  Desde seguridad, en IaaS tu responsabilidad se parece mucho a un servidor on-premise:
  - Parcheo del SO.
  - Endurecimiento.
  - Gestión de usuarios.
  - Servicios expuestos.
  - Logs, detección, respuesta.

- **PaaS (Platform as a Service)**  
  El proveedor ofrece una plataforma para que despliegues aplicaciones sin preocuparte de la infraestructura subyacente.  
  Ejemplos: servicios de base de datos gestionada, App Services, funciones serverless, colas de mensajería, etc.

  En PaaS, tu foco de seguridad suele moverse hacia:
  - Configuración del servicio (accesos, redes, cifrado, parámetros).
  - Identidad y roles.
  - Seguridad de la aplicación (autenticación, validación, secretos).
  - Monitorización y auditoría del servicio.

- **SaaS (Software as a Service)**  
  El proveedor ofrece una aplicación completa ya lista para usar.  
  Correo corporativo, CRM online, herramientas de gestión, etc.

  En SaaS, la seguridad del cliente suele girar en:
  - Identidad (MFA, accesos condicionales, cuentas).
  - Configuración segura de la aplicación (políticas, permisos, comparticiones).
  - Protección de datos (retención, DLP, clasificación).
  - Monitorización (auditoría de actividad).

En seguridad:

- Cuanto más te acercas a **SaaS**, más responsabilidades asume el proveedor.
- Cuanto más te acercas a **IaaS**, más recae en ti (configuración de sistemas, parches, endurecimiento, etc.).

Un truco mental: imagina un "slider" de control:

- IaaS: máximo control, máxima responsabilidad.
- PaaS: control medio, responsabilidad compartida más marcada.
- SaaS: menos control técnico, pero responsabilidad fuerte en identidad y configuración.

**Prompt IA:**  
"Explica cloud computing a nivel práctico para un alumno de ciberseguridad. Incluye analogías simples para IaaS, PaaS y SaaS, pero añade también un mapa de responsabilidades: qué gestiona el proveedor y qué gestiona el cliente en cada modelo. Termina con 10 ejemplos concretos de servicios típicos de cada modelo (sin entrar en marcas, solo tipos)."

---

## 3. Modelo de responsabilidad compartida

Uno de los conceptos más importantes en seguridad cloud es el **shared responsibility model**.

El error clásico es pensar: "como está en la nube, el proveedor lo asegura todo". En realidad:

- El proveedor asegura "la nube" (infraestructura y servicio base).
- Tú aseguras "lo que pones en la nube" (configuración, identidad, datos, uso).

### 3.1. ¿Qué hace el proveedor?

El proveedor (Microsoft, Amazon, Google, etc.) se encarga de:

- Seguridad física de los centros de datos.
- Energía, refrigeración, hardware, reemplazo de discos.
- Seguridad de la infraestructura subyacente (hipervisores, red central, etc.).
- Disponibilidad básica del servicio (SLA).

Esto es importante: el proveedor suele ofrecer un entorno estable, redundante y bien gestionado, pero no puede adivinar qué quieres exponer o proteger. El proveedor te da herramientas y límites, pero tú decides la configuración final.

### 3.2. ¿Qué haces tú como cliente?

Tú eres responsable de:

- Configurar correctamente:
  - Redes (subredes, reglas, firewalls).
  - Máquinas virtuales (SO, parches, servicios).
  - Almacenamiento (permisos, cifrado, acceso).
- Gestionar la identidad:
  - Usuarios.
  - Roles.
  - Permisos sobre recursos.
- Proteger tus credenciales y secretos (claves, tokens, contraseñas).
- Vigilar tus logs, alertas y riesgos.

En cloud, hay un detalle especialmente importante: "todo se gestiona por permisos". Si tu IAM está mal, todo lo demás cae como fichas.

### 3.3. Ejemplo práctico

Si despliegas una máquina virtual en la nube y:

- Abres el puerto 3389 (RDP) a todo Internet ("0.0.0.0/0").
- Dejas una contraseña débil para el administrador.

Y alguien entra por fuerza bruta:

- El proveedor ha cumplido su parte (la VM está disponible).
- El fallo está en tu configuración.

Esto es un ejemplo muy real de incidente por mala configuración.

Amplía el ejemplo con mentalidad SOC:

- Primero verías intentos de login (muchos) contra RDP/SSH.
- Luego verías un login exitoso.
- Después podrías ver creación de usuarios, instalación de herramientas, conexiones salientes.
- Y finalmente movimiento lateral si esa VM tiene permisos hacia otros recursos.

Es decir, una mala regla de red puede ser el primer dominó.

**Prompt IA:**  
"Actúa como instructor y crea una explicación del modelo de responsabilidad compartida con 3 ejemplos de incidentes: VM con puerto abierto, bucket público y permisos IAM excesivos. Para cada ejemplo: qué responsabilidad era del proveedor, cuál era del cliente, qué señal vería un SOC y qué control preventivo habría evitado el incidente."

---

## 4. Componentes comunes en Azure, AWS y Google Cloud

Aunque cambie el nombre, los conceptos son muy similares. Lo importante no es memorizar marcas, sino reconocer patrones: compute, red, reglas de acceso, almacenamiento, identidad, logs.

### 4.1. Máquinas virtuales (compute)

- En **Azure** → Azure Virtual Machines.
- En **AWS** → EC2 (Elastic Compute Cloud).
- En **Google Cloud** → Compute Engine.

Son servidores virtuales en la nube donde tú decides:

- Sistema operativo.
- Software instalado.
- Reglas de firewall asociadas.

Desde seguridad, una VM se comporta como un servidor "de toda la vida":

- Si lo parcheas mal, se expone.
- Si abres servicios de administración, aumenta el riesgo.
- Si no recoges logs, pierdes visibilidad.
- Si le das permisos cloud excesivos, se convierte en pivot hacia otros recursos.

### 4.2. Redes virtuales

- En **Azure** → Virtual Network (VNet).
- En **AWS** → VPC (Virtual Private Cloud).
- En **GCP** → VPC Network.

Dentro de esas redes puedes crear:

- Subredes (subnets).
- Rutas.
- Puertas de enlace (gateways) para salida a Internet.

Concepto clave: dentro de una red virtual, tienes segmentación lógica. Eso se parece a VLANs o redes internas, pero gestionado por software.

Y también aparece la exposición controlada:

- IP privada: comunicación interna.
- IP pública: exposición a Internet.
- NAT, gateways, reglas: el "camino" de entrada y salida.

### 4.3. Grupos de seguridad / NSG

Estos son como firewalls "cercanos" a tus máquinas:

- En **Azure** → NSG (Network Security Group).
- En **AWS** → Security Groups.
- En **GCP** → Firewall rules.

Te permiten definir:

- Qué tráfico entrante y saliente se permite.
- Desde qué IPs o rangos.
- Hacia qué puertos.

Ejemplo regla típica (no ideal):

- Permitir tráfico entrante en puerto 22 (SSH) desde 0.0.0.0/0

Eso significa: cualquiera desde Internet puede intentar conectarse por SSH a tu máquina. No es una buena práctica sin más controles (listas permitidas, VPN, llaves SSH, MFA en salto, etc.).

Idea práctica: estas reglas "definen la puerta". Pero además hay otra capa:

- En la VM: el firewall del sistema operativo, servicios activos, autenticación fuerte.
- Es decir, no dependes solo de la regla cloud, pero la regla cloud puede ser la barrera más visible.

### 4.4. Almacenamiento de objetos (blobs / buckets)

- Azure → Blob Storage.
- AWS → S3.
- GCP → Cloud Storage.

Aquí se guardan:

- Ficheros estáticos.
- Backups.
- Logs.
- Datos de aplicaciones.

Errores típicos:

- Buckets públicos por defecto o por error humano.
- Ficheros sensibles accesibles sin autenticación.
- No cifrar datos sensibles en reposo.
- No activar versionado o retención (más fácil borrar o cifrar sin recuperación).

En seguridad, el almacenamiento de objetos es un foco continuo de incidentes porque:

- Se accede por URL o API.
- Es muy fácil compartir "para salir del paso".
- Y luego se queda abierto meses o años.

**Prompt IA:**  
"Actúa como instructor. Haz una tabla mental explicada (sin usar tablas) de equivalencias entre proveedores: compute, red, security groups, almacenamiento de objetos e identidad. Para cada componente explica: para qué sirve, qué mal configuración típica existe y qué control mínimo debería aplicarse."

---

## 5. Seguridad en redes cloud: extensión de lo que ya conoces

Todo lo que viste en la Semana 2 (IP, puertos, tráfico normal vs sospechoso) se aplica en la nube, con algunas particularidades:

- Las máquinas tienen IPs privadas dentro de la VPC/VNet.
- Pueden tener IPs públicas para ser accesibles desde Internet.
- Los Security Groups / NSG actúan como filtros de entrada y salida.
- Puedes tener firewalls adicionales o WAFs delante de aplicaciones web.

La nube introduce además el "plano de control":

- No solo hay tráfico a la VM.
- Hay llamadas a APIs para crear, borrar, modificar recursos.
- Muchas intrusiones cloud no empiezan por puertos, empiezan por credenciales y API.

Como analista o pentester:

- Te interesará saber qué puertos están expuestos a Internet.
- Querrás entender desde dónde se puede acceder (listas de IP permitidas).
- En SOC, mirarás logs de acceso a servicios cloud, identificación de actividades raras, cambios de configuración, etc.

Un enfoque práctico:

- Pregunta 1: "Qué está expuesto"
- Pregunta 2: "Quién puede tocarlo"
- Pregunta 3: "Cómo lo estoy monitorizando"

Si una aplicación web está expuesta (puerto 443) eso puede ser correcto. El problema aparece cuando:

- Administración también está expuesta (22, 3389, paneles).
- O la identidad que gestiona recursos tiene permisos excesivos.
- O no hay alertas básicas ante cambios y accesos.

**Prompt IA:**  
"Actúa como analista SOC especializado en cloud. Explica cómo se traduce la seguridad de red tradicional a cloud: IP privada/pública, security group, rutas, gateways. Luego lista 12 señales de tráfico o eventos sospechosos relacionados con exposición de puertos y cambios en reglas. Incluye qué revisarías primero para validar si es un incidente real."

---

## 6. Errores de configuración habituales que llevan a incidentes

Aunque no vamos a entrar aún en ataques avanzados a cloud, sí es útil que conozcas fallos típicos. Estos fallos suelen ser peligrosos porque:

- Son fáciles de cometer.
- Se detectan tarde si no hay monitorización.
- Pueden dar acceso directo o fuga masiva de datos.

1. **Puertos administrativos expuestos a todo Internet**  
   - RDP (3389), SSH (22), WinRM, etc.
   - Sin listas de IP restringidas.
   - Sin MFA ni políticas adicionales.
   - Sin mecanismos de salto seguro (VPN, bastion host).

2. **Buckets de almacenamiento públicos sin querer**  
   - Datos sensibles accesibles con una simple URL.
   - Listados de directorios o ficheros expuestos.
   - Sin versionado ni retención, lo que complica recuperación.

3. **Credenciales y claves en repositorios públicos**  
   - Claves de AWS/Azure/GCP subidas a GitHub sin querer.
   - Tokens de acceso embebidos en código.
   - Variables de entorno volcadas en logs o errores.

4. **Roles IAM excesivamente permisivos**  
   - Políticas tipo "Administrator" para usuarios que no lo necesitan.
   - Principio de privilegio mínimo no aplicado.
   - Roles reutilizados para todo por comodidad.

5. **Falta de logging y monitorización**  
   - No habilitar logs de acceso a buckets, VMs, APIs.
   - No configurar alertas básicas de actividad sospechosa.
   - No revisar cambios de configuración de forma continua.

Añade dos patrones muy comunes:

- "Shadow IT": equipos que crean recursos sin control central, sin etiquetas, sin estándares.
- "Exposición accidental": un recurso se abre para una prueba y se olvida abierto.

Muchos incidentes modernos en la nube no requieren vulnerabilidades técnicas avanzadas, sino simplemente aprovechar **configuraciones descuidadas**.

**Prompt IA:**  
"Actúa como red team y blue team a la vez. Para cada uno de los 5 errores típicos, describe: cómo lo aprovecharía un atacante, qué evidencia podría dejar, y qué control preventivo o detective debería implementar el cliente. Mantén el enfoque en buenas prácticas y detección."

---

## 7. Laboratorio de la Semana 5

El objetivo del laboratorio no es que te conviertas en admin cloud en una semana, sino que:

- Pierdas el miedo al portal de Azure/AWS/GCP.
- Veas cómo se define una red, una VM y un conjunto de reglas de acceso.
- Entiendas visualmente qué significa exponer un puerto.

Nota: si no puedes crear recursos en los tres proveedores, céntrate al menos en **uno** (el que te resulte más accesible). El laboratorio está pensado para ser adaptable.

En esta semana, el aprendizaje está más en "interpretar lo que ves" que en desplegar infra compleja. Si consigues reconocer:

- Dónde están las reglas de acceso.
- Dónde están los permisos IAM.
- Dónde está el almacenamiento.
- Dónde están los logs.

Entonces ya tienes una base que te servirá en análisis reales.

**Prompt IA:**  
"Actúa como guía de laboratorio paso a paso para un alumno principiante en cloud. Diseña un plan seguro: explorar portal, crear VM minimal, restringir acceso, revisar reglas, y documentar. Incluye una lista de comprobaciones de seguridad mínimas antes de dejar una VM encendida."

---

### 7.1. Ejercicio 1 – Explorando el portal cloud

Elige un proveedor (Azure, AWS o GCP) y:

1. Accede al portal web (ej. portal.azure.com, console.aws.amazon.com, etc.).
2. Localiza:
   - Sección de máquinas virtuales (Virtual Machines / EC2 / Compute Engine).
   - Sección de redes (VNet / VPC / Networking).
   - Sección de almacenamiento (Storage / S3 / Cloud Storage).
   - Sección de IAM o identidad (Usuarios/Roles/Permisos).

3. Haz capturas de:
   - La pantalla donde se listan VMs (aunque todavía no tengas ninguna).
   - La sección donde se configuran reglas de firewall / Security Groups / NSG.

Crea un fichero:

"02-Cloud-Seguridad-Semana_5_Exploracion_Portal.md"

Incluye:

- Lista de secciones encontradas.
- Breve descripción de lo que crees que hace cada una.
- Capturas (o enlaces a imágenes) si quieres enriquecer el repo.
- Qué diferencias ves entre conceptos on-premise y el portal (por ejemplo: todo está organizado por recursos y servicios).

---

### 7.2. Ejercicio 2 – Desplegando (o simulando) una máquina virtual en la nube

Idealmente, despliega una pequeña VM (ej: Linux) en tu proveedor elegido.

Pasos orientativos (conceptuales, adaptables a la plataforma):

1. Crea o localiza una **red virtual** (VNet/VPC) y una subred.
2. Crea una máquina virtual pequeña (tipo free tier si es posible).
   - Sistema operativo Linux o Windows.
   - Parámetros mínimos de CPU/RAM.
3. Configura las reglas de entrada:
   - Permitir SSH (22) o RDP (3389).
   - Preferiblemente, **restringido a tu IP**, no a "0.0.0.0/0".

4. Revisa:
   - IP pública (si la tiene).
   - IP privada.
   - Reglas de Security Group / NSG.

5. Si puedes, intenta conectarte desde tu máquina local:
   - SSH si es Linux ("ssh usuario@IP_PUBLICA").
   - RDP si es Windows.

En tu repositorio, crea:

"02-Cloud-Seguridad-Semana_5_VM_Cloud.md"

Incluye:

- Tipo de VM creada (SO, tamaño).
- Configuración de red (IP pública, privada).
- Reglas de seguridad configuradas.
- Reflexión:
  - "Si abriera este puerto a todo Internet, ¿qué riesgo supondría?"
  - "Qué medidas pondría para reducir ese riesgo"

Si no puedes desplegar por limitaciones de cuenta, haz el ejercicio en "modo diseño": describe los pasos que harías y revisa bien la pantalla de creación de VM para entender cada parámetro.

---

### 7.3. Ejercicio 3 – Analizando exposición de puertos en la nube

Si has creado una VM Linux en la nube:

1. Desde tu Kali (o desde tu host, con cuidado si usas tu red real), haz un escaneo controlado a la **IP pública** de la VM.

   Nota importante: solo si la VM es tuya y tienes permiso, y ajustando el escaneo para no hacer nada agresivo.

   Ejemplo (desde Kali):

   nmap -sV IP_PUBLICA_DE_LA_VM

2. Observa:
   - Qué puertos aparecen.
   - Si coinciden con lo que definiste en las reglas de Security Group / NSG.
   - Si aparece algo que no esperabas, qué podría significar (servicio adicional, regla mal aplicada, configuración por defecto).

3. Piensa:
   - Si fueras un atacante y vieras esta máquina en Internet, qué pensarías.
   - Si vieras muchos puertos abiertos, te parecería una buena práctica.
   - Qué pondrías delante si esto fuera producción (VPN, bastion, WAF, restricciones por IP).

Documenta en:

"02-Cloud-Seguridad-Semana_5_Exposicion_Puertos.md"

Si no puedes hacer el escaneo, describe conceptualmente cómo lo harías y qué te gustaría comprobar.

---

### 7.4. Ejercicio 4 – Caso mental: bucket mal configurado

Este ejercicio es de reflexión, no técnico.

Imagina:

- Una empresa guarda copias de facturas de clientes en un bucket S3 (o Blob Storage).
- Ese bucket está configurado como "público" sin que nadie se haya dado cuenta.
- Cualquier persona con el enlace correcto puede descargar ficheros.
- No hay cifrado, ni control de acceso, ni registros de acceso activados.

Piensa y responde (por escrito) en:

"02-Cloud-Seguridad-Semana_5_Buckets_Riesgos.md"

- Qué impacto tendría si alguien descubre ese bucket.
- A qué principios de CIA afectaría (Confidencialidad, Integridad, Disponibilidad).
- Qué medidas tomarías para:
  - Detectar este problema.
  - Corregirlo.
  - Evitar que vuelva a ocurrir.

Amplía la reflexión con:

- Qué señales o "pistas" podrían indicar que el bucket estaba siendo accedido por terceros.
- Qué políticas pondrías en la organización para evitar que se creen buckets públicos sin revisión.

---

## 8. Entregables de la Semana 5

Al finalizar la semana deberías tener:

1. "02-Cloud-Seguridad-Semana_5_Exploracion_Portal.md"  
   - Con secciones exploradas en el portal cloud.
   - Descripciones sencillas de cada una.

2. "02-Cloud-Seguridad-Semana_5_VM_Cloud.md"  
   - Con los detalles de la VM (real o simulada).
   - Configuración de red y reglas de seguridad.
   - Reflexiones sobre exposición de puertos.

3. "02-Cloud-Seguridad-Semana_5_Exposicion_Puertos.md"  
   - Resultados (o diseño) de un escaneo básico.
   - Conclusiones sobre superficie de ataque.

4. "02-Cloud-Seguridad-Semana_5_Buckets_Riesgos.md"  
   - Análisis del caso mental del bucket mal configurado.
   - Impacto y medidas de mitigación.

Consejo: intenta que cada entregable sea "útil" en el futuro. No solo describas, también deja:

- Qué aprendiste.
- Qué harías distinto en producción.
- Qué controles mínimos aplicarías.

**Prompt IA:**  
"Actúa como revisor de entregables. Genera una plantilla Markdown para cada uno de los 4 entregables de la Semana 5 con secciones: objetivo, pasos, evidencia, resultados, riesgos detectados, mitigaciones, y conclusiones. Añade un ejemplo de 3-4 líneas por sección para guiar el estilo de redacción."

---

## 9. Checklist de cierre de la Semana 5

Marca como completado cuando:

- [ ] Puedes explicar la diferencia entre IaaS, PaaS y SaaS con ejemplos.
- [ ] Entiendes el modelo de responsabilidad compartida y sabes poner un ejemplo de mala configuración que sea culpa del cliente.
- [ ] Reconoces los elementos básicos de infraestructura cloud: VMs, redes, Security Groups / NSG, almacenamiento de objetos.
- [ ] Has explorado al menos el portal de un proveedor cloud y sabes dónde se gestionan VMs, redes, almacenamiento e IAM.
- [ ] Has diseñado (y si es posible, desplegado) una VM sencilla en la nube y has revisado su superficie de exposición.
- [ ] Puedes razonar sobre los riesgos de un bucket o contenedor mal configurado.
- [ ] Has creado todos los ficheros de la Semana 5 y los has añadido a tu repositorio.

Si has completado esta semana, ya no ves "la nube" como un concepto abstracto, sino como:

- Un entorno con recursos muy parecidos a los on-premise.
- Pero con particularidades de configuración, identidad y exposición.

Una señal de que lo has entendido es que puedes mantener una conversación técnica básica sobre:

- Qué significa exponer un puerto en cloud y cómo mitigarlo.
- Qué es un security group y qué riesgo tiene una regla "0.0.0.0/0".
- Por qué IAM es el núcleo de seguridad cloud.
- Por qué logging y alertas son imprescindibles para detectar cambios y accesos.

En la **Semana 6** empezaremos a combinar esto con **automatización y scripting (Python, PowerShell, Bash) e IA ("vibecoding")**, para que veas cómo la automatización se convierte en un aliado clave en seguridad.

**Prompt IA:**  
"Actúa como evaluador final de la Semana 5. Crea un mini-examen con: 10 preguntas tipo test, 6 preguntas abiertas y 3 casos prácticos (VM expuesta, bucket público, IAM excesivo). Para cada caso pide: impacto CIA, evidencias a revisar y mitigaciones. Incluye una rúbrica simple de puntuación."

---

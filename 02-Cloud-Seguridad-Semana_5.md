# Semana 5 – Fundamentos de Cloud y Seguridad en la Nube ☁️🔐

## 0. Introducción a la semana

Hasta ahora has trabajado sobre todo en entornos “on-premise” simulados:

- Máquinas locales (Kali, Windows, Ubuntu) en tu laboratorio.
- Redes internas, procesos, servicios, permisos, datos.

Pero la realidad actual es que **la mayoría de empresas usan servicios en la nube**:

- Infraestructura en Azure, AWS o Google Cloud.
- Aplicaciones desplegadas en máquinas virtuales, contenedores o servicios gestionados.
- Almacenamiento de datos críticos en buckets, blobs, bases de datos cloud, etc.

En esta Semana 5, el objetivo es que entiendas:

- Qué es **cloud computing** en términos prácticos.
- Qué partes gestiona el proveedor y qué partes son responsabilidad del cliente.
- Cómo se amplían los conceptos que ya conoces (red, identidad, datos) al mundo cloud.
- Cuáles son los errores de configuración más habituales que llevan a incidentes de seguridad.
- Cómo desplegar (o al menos entender cómo se desplegaría) una máquina virtual en Azure, AWS o GCP y qué implica abrir un puerto.

No buscamos que seas arquitecto cloud todavía, sino que tengas una **base sólida** para poder hablar de seguridad en la nube con criterio.

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

---

## 2. ¿Qué es la computación en la nube? (visión práctica)

### 2.1. Definición aterrizada

La computación en la nube consiste en **usar recursos de computación (servidores, almacenamiento, redes, bases de datos, etc.) proporcionados por un tercero**, a través de Internet, en lugar de gestionarlos tú mismo en tu propio CPD.

En vez de comprar y mantener servidores físicos:

- “Alquilas” recursos a **Azure**, **AWS** o **Google Cloud**.
- Pagas en función del uso (tiempo de CPU, almacenamiento, transferencia de datos).
- Puedes escalar hacia arriba o hacia abajo cuando lo necesites.

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

- **PaaS (Platform as a Service)**  
  El proveedor ofrece una plataforma para que despliegues aplicaciones sin preocuparte de la infraestructura subyacente:  
  Ejemplos: servicios de base de datos gestionada, App Services, etc.

- **SaaS (Software as a Service)**  
  El proveedor ofrece una aplicación completa ya lista para usar:  
  Correo corporativo, CRM online, herramientas de gestión, etc.

En seguridad:

- Cuanto más te acercas a **SaaS**, más responsabilidades asume el proveedor.
- Cuanto más te acercas a **IaaS**, más recae en ti (configuración de sistemas, parches, endurecimiento, etc.).

---

## 3. Modelo de responsabilidad compartida

Uno de los conceptos más importantes en seguridad cloud es el **shared responsibility model**.

### 3.1. ¿Qué hace el proveedor?

El proveedor (Microsoft, Amazon, Google, etc.) se encarga de:

- Seguridad física de los centros de datos.
- Energía, refrigeración, hardware, reemplazo de discos.
- Seguridad de la infraestructura subyacente (hipervisores, red central, etc.).
- Disponibilidad básica del servicio (SLA).

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

### 3.3. Ejemplo práctico

Si despliegas una máquina virtual en la nube y:

- Abres el puerto 3389 (RDP) a todo Internet (`0.0.0.0/0`).
- Dejas una contraseña débil para el administrador.

Y alguien entra por fuerza bruta:

- El proveedor ha cumplido su parte (la VM está disponible).
- El fallo está en tu configuración.

Esto es un ejemplo muy real de incidente por mala configuración.

---

## 4. Componentes comunes en Azure, AWS y Google Cloud

Aunque cambie el nombre, los conceptos son muy similares.

### 4.1. Máquinas virtuales (compute)

- En **Azure** → Azure Virtual Machines.
- En **AWS** → EC2 (Elastic Compute Cloud).
- En **Google Cloud** → Compute Engine.

Son servidores virtuales en la nube donde tú decides:

- Sistema operativo.
- Software instalado.
- Reglas de firewall asociadas.

### 4.2. Redes virtuales

- En **Azure** → Virtual Network (VNet).
- En **AWS** → VPC (Virtual Private Cloud).
- En **GCP** → VPC Network.

Dentro de esas redes puedes crear:

- Subredes (subnets).
- Rutas.
- Puertas de enlace (gateways) para salida a Internet.

### 4.3. Grupos de seguridad / NSG

Estos son como firewalls “cercanos” a tus máquinas:

- En **Azure** → NSG (Network Security Group).
- En **AWS** → Security Groups.
- En **GCP** → Firewall rules.

Te permiten definir:

- Qué tráfico entrante y saliente se permite.
- Desde qué IPs o rangos.
- Hacia qué puertos.

Ejemplo regla típica (no ideal):

> Permitir tráfico entrante en puerto 22 (SSH) desde 0.0.0.0/0

Eso significa: cualquiera desde Internet puede intentar conectarse por SSH a tu máquina. No es una buena práctica sin más controles (listas permitidas, VPN, etc.).

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

- Buckets públicos por defecto.
- Ficheros sensibles accesibles sin autenticación.
- No cifrar datos sensibles en reposo.

---

## 5. Seguridad en redes cloud: extensión de lo que ya conoces

Todo lo que viste en la Semana 2 (IP, puertos, tráfico normal vs sospechoso) se aplica en la nube, con algunas particularidades:

- Las máquinas tienen IPs privadas dentro de la VPC/VNet.
- Pueden tener IPs públicas para ser accesibles desde Internet.
- Los Security Groups / NSG actúan como filtros de entrada y salida.
- Puedes tener firewalls adicionales o WAFs delante de aplicaciones web.

Como analista o pentester:

- Te interesará saber qué puertos están expuestos a Internet.
- Querrás entender desde dónde se puede acceder (listas de IP permitidas).
- En SOC, mirarás logs de acceso a servicios cloud, identificación de actividades raras, etc.

---

## 6. Errores de configuración habituales que llevan a incidentes

Aunque no vamos a entrar aún en ataques avanzados a cloud, sí es útil que conozcas fallos típicos:

1. **Puertos administrativos expuestos a todo Internet**  
   - RDP (3389), SSH (22), WinRM, etc.
   - Sin listas de IP restringidas.
   - Sin MFA ni políticas adicionales.

2. **Buckets de almacenamiento públicos sin querer**  
   - Datos sensibles accesibles con una simple URL.
   - Listados de directorios o ficheros expuestos.

3. **Credenciales y claves en repositorios públicos**  
   - Claves de AWS/ Azure / GCP subidas a GitHub sin querer.
   - Tokens de acceso embebidos en código.

4. **Roles IAM excesivamente permisivos**  
   - Políticas tipo “Administrator” para usuarios que no lo necesitan.
   - Principio de privilegio mínimo no aplicado.

5. **Falta de logging y monitorización**  
   - No habilitar logs de acceso a buckets, VMs, APIs.
   - No configurar alertas básicas de actividad sospechosa.

Muchos incidentes modernos en la nube no requieren vulnerabilidades técnicas avanzadas, sino simplemente aprovechar **configuraciones descuidadas**.

---

## 7. Laboratorio de la Semana 5

El objetivo del laboratorio no es que te conviertas en admin cloud en una semana, sino que:

- Pierdas el miedo al portal de Azure/AWS/GCP.
- Veas cómo se define una red, una VM y un conjunto de reglas de acceso.
- Entiendas visualmente qué significa exponer un puerto.

⚠️ Nota: si no puedes crear recursos en los tres proveedores, céntrate al menos en **uno** (el que te resulte más accesible). El laboratorio está pensado para ser adaptable.

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

`02-Cloud-Seguridad-Semana_5_Exploracion_Portal.md`

Incluye:

- Lista de secciones encontradas.
- Breve descripción de lo que crees que hace cada una.
- Capturas (o enlaces a imágenes) si quieres enriquecer el repo.

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
   - Preferiblemente, **restringido a tu IP**, no a `0.0.0.0/0`.

4. Revisa:
   - IP pública (si la tiene).
   - IP privada.
   - Reglas de Security Group / NSG.

5. Si puedes, intenta conectarte desde tu máquina local:
   - SSH si es Linux (`ssh usuario@IP_PUBLICA`).
   - RDP si es Windows.

En tu repositorio, crea:

`02-Cloud-Seguridad-Semana_5_VM_Cloud.md`

Incluye:

- Tipo de VM creada (SO, tamaño).
- Configuración de red (IP pública, privada).
- Reglas de seguridad configuradas.
- Reflexión:  
  > “Si abriera este puerto a todo Internet, ¿qué riesgo supondría?”

Si no puedes desplegar por limitaciones de cuenta, haz el ejercicio en “modo diseño”: describe los pasos que harías y revisa bien la pantalla de creación de VM para entender cada parámetro.

---

### 7.3. Ejercicio 3 – Analizando exposición de puertos en la nube

Si has creado una VM Linux en la nube:

1. Desde tu Kali (o desde tu host, con cuidado si usas tu red real), haz un escaneo controlado a la **IP pública** de la VM.

   > ⚠️ Solo si la VM es tuya y tienes permiso, y ajustando el escaneo para no hacer nada agresivo.

   Ejemplo (desde Kali):

   ```bash
   nmap -sV IP_PUBLICA_DE_LA_VM
   ```

2. Observa:
   - ¿Qué puertos aparecen?
   - ¿Coinciden con lo que definiste en las reglas de Security Group / NSG?

3. Piensa:
   - Si fueras un atacante y vieras esta máquina en Internet, ¿qué pensarías?
   - Si vieras muchos puertos abiertos, ¿te parecería una buena práctica?

Documenta en:

`02-Cloud-Seguridad-Semana_5_Exposicion_Puertos.md`

Si no puedes hacer el escaneo, describe conceptualmente cómo lo harías y qué te gustaría comprobar.

---

### 7.4. Ejercicio 4 – Caso mental: bucket mal configurado

Este ejercicio es de reflexión, no técnico.

Imagina:

- Una empresa guarda copias de facturas de clientes en un bucket S3 (o Blob Storage).
- Ese bucket está configurado como “público” sin que nadie se haya dado cuenta.
- Cualquier persona con el enlace correcto puede descargar ficheros.
- No hay cifrado, ni control de acceso, ni registros de acceso activados.

Piensa y responde (por escrito) en:

`02-Cloud-Seguridad-Semana_5_Buckets_Riesgos.md`

- ¿Qué impacto tendría si alguien descubre ese bucket?
- ¿A qué principios de CIA afectaría (Confidencialidad, Integridad, Disponibilidad)?
- ¿Qué medidas tomarías para:
  - Detectar este problema.
  - Corregirlo.
  - Evitar que vuelva a ocurrir?

---

## 8. Entregables de la Semana 5

Al finalizar la semana deberías tener:

1. `02-Cloud-Seguridad-Semana_5_Exploracion_Portal.md`  
   - Con secciones exploradas en el portal cloud.
   - Descripciones sencillas de cada una.

2. `02-Cloud-Seguridad-Semana_5_VM_Cloud.md`  
   - Con los detalles de la VM (real o simulada).
   - Configuración de red y reglas de seguridad.
   - Reflexiones sobre exposición de puertos.

3. `02-Cloud-Seguridad-Semana_5_Exposicion_Puertos.md`  
   - Resultados (o diseño) de un escaneo básico.
   - Conclusiones sobre superficie de ataque.

4. `02-Cloud-Seguridad-Semana_5_Buckets_Riesgos.md`  
   - Análisis del caso mental del bucket mal configurado.
   - Impacto y medidas de mitigación.

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

Si has completado esta semana, ya no ves “la nube” como un concepto abstracto, sino como:

- Un entorno con recursos muy parecidos a los on-premise.
- Pero con particularidades de configuración, identidad y exposición.

En la **Semana 6** empezaremos a combinar esto con **automatización y scripting (Python, PowerShell, Bash) e IA (vibecoding)**, para que veas cómo la automatización se convierte en un aliado clave en seguridad.

---

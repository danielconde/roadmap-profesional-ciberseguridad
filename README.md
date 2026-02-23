# Roadmap Profesional de Ciberseguridad en 3 Meses 🚀

Autor: Daniel Conde → https://www.linkedin.com/in/daniconde/

Nivel: Principiante → Intermedio

Duración: 12 semanas

Enfoque: Blue Team | Pentesting | Purple Team | Cloud | Automatización

Objetivo: Preparación para empleo real

---

## 📌 Visión General

Este roadmap está diseñado para transformar a una persona sin experiencia previa en un perfil junior sólido (con base operativa real) en 3 meses.

No es un curso teórico.
Está orientado a empleabilidad real y a construir evidencias (laboratorio + documentación + scripts + mini informes).

Al finalizar este programa podrás:

- Entender cómo funciona una infraestructura empresarial (sistemas, red, identidad, datos)
- Realizar reconocimiento y un mini pentest básico en un entorno controlado y autorizado
- Analizar logs y detectar actividad sospechosa con mentalidad SOC
- Comprender fundamentos de seguridad en Azure, AWS y Google Cloud (modelo de responsabilidad compartida)
- Automatizar tareas repetitivas con Python, PowerShell y Bash
- Usar IA (vibecoding) de forma responsable (generar, revisar, probar, documentar)
- Documentar hallazgos e investigaciones con estructura profesional
- Conectar ataque ↔ evidencias ↔ detecciones ↔ mitigaciones con MITRE ATT&CK (enfoque Purple Team)

---

# 🧭 Estructura del Programa

## 🔹 Fase 0 – Entorno de laboratorio (Semana 0)

Construcción del laboratorio personal (base de todo el roadmap).

- Instalación de VirtualBox o VMware
- Creación de VMs:
  - Kali Linux (atacante/lab)
  - Windows 10/11 (objetivo/lab)
  - Ubuntu Server (servicios + logs/lab)
- Configuración de red:
  - NAT vs Host-Only
  - IPs, gateway, DNS (entender qué hace cada cosa)
- Uso de snapshots (antes y después de cada práctica)
- “Reglas del lab”:
  - Todo se hace en entorno controlado
  - No se prueban técnicas fuera del lab sin permiso
- Creación de cuentas trial en:
  - Azure
  - AWS
  - Google Cloud
- Exploración inicial de los portales cloud (sin necesidad de desplegar mucho todavía)

**Resultado:**  
Dispondrás de un entorno seguro y reproducible para practicar ofensiva y defensiva.

---

## 🔹 Fase 1 – Fundamentos Técnicos (Semanas 1–4)

### Semana 1 – Fundamentos de Ciberseguridad (base mental + vocabulario)
- Modelo CIA (Confidencialidad, Integridad, Disponibilidad) con ejemplos reales
- Amenazas comunes:
  - phishing, malware, ransomware, fuerza bruta, robo de credenciales, etc.
- Conceptos de riesgo: activo, amenaza, vulnerabilidad, impacto, probabilidad
- Ciclo de un ataque (visión general) y por qué existe defensa en capas
- Introducción a MITRE ATT&CK (tácticas y técnicas, sin memorizar IDs)
- Roles en ciberseguridad (SOC, pentest, ingeniería de seguridad, GRC)
- Ciclo de gestión de incidentes (detección → análisis → contención → remediación → lecciones)

**Laboratorio (sencillo):**
- Identificar “qué sería sospechoso” en un sistema (logs básicos / actividad anómala)
- Documentar un mini caso en Markdown

---

### Semana 2 – Redes en Profundidad (lo imprescindible para seguridad)
- Modelo OSI vs TCP/IP (qué hace cada capa “en cristiano”)
- Conceptos clave: IP, subnet, gateway, DNS, puertos, protocolos
- HTTP/HTTPS y TLS (qué protege y qué NO protege)
- Dispositivos y controles:
  - router, switch, firewall, proxy, WAF, IDS/IPS
- Tráfico normal vs tráfico sospechoso (patrones típicos: escaneos, picos, conexiones raras)
- Introducción a enumeración/escaneo controlado en lab (sin enfoque destructivo)

**Herramientas:**
- Wireshark / tcpdump (captura y lectura básica)
- nmap (descubrimiento y enumeración de servicios)

**Laboratorio:**
- Capturar tráfico entre máquinas del lab
- Identificar un escaneo y diferenciarlo de tráfico “normal”
- Documentar hallazgos y conclusiones

---

### Semana 3 – Linux y Windows orientado a Seguridad (operación y evidencias)
**Linux (Ubuntu/Kali):**
- Procesos, servicios, usuarios, permisos
- Logs clave:
  - `/var/log/auth.log` (autenticación, sudo, SSH)
- Endurecimiento básico:
  - conceptos de mínimos privilegios
  - prácticas seguras en SSH (sin entrar en “hardening extremo”)

**Windows:**
- Procesos, servicios y fundamentos del sistema
- Visor de eventos (qué es un evento, cómo buscar)
- PowerShell básico (comandos útiles y por qué también se usa en ataques)
- Concepto general de Active Directory (visión, sin profundizar aún)

**Laboratorio:**
- Generar eventos controlados y localizarlos en logs/eventos
- Documentar: qué pasó, dónde se vio, qué significaría en un entorno real

---

### Semana 4 – Datos, Almacenamiento e Identidad (la base de la mayoría de incidentes)
- Sistemas de archivos (visión comparada):
  - NTFS vs ext4 (permisos, evidencias, etc.)
- Backups y ransomware:
  - por qué el backup es defensa real
  - conceptos RPO / RTO con ejemplos
- Identidad y permisos (IAM):
  - usuarios, roles, privilegios, MFA (conceptos)
- Seguridad básica en Active Directory (visión conceptual):
  - cuentas, grupos, privilegios, riesgo de cuentas compartidas

**Laboratorio:**
- Simulación de pérdida de datos y “plan de recuperación”
- Documentar: impacto, medidas preventivas, y recuperación

---

## 🔹 Fase 2 – Cloud, Automatización y Mentalidad Ofensiva (Semanas 5–8)

### Semana 5 – Fundamentos de Cloud y Seguridad en la Nube ☁️🔐
- Qué es cloud computing (visión práctica)
- Modelos: IaaS, PaaS, SaaS (y por qué cambia la responsabilidad)
- Modelo de responsabilidad compartida (concepto clave)
- Componentes comunes:
  - VMs (Azure VM / AWS EC2 / GCP Compute Engine)
  - Redes virtuales (VNet / VPC)
  - Security Groups / NSG (control de tráfico)
  - Almacenamiento (Blob/S3/Buckets)
- Errores típicos:
  - puertos administrativos expuestos a Internet
  - buckets públicos
  - credenciales expuestas
  - permisos excesivos (IAM)

**Laboratorio:**
- Explorar un portal cloud (Azure/AWS/GCP)
- Diseñar o desplegar una VM pequeña (si se puede)
- Revisar exposición de puertos y reflexionar sobre riesgo
- Documentar todo con capturas o descripciones

---

### Semana 6 – Automatización y Scripting aplicado a Ciberseguridad 🤖🔐
- Por qué automatizar (SOC y pentest trabajan con volumen)
- Bash:
  - grep, wc, pipelines para análisis rápido de logs
- Python:
  - lectura de ficheros, filtros por patrón, conteos
  - scripts simples orientados a “una tarea útil”
- PowerShell:
  - listar eventos, filtrar procesos, extraer datos de logs
- IA / vibecoding responsable:
  - generar script → revisar → ajustar → probar en lab → documentar
  - nunca ejecutar sin entender lo mínimo

**Laboratorio:**
- Script Bash para contar intentos fallidos (auth.log copia)
- Script Python para filtrar y contar patrones
- Script PowerShell para listar eventos y detectar patrones simples
- Documentación de scripts y outputs

---

### Semana 7 – Fundamentos de Hacking Ético 🕵️‍♂️💻
- Qué es hacking ético (legalidad, autorización, alcance)
- Fases de un pentest (visión junior):
  - Reconocimiento (en lab)
  - Escaneo y enumeración
  - Explotación (conceptual, sin necesidad de CVEs complejas)
  - Post-explotación (conceptual)
  - Informe
- Uso de Kali para reconocimiento estructurado
- Pensamiento ofensivo con objetivo defensivo (Purple mindset inicial)

**Herramientas (nivel básico):**
- nmap (escaneo y enumeración)
- curl/wget (banners, cabeceras)

**Laboratorio:**
- Mini pentest interno del lab (Host-Only)
- Output: scope, resultados nmap, enumeración básica, threat modeling simple y mini informe

---

### Semana 8 – Blue Team, SOC y Detección Básica 🛡️📊
- Qué es un SOC y tipos de SOC (interno / MSSP / híbrido)
- Evento vs alerta vs incidente (y por qué importa)
- Conceptos base:
  - SIEM (qué hace, por qué es “el centro”)
  - EDR / IDS/IPS / WAF como fuentes de señales
  - IOC (qué es y cómo se usa)
  - playbooks (por qué estandarizan)
- Flujo SOC:
  - evento → alerta → triage → contexto → clasificación → respuesta

**Laboratorio:**
- Simular:
  - intentos repetidos de login SSH en Linux
  - evento “sospechoso” de PowerShell en Windows
  - detección por IOC inventado con script
- Triage: clasificar y proponer acciones (como analista junior)

---

## 🔹 Fase 3 – Nivel Intermedio / Enfoque Purple (Semanas 9–12)

### Semana 9 – Seguridad en Aplicaciones Web y OWASP Top 10 (Nivel Básico) 🌐🔍
- Cómo funciona HTTP:
  - petición/respuesta, métodos, cabeceras, cookies, códigos
- Parámetros GET/POST, formularios y sesiones
- OWASP Top 10 (visión práctica, sin memorizar):
  - inyección (conceptual), XSS, control de acceso, datos sensibles, configuración insegura
- Herramientas:
  - DevTools (Network, Storage)
  - Burp Suite Community (proxy e interceptación básica)

**Laboratorio:**
- Montar una web simple de prueba (o equivalente en lab)
- Ver peticiones con DevTools
- Interceptar y modificar parámetros con Burp (solo lab)
- Demostración controlada de XSS (solo lab) + mitigación
- Mini “modelado de riesgos” estilo OWASP

---

### Semana 10 – Threat Hunting Básico y Correlación de Eventos 🕵️‍♂️📈
- Qué es Threat Hunting (proactivo vs reactivo)
- Ciclo:
  - hipótesis → búsqueda → análisis → conclusiones → mejoras
- Fuentes de datos del lab:
  - Linux auth.log (SSH/sudo)
  - Windows eventos PowerShell
  - logs de pruebas
- Hunts prácticos:
  - SSH: agrupar intentos fallidos por IP/usuario
  - PowerShell: detectar flags sospechosos (-NoProfile / WindowStyle Hidden)
- Convertir hunts en detecciones:
  - umbrales, ventanas temporales, severidad, campos necesarios

**Laboratorio:**
- 3 hipótesis
- 2 hunts (Linux + Windows)
- Propuesta de reglas tipo SIEM
- Mini informe de hunting

---

### Semana 11 – Enfoque Purple Team y MITRE ATT&CK aplicado 🟣🧩
- Qué es Purple Team (colaboración ofensiva/defensiva)
- MITRE ATT&CK como lenguaje común:
  - tácticas (el “para qué”)
  - técnicas (el “cómo”)
- Elegir un escenario (simple y repetible) en lab:
  - SSH (fallos → éxito)
  - PowerShell “sospechoso”
  - Web (parámetros/XSS controlado)
- Conectar:
  - pasos ofensivos → evidencias → mapeo MITRE → detecciones → mejoras

**Laboratorio:**
- Definir escenario
- Ejecutar pasos “Red”
- Recopilar evidencias “Blue”
- Mapear a MITRE
- Proponer detecciones y hardening
- Informe Purple Team final

---

### Semana 12 – Proyecto Final, Portfolio y Preparación para Empleo Real 🎓💼
- Repaso global estructurado (capas de conocimiento)
- Proyecto final integrador (escenario ataque–defensa):
  - parte ofensiva controlada (lab)
  - evidencias defensivas + hunting
  - automatización (al menos 1 script útil)
  - mapeo MITRE + detecciones + mitigaciones
  - informe final profesional
- Portfolio en GitHub:
  - estructura de carpetas por fases
  - README orientado a reclutadores (qué demuestra el repo y cómo navegarlo)
- Preparación de entrevistas (junior):
  - guiones para explicar lab, metodología, proyecto final y aprendizajes
  - “qué me diferencia como junior” (evidencias reales, documentación y visión Purple)

**Entrega:**
- Proyecto final documentado + scripts + anexos de logs
- README y estructura del repo lista para enseñar
- Documento de preparación para entrevista (preguntas y respuestas)

---

# 🛠 Herramientas Utilizadas

Gratuitas y utilizadas en industria:

- Wireshark
- nmap
- Burp Suite
- Metasploit
- Wazuh
- Elastic
- Splunk (conceptos)
- Microsoft Defender (conceptos)
- Azure / AWS / GCP Free Tier

---

# 📂 Estructura del Repositorio

```
Roadmap-Ciberseguridad-3-Meses/
│
├── README.md
├── 00_Entorno_Laboratorio/
├── 01_Fundamentos/
├── 02_Ofensiva_y_Defensiva/
├── 03_Purple_Team/
├── Scripts/
├── Recursos/
└── Proyecto_Final/
```

---

# 🎯 Resultado Esperado

Tras 3 meses:

- Pensarás como atacante y defensor
- Entenderás infraestructuras empresariales
- Analizarás logs con criterio
- Usarás automatización de forma profesional
- Tendrás portfolio técnico en GitHub
- Estarás preparado para entrevistas técnicas junior

---

# ⚠️ Norma Fundamental

Todo debe probarse únicamente en entornos controlados.

Nunca realizar pruebas en sistemas reales sin la debida autorización expresa.

Este roadmap tiene fines exclusivamente educativos y de desarrollo profesional.

El autor no se hace responsable del uso indebido del contenido aquí expuesto ni de las acciones que los usuarios puedan realizar a partir de la información proporcionada. Cada lector es responsable de cumplir con la legislación vigente y las normas aplicables en su país o entorno profesional.

---

# 🚀 Comienza por:

[00_Entorno_Laboratorio](00_Entorno_Laboratorio.md)

Sin laboratorio no hay aprendizaje real.

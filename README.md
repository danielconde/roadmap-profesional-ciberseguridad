# Roadmap Profesional de Ciberseguridad en 3 Meses 🚀

Autor: Daniel Conde → https://www.linkedin.com/in/daniconde/

Nivel: Principiante → Intermedio

Duración: 12 semanas

Enfoque: Blue Team | Pentesting | Purple Team | Cloud | Automatización

Objetivo: Preparación para empleo real

---

## 📌 Visión General

Este roadmap está diseñado para transformar a una persona sin experiencia previa en un perfil junior sólido con capacidades intermedias operativas en 3 meses.

No es un curso teórico.
Está orientado a empleabilidad real.

Al finalizar este programa podrás:

- Entender cómo funciona una infraestructura empresarial
- Realizar un pentest básico en entorno controlado
- Analizar logs y detectar actividad maliciosa
- Comprender los fundamentos de seguridad en Azure, AWS y Google Cloud
- Utilizar automatización con Python, PowerShell y Bash
- Usar IA (vibecoding) de forma responsable
- Documentar incidentes como un profesional
- Simular escenarios de ataque y defensa

---

# 🧭 Estructura del Programa

## 🔹 Fase 0 – Entorno de laboratorio (Semana 0)

Construcción de laboratorio personal.

- Instalación de VirtualBox o VMware
- Creación de:
  - Kali Linux
  - Windows 10/11
  - Ubuntu Server
- Configuración de red NAT vs Host-Only
- Uso de snapshots
- Seguridad del laboratorio
- Creación de cuentas trial en:
  - Azure
  - AWS
  - Google Cloud
- Despliegue de una VM básica en la nube

Resultado:
Dispondrás de un entorno seguro de ataque y defensa.

---

## 🔹 Fase 1 – Fundamentos Técnicos (Semanas 1–4)

### Semana 1 – Fundamentos de Ciberseguridad
- Triada CIA
- Panorama actual de amenazas
- Ciclo de vida de un ataque
- Introducción a MITRE ATT&CK
- Roles profesionales en ciberseguridad
- Ciclo de gestión de incidentes

Laboratorio:
Análisis de un incidente simplificado.

---

### Semana 2 – Redes en Profundidad
- Modelo OSI vs TCP/IP
- DNS, HTTP/HTTPS, TLS
- Firewalls, WAF, IDS/IPS
- Análisis de tráfico

Herramientas:
- Wireshark
- tcpdump
- nmap
- Burp Suite

Laboratorio:
Detectar actividad de escaneo y tráfico sospechoso.

---

### Semana 3 – Linux y Windows orientado a Seguridad

Linux:
- Procesos
- Permisos
- Logs
- Endurecimiento básico SSH

Windows:
- Arquitectura básica
- Visor de eventos
- PowerShell básico
- Introducción a Active Directory

Laboratorio:
Búsqueda de persistencia básica y revisión de logs.

---

### Semana 4 – Datos, Almacenamiento e Identidad
- NTFS vs ext4
- Copias de seguridad y ransomware
- RPO y RTO
- Gestión de identidad (IAM)
- Seguridad básica en Active Directory

Laboratorio:
Simulación de pérdida de datos y plan de recuperación.

---

## 🔹 Fase 2 – Cloud, Automatización y Mentalidad Ofensiva (Semanas 5–8)

### Semana 5 – Fundamentos de Seguridad en la Nube

Azure:
- Resource Groups
- Máquinas virtuales
- Network Security Groups
- Concepto Defender for Cloud

AWS:
- IAM
- EC2
- Security Groups
- CloudTrail

Google Cloud:
- IAM
- Compute Engine
- Logging

Concepto clave:
Modelo de responsabilidad compartida.

Laboratorio:
Desplegar una VM y analizar exposición de puertos.

---

### Semana 6 – Automatización y Scripting en Seguridad

Python:
- Lectura y parseo de logs
- Automatización básica
- Peticiones HTTP

PowerShell:
- Consulta de procesos
- Extracción de eventos

Bash:
- Automatización de análisis en Linux

IA y Vibecoding:
- Generar scripts con IA
- Comprender el código antes de ejecutarlo
- Probar siempre en entorno controlado
- No confiar ciegamente en código generado

Objetivo:
Aprender a usar la IA como acelerador profesional, no como sustituto del conocimiento técnico.

---

### Semana 7 – Fundamentos de Hacking Ético
- Reconocimiento
- Escaneo
- Enumeración
- Explotación
- Escalada de privilegios
- Post-explotación

Herramientas:
- nmap
- Burp Suite
- Metasploit
- LinPEAS / WinPEAS

Laboratorio:
Pentest completo en máquina vulnerable.

---

### Semana 8 – Blue Team y Detección
- Funcionamiento de un SOC real
- Alertas vs Incidentes
- Indicadores de compromiso (IOC)
- Playbooks
- Introducción a Threat Hunting
- Conceptos de SIEM

Herramientas:
- Wazuh
- Elastic
- Splunk (conceptos)
- Microsoft Defender (conceptos)

Laboratorio:
Detectar el ataque realizado en la semana anterior.

---

## 🔹 Fase 3 – Nivel Intermedio / Enfoque Purple (Semanas 9–12)

### Semana 9 – Seguridad en Active Directory
- Conceptos de Kerberoasting
- Pass-the-Hash
- Movimiento lateral
- Escalada de privilegios en dominio (visión general)

---

### Semana 10 – Simulación de Ataque Completo
Escenario:
- Acceso inicial
- Persistencia
- Movimiento lateral
- Exfiltración controlada

---

### Semana 11 – Respuesta ante Incidentes
- Triage
- Contención
- Erradicación
- Recuperación
- Elaboración de informe profesional

Entrega:
Informe técnico completo.

---

### Semana 12 – Proyecto Final

Escenario empresarial ficticio.

El alumno debe:
- Detectar
- Analizar
- Documentar
- Proponer mitigaciones

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

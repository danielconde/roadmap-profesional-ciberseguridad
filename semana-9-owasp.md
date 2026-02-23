# Semana 9 – Seguridad en Aplicaciones Web y OWASP Top 10 (Nivel Básico) 🌐🔍

## 0. Introducción a la semana

Hasta ahora ya dominas:

- Fundamentos de ciberseguridad, redes y sistemas.
- Cloud y modelo de responsabilidad compartida.
- Automatización básica (scripts).
- Hacking ético (visión general).
- Blue Team y SOC (detección, eventos, alertas).

En esta Semana 9 vamos a entrar en uno de los campos más demandados en ciberseguridad:

> La seguridad de aplicaciones web (WebApp Security), conectada con el OWASP Top 10.

La mayoría de servicios que usa una empresa pasan por aplicaciones web:

- Portales internos.
- APIs.
- E-commerce.
- Intranets.
- Backoffices de administración.

Y muchas brechas de seguridad vienen de lo mismo una y otra vez:

- Validaciones deficientes de entrada.
- Gestión insegura de sesiones.
- Fallos de autenticación y control de acceso.
- Configuraciones por defecto, paneles expuestos, errores demasiado verbosos.
- Dependencias desactualizadas o mal integradas.

El objetivo de esta semana NO es que seas pentester web completo, sino que:

- Entiendas cómo funciona una aplicación web por dentro (HTTP, parámetros, sesiones).
- Tengas una primera visión de las vulnerabilidades más comunes (OWASP Top 10).
- Aprendas a usar herramientas básicas (navegador, DevTools, Burp Suite Community).
- Sepas interpretar las vulnerabilidades desde un enfoque tanto Red como Blue Team:
  - Red: "cómo se explota y por qué funciona".
  - Blue: "cómo se detecta, cómo se mitiga y qué evidencias deja".

**Prompt IA:**  
"Actúa como instructor de WebApp Security para un perfil junior. Explica por qué la seguridad web es crítica hoy y cómo se conecta con SOC (logs, WAF, SIEM) y con pentesting. Incluye 10 incidentes típicos por mala configuración o validación pobre (sin dar pasos de explotación), y para cada uno indica: impacto en CIA, evidencia que dejaría, y mitigación recomendada."

---

## 1. Objetivos de aprendizaje de la Semana 9

Al finalizar esta semana deberías ser capaz de:

1. Explicar de forma clara cómo funciona HTTP a nivel de petición / respuesta, incluyendo:
   - Métodos (GET, POST, PUT, DELETE a nivel conceptual).
   - Parámetros (querystring, body).
   - Cabeceras (headers).
   - Cookies.
2. Entender qué es una aplicación web dinámica y qué partes la componen (cliente, servidor, base de datos).
3. Describir, a alto nivel, algunas categorías importantes del OWASP Top 10 (sin memorizarlas todas).
4. Comprender conceptos básicos de:
   - Inyección (ej. SQLi a nivel conceptual).
   - XSS (Cross-Site Scripting).
   - Fallos de autenticación y control de acceso.
   - Exposición de datos sensibles.
5. Usar el navegador y las herramientas de desarrollo para:
   - Ver peticiones HTTP.
   - Ver parámetros, cabeceras y respuestas.
6. Configurar Burp Suite Community de forma muy básica para interceptar tráfico entre navegador y una aplicación de tu laboratorio.
7. Realizar pequeños laboratorios controlados para:
   - Ver cómo viajan los parámetros.
   - Probar cambios simples en parámetros (sin romper nada).
8. Documentar tus observaciones en tu repositorio con mentalidad profesional:
   - Qué viste.
   - Por qué importa.
   - Qué riesgo podría existir.
   - Qué mitigación aplicarías.

**Prompt IA:**  
"Actúa como evaluador. Crea un test de 15 preguntas para la Semana 9: 5 de HTTP, 5 de sesiones/cookies, 5 de OWASP (alto nivel). Añade 3 mini casos con peticiones HTTP y pide al alumno que identifique: parámetros manipulables, posible riesgo y mitigación. Devuelve respuestas esperadas y errores típicos del junior."

---

## 2. Cómo funciona una aplicación web (visión sencilla)

### 2.1. Arquitectura básica

Imagina una aplicación web típica:

- Cliente: navegador (Chrome, Firefox, etc.) que hace peticiones HTTP/HTTPS.
- Servidor web: Apache, Nginx, IIS… recibe la petición y la enruta.
- Aplicación: código (PHP, Python, .NET, Node.js…) que aplica lógica de negocio.
- Base de datos: MySQL, PostgreSQL, SQL Server… guarda usuarios, pedidos, etc.

Flujo simplificado:

1. El usuario escribe una URL o pulsa un botón.
2. El navegador envía una petición HTTP al servidor.
3. El servidor ejecuta código, consulta base de datos o servicios, y genera una respuesta.
4. El navegador renderiza la respuesta (HTML, CSS, JS) o consume un JSON (API).

Punto clave de seguridad:

- Cada salto del flujo es una oportunidad de validación o de fallo.
- El atacante intenta "romper suposiciones":
  - "Este parámetro siempre será un número".
  - "Esta ruta solo la verá un usuario autenticado".
  - "Este token no se puede adivinar".
  - "Nadie verá este endpoint interno".

### 2.2. HTTP: petición / respuesta

Petición HTTP típica:

- Método: GET / POST / otros.
- Ruta: /login, /productos, /buscar, /api/v1/users.
- Cabeceras: información adicional (User-Agent, cookies, Accept, Content-Type).
- Cuerpo (body): datos enviados (por ejemplo, usuario y contraseña en un POST).

Respuesta HTTP:

- Código de estado: 200 OK, 302 Found, 401 Unauthorized, 403 Forbidden, 404 Not Found, 500 Internal Server Error.
- Cabeceras: tipo de contenido, cookies (Set-Cookie), cache, seguridad (CSP, HSTS, etc.).
- Cuerpo: HTML, JSON, etc.

Si entiendes estas piezas, puedes:

- Ver por dónde pasan los datos.
- Identificar "puntos de entrada" (inputs) y "puntos de salida" (outputs).
- Comprender qué partes podría manipular un atacante.
- Pensar cómo lo detectarías (logs de aplicación, WAF, trazas).

**Prompt IA:**  
"Actúa como profesor. Explica HTTP (request/response) con un ejemplo completo: petición GET con querystring y petición POST con body. Para cada una, desglosa método, URL, headers, cookies y status codes. Añade una sección: 'qué miraría un atacante' y 'qué miraría un analista SOC' para esa misma petición."

---

## 3. Parámetros, formularios y sesiones

### 3.1. Parámetros en URL (GET)

Ejemplo:

    https://miweb.local/buscar?producto=teclado&categoria=perifericos

- "producto" y "categoria" son parámetros.
- Viajan en la URL (fáciles de ver, copiar y modificar).

Qué implica:

- Son perfectos para búsquedas y filtros.
- Pero también son un punto típico de manipulación:
  - Valores inesperados.
  - Saltos de página.
  - IDs de recursos.
  - Parámetros ocultos por front-end pero presentes en la URL.

### 3.2. Parámetros en POST

Cuando envías un formulario (por ejemplo, login):

- Los datos suelen ir en el cuerpo de la petición (no visibles en la URL).
- Pero siguen siendo modificables con DevTools o con un proxy.

Ejemplo de body:

    username=juan&password=123456

Punto clave:

- "No visible" no significa "seguro".
- Seguridad real depende de validación en servidor y controles de autenticación/autorización.

### 3.3. Sesiones y cookies

Para saber quién eres entre peticiones, la app usa:

- Cookies: fragmentos de información que el servidor envía al navegador y el navegador reenvía en cada petición.
- IDs de sesión: identifican tu sesión de usuario (a menudo en una cookie).

Ejemplo:

- Cookie "SESSIONID=abc123xyz".
- Si alguien roba esa cookie y no hay protecciones adicionales, podría suplantarte (session hijacking).

Conceptos básicos que debes manejar:

- Autenticación: "quién eres" (login).
- Autorización: "qué puedes hacer" (permisos).
- Sesión: "mantener estado" (seguir logueado).
- Token: "credencial" temporal (mucho en APIs modernas, JWT, etc. a nivel conceptual).

Buenas prácticas (alto nivel):

- Cookies con flags de seguridad (HttpOnly, Secure, SameSite).
- Rotación de sesión tras login.
- Expiración razonable y logout efectivo.
- MFA cuando el riesgo lo exige.

**Prompt IA:**  
"Actúa como instructor. Explica parámetros GET vs POST y sesiones/cookies con ejemplos claros. Incluye qué datos nunca deberían viajar en URL en una app real, y qué flags de cookies son recomendables y por qué. Añade 5 señales de riesgo comunes en sesiones (tokens largos en URL, sesiones sin expiración, etc.) y mitigaciones."

---

## 4. OWASP Top 10 – Introducción

OWASP (Open Web Application Security Project) mantiene una lista de categorías de vulnerabilidades críticas en aplicaciones web: el "OWASP Top 10".

No necesitas memorizarlo todo ahora, pero sí entender:

- Qué significa cada categoría a nivel "historia" (qué error comete la app).
- Qué impacto suele tener.
- Qué mitigación típica existe.
- Qué evidencia podrías ver desde Blue Team (logs, WAF, errores).

Nota importante:

- OWASP Top 10 cambia con el tiempo, pero las ideas base se repiten.
- En la práctica, una sola brecha suele combinar varias categorías (ej. auth débil + control de acceso roto + mala configuración).

### 4.1. Fallos de control de acceso / autenticación

Problemas típicos:

- Permitir a usuarios no autenticados acceder a recursos privados.
- Permitir a usuarios normales realizar acciones de administrador.
- No invalidar sesiones adecuadamente tras logout.
- Recuperación de contraseña débil (preguntas triviales, tokens reutilizables).
- No aplicar rate limiting o bloqueo tras intentos fallidos.

Impacto:

- Acceso no autorizado a datos.
- Toma de control de cuentas.
- Escalado de privilegios a nivel de aplicación.

Visión Blue Team (qué verías):

- Accesos a rutas sensibles sin login.
- Cambios de rol o acciones administrativas desde usuarios no esperables.
- Picos de intentos de login, y errores 401/403.

### 4.2. Inyección (ej. SQL Injection – SQLi)

Ocurre cuando:

- La aplicación construye consultas con datos del usuario sin validarlos o sin usar mecanismos seguros.
- El atacante intenta "inyectar" lógica dentro de lo que debería ser un dato.

Ejemplo conceptual de consulta (solo para entender el riesgo):

    SELECT * FROM usuarios WHERE usuario = 'juan' AND password = '123456';

Si una app concatena datos sin protección, un atacante intentaría forzar comportamientos inesperados.

Impacto:

- Lectura no autorizada de datos.
- Modificación o borrado de información.
- En casos graves, ejecución de acciones no previstas por la aplicación.

Mitigación (alto nivel):

- Consultas preparadas / parametrizadas.
- Validación de entrada con tipo y formato.
- Principio de mínimo privilegio en cuentas de base de datos.

Visión Blue Team:

- Errores repetidos 500 en endpoints de búsqueda/login.
- Patrones anómalos en parámetros (longitudes raras, caracteres especiales).
- Alertas de WAF por firmas de inyección.

### 4.3. XSS (Cross-Site Scripting)

Ocurre cuando:

- La aplicación muestra datos que vienen del usuario sin "sanitizar" o sin codificar salida.
- El atacante intenta que el navegador ejecute JavaScript no deseado.

Ejemplo conceptual:

- Un campo de comentarios muestra exactamente lo que envía el usuario, sin tratarlo.
- Eso permite que se ejecute contenido activo en el navegador.

Impacto:

- Robo de cookies o tokens (según configuración).
- Redirecciones, manipulación del DOM, phishing dentro del sitio.
- Acciones en nombre del usuario si se combinan condiciones.

Mitigación (alto nivel):

- Output encoding (codificar salida según contexto HTML/JS/URL).
- Sanitización de entrada cuando aplica.
- Content Security Policy (CSP) bien diseñada.
- Cookies HttpOnly para reducir impacto sobre sesión.

Visión Blue Team:

- Peticiones con payloads raros en campos de texto.
- Alertas WAF por patrones XSS.
- Reportes de CSP (si se habilitan) indicando intentos de ejecutar scripts.

### 4.4. Exposición de datos sensibles

Ejemplos:

- Contraseñas guardadas en texto plano.
- Datos personales visibles en URLs o en respuestas JSON excesivas.
- Ficheros de backup accesibles públicamente.
- Logs que incluyen tokens, cookies, datos personales.

Impacto:

- Robo de información crítica.
- Incumplimiento de normativas (ej. GDPR).
- Daño reputacional y legal.

Mitigación (alto nivel):

- Cifrado en tránsito (HTTPS) y en reposo cuando aplica.
- Minimización de datos (no devolver de más en APIs).
- Gestión de secretos y rotación.
- Control de acceso y auditoría de accesos.

### 4.5. Configuración insegura

Ejemplos:

- Páginas de administración accesibles sin protección.
- Directory listing habilitado.
- Archivos de configuración o backups accesibles (.bak, .old).
- Mensajes de error demasiado detallados (stack traces, rutas internas).
- CORS mal configurado en APIs.

Impacto:

- Exposición de información interna que facilita ataques.
- Accesos indebidos por endpoints olvidados.
- Aumento de superficie de ataque.

Mitigación (alto nivel):

- Hardening y revisión de configuraciones.
- Deshabilitar módulos y rutas no usadas.
- Gestión de errores segura (mensajes genéricos hacia fuera).
- Revisiones periódicas y escaneos de configuración.

**Prompt IA:**  
"Actúa como instructor OWASP. Resume 6 categorías del OWASP Top 10 a nivel básico: qué es, ejemplo típico (sin explotación), impacto y mitigación. Añade para cada categoría: 'qué vería un WAF', 'qué vería un SIEM' y 'qué vería un desarrollador en logs'. Devuélvelo en formato de tabla en Markdown."

---

## 5. Herramientas básicas para analizar aplicaciones web

### 5.1. Navegador + DevTools

En tu navegador (Chrome/Firefox):

- Pestaña "Network":
  - Ves todas las peticiones HTTP.
  - Métodos, URLs, cabeceras, parámetros, respuestas, tiempos.
  - Puedes ver el body del POST y respuestas JSON.

- Pestaña "Storage / Application":
  - Cookies.
  - LocalStorage / SessionStorage.
  - Cache y algunos tokens (depende de la app).

Esto ya te permite:

- Entender qué se envía exactamente al servidor.
- Ver parámetros y respuestas sin herramientas extra.
- Identificar endpoints de API.
- Ver redirecciones 302 y cadenas de autenticación.

Consejo práctico:

- Aprende a distinguir:
  - Parámetros del cliente (front-end) vs validación real (servidor).
  - Cookies de sesión vs cookies de tracking.
  - Respuestas 401/403 vs 404 (y qué significa cada una).

### 5.2. Burp Suite Community

Burp Suite Community Edition permite:

- Actuar como proxy entre el navegador y el servidor.
- Interceptar peticiones.
- Modificar parámetros antes de enviarlos.
- Repetir peticiones (Repeater).
- Guardar historial y estudiar el comportamiento.

Flujo básico:

1. Configuras el navegador para usar proxy en "127.0.0.1:8080".
2. Abres Burp → "Proxy" → "Intercept on".
3. Navegas por la web objetivo (en tu lab).
4. Burp captura la petición.
5. Puedes editar y reenviar.

Importante (buen hábito):

- Trabaja siempre en laboratorio o con autorización.
- Documenta cambios y resultados: esto es lo que convierte "trasteo" en aprendizaje profesional.

**Prompt IA:**  
"Actúa como tutor práctico. Explica cómo usar DevTools y Burp a nivel básico para entender una aplicación: qué mirar en Network, cómo identificar parámetros, cookies y endpoints, y cómo interceptar una petición y modificar un campo. Incluye una checklist de 12 puntos para 'primera inspección' de una web."

---

## 6. Laboratorio de la Semana 9

Idealmente, tendrás una pequeña aplicación web en tu Ubuntu (Apache + scripts de prueba) o una VM vulnerable tipo DVWA. Si no puedes montar DVWA todavía, con un par de páginas simples te vale para entender el flujo.

Objetivo del laboratorio:

- Ver "datos en movimiento" (HTTP).
- Aprender a observar y a modificar de forma controlada.
- Entender el riesgo cuando una app refleja entrada sin control.
- Practicar documentación: evidencia, hipótesis, mitigación.

**Prompt IA:**  
"Actúa como diseñador de laboratorio. Propón un lab mínimo de seguridad web en una VM Ubuntu: servidor web, página con formulario, endpoint que refleja input y un ejemplo seguro vs inseguro. Incluye cómo validar que todo funciona y qué evidencias capturar en cada ejercicio."

---

### 6.1. Ejercicio 1 – Ver peticiones y respuestas HTTP con DevTools

1. Asegúrate de tener un servidor web sencillito en Ubuntu, por ejemplo Apache (si no lo tienes):

    sudo apt update  
    sudo apt install apache2

2. Crea una página simple en "/var/www/html/test.html":

    <!-- /var/www/html/test.html -->
    <html>
    <body>
      <h1>Prueba de Seguridad Web</h1>
      <form action="/test_form.php" method="POST">
        Usuario: <input type="text" name="usuario" />
        <br>
        Mensaje: <input type="text" name="mensaje" />
        <br>
        <button type="submit">Enviar</button>
      </form>
    </body>
    </html>

3. Crea un "test_form.php" muy básico para ver parámetros (versión segura, con salida tratada):

    <?php
      $usuario = $_POST["usuario"];
      $mensaje = $_POST["mensaje"];
      echo "Hola " . htmlspecialchars($usuario) . ", has enviado: " . htmlspecialchars($mensaje);
    ?>

4. Desde tu máquina (Kali o el host), abre el navegador y accede a:

    http://IP_DE_UBUNTU/test.html

5. Abre las DevTools → pestaña "Network".
6. Rellena el formulario y envíalo.
7. Observa:
   - Petición POST a "/test_form.php".
   - Parámetros "usuario" y "mensaje" en el body.
   - Cabeceras "Content-Type" y cookies (si aparecen).
   - Respuesta que devuelve el servidor.

Crea:

"06-WebApp-OWASP-Semana_9_HTTP_DevTools.md"

Incluye:

- Capturas o descripciones de la petición.
- Explicación con tus palabras de:
  - Método, URL, parámetros, body.
  - Código de respuesta y cuerpo de la respuesta.
- Reflexión:
  - ¿Qué campos serían "sensibles" en una app real?
  - ¿Qué controles añadirías si fuera un login real?

**Prompt IA:**  
"Actúa como revisor. A partir de una captura textual de DevTools (método, URL, headers, body, status), redacta un análisis en Markdown con: resumen, qué datos viajan, qué podría manipularse, y mitigaciones básicas. Incluye un ejemplo de 'qué buscaría un SOC' en logs para esa misma petición."

---

### 6.2. Ejercicio 2 – Interceptar una petición con Burp Suite Community

1. Abre Burp Suite Community en tu Kali o en la máquina donde tengas el navegador.
2. Configura el navegador para que use el proxy de Burp:
   - Proxy HTTP: "127.0.0.1"
   - Puerto: "8080"
3. En Burp → pestaña "Proxy" → "Intercept is on".
4. Vuelve a enviar el formulario de "test.html".
5. Burp debería interceptar la petición.
6. Observa:
   - Cómo se ven los parámetros "usuario" y "mensaje".
   - Cabeceras de la petición.
   - Cookies si existen.
7. Prueba a:
   - Cambiar el valor de "usuario" o "mensaje" en Burp antes de enviarla al servidor.
   - Enviar la petición y ver cómo cambia la respuesta.

Crea:

"06-WebApp-OWASP-Semana_9_Burp_Intercept.md"

Incluye:

- Descripción del flujo (navegador → Burp → servidor).
- Qué cambios has hecho en la petición.
- Cómo ha cambiado la respuesta.
- Nota: esto no es "hackear", es aprender a ver el tráfico como lo ve un analista/pentester.

**Prompt IA:**  
"Actúa como instructor. Crea una guía paso a paso para interceptar una petición con Burp y explicar cada parte de la request (método, path, headers, body, cookies). Añade una sección de troubleshooting: 'no veo tráfico', 'se rompe HTTPS', 'no intercepta'."

---

### 6.3. Ejercicio 3 – Concepto de XSS en entorno controlado

Importante: Haz esto solo en tu laboratorio, en una página de prueba.

Objetivo del ejercicio:

- Entender la diferencia entre "recibir datos" y "renderizar datos".
- Entender por qué "sanitizar salida" (output encoding) es esencial.

1. Modifica "test_form.php" para que NO use "htmlspecialchars" (versión insegura solo para ver el riesgo):

    <?php
      $usuario = $_POST["usuario"];
      $mensaje = $_POST["mensaje"];
      echo "Hola " . $usuario . ", has enviado: " . $mensaje;
    ?>

2. Envía en el campo "mensaje" un contenido de prueba que muestre ejecución en el navegador.

3. Si el navegador muestra un comportamiento inesperado (por ejemplo, un popup), significa que el navegador ha interpretado contenido activo.

4. Reflexiona:
   - Esto es un ejemplo simple de XSS (reflejado en este caso, porque depende de esa respuesta).
   - En un caso real, el impacto puede ser mayor si afecta a usuarios autenticados o a páginas críticas.

Crea:

"06-WebApp-OWASP-Semana_9_XSS_Concepto.md"

Incluye:

- Pasos que has seguido.
- Captura o descripción del comportamiento.
- Reflexión sobre:
  - Por qué es peligroso.
  - Cómo se mitiga:
    - Volver a usar "htmlspecialchars" u otras medidas de output encoding.
    - Validación de entrada cuando aplica.
    - Content Security Policy (CSP).
    - Cookies con "HttpOnly" para reducir impacto sobre sesión.

Después del ejercicio, vuelve a poner "htmlspecialchars" para dejar tu página de prueba en modo seguro.

**Prompt IA:**  
"Actúa como formador. Explica XSS a nivel conceptual con ejemplo de 'entrada del usuario que se refleja en HTML'. Describe la diferencia entre XSS reflejado y almacenado sin dar pasos ofensivos. Añade mitigaciones: output encoding, validación, CSP y flags de cookies. Termina con una checklist de revisión para developers."

---

### 6.4. Ejercicio 4 – Mini "modelado OWASP" de tu aplicación de prueba

Ahora que has visto:

- Cómo viajan parámetros.
- Cómo puedes interceptarlos y modificarlos.
- Cómo sería un XSS sencillo si no se trata la salida.

Crea:

"06-WebApp-OWASP-Semana_9_Modelado_Riesgos.md"

Para la aplicación de prueba ("test.html" + "test_form.php"), responde:

1. "Datos que maneja"
   - ¿Qué información recibe desde el usuario?
   - ¿Qué podría añadirse en una app real (emails, teléfonos, tokens, contraseñas)?
   - ¿Qué datos nunca deberían imprimirse tal cual en una respuesta?

2. "Superficie de ataque"
   - Formularios (parámetros).
   - Cookies y sesión.
   - Rutas accesibles.
   - Cabeceras relevantes (Host, Origin, Referer a nivel conceptual).

3. "Posibles vulnerabilidades (alto nivel)"
   - XSS (si no se sanitiza salida).
   - Inyección (si concatenaras parámetros en consultas sin protección).
   - Fallos de control de acceso (si un usuario accede a recursos de otro).
   - Configuración insegura (errores verbosos, rutas de admin expuestas).

4. "Medidas de mitigación básicas"
   - Validar entrada en servidor (tipo, formato, longitud).
   - Sanitizar salida (output encoding).
   - Usar consultas preparadas (parametrizadas).
   - Manejo seguro de errores.
   - Principio de mínimo privilegio (en DB y en roles de app).
   - Logging útil (sin filtrar datos sensibles en logs).

Este ejercicio te pone en modo "consultor": no solo ves el fallo, sino el riesgo y la solución.

**Prompt IA:**  
"Actúa como consultor AppSec. Dada una app con formulario que refleja entrada, crea un mini threat model en Markdown: activos, actores, puntos de entrada, riesgos alineados con OWASP, evidencias esperables, y mitigaciones priorizadas. Incluye un apartado 'Quick wins' y otro 'mejoras estructurales'."

---

## 7. Entregables de la Semana 9

Al finalizar la semana deberías tener:

1. "06-WebApp-OWASP-Semana_9_HTTP_DevTools.md"
   - Análisis de peticiones/respuestas HTTP con DevTools.
   - Explicación clara de cómo ves y entiendes los parámetros.
   - Observaciones de cookies y cabeceras relevantes (si aplica).

2. "06-WebApp-OWASP-Semana_9_Burp_Intercept.md"
   - Descripción del uso básico de Burp como proxy.
   - Ejemplo de modificación de parámetros y su efecto.
   - Qué aprendiste sobre "lo visible" y "lo controlable".

3. "06-WebApp-OWASP-Semana_9_XSS_Concepto.md"
   - Demostración controlada de un XSS simple.
   - Reflexión sobre el riesgo y mitigación.
   - Restauración del estado seguro tras el experimento.

4. "06-WebApp-OWASP-Semana_9_Modelado_Riesgos.md"
   - Análisis de riesgos de tu pequeña aplicación.
   - Referencias a OWASP (XSS, inyección, auth/access, misconfig) de forma razonada.
   - Mitigaciones priorizadas.

Opcional:

- Añadir una sección en tu "Lab_Setup_Report.md" llamada:
  - "Aplicaciones web de laboratorio", indicando:
    - Qué servidor web tienes.
    - Qué páginas de prueba usas.
    - Para qué las usas (formación, pruebas controladas).

Consejo de calidad:

- En cada entregable, termina con:
  - "Conclusión"
  - "Siguientes pasos"
  - "Qué evidencias guardé" (capturas o extractos mínimos)

**Prompt IA:**  
"Actúa como revisor de repositorio. Define un estándar de documentación para entregables de AppSec: secciones mínimas, cómo incluir evidencias sin exponer datos sensibles, y cómo redactar conclusiones. Incluye un ejemplo de plantilla Markdown para los ficheros de la Semana 9."

---

## 8. Checklist de cierre de la Semana 9

Marca como completado cuando:

- [ ] Puedes explicar el funcionamiento de HTTP (petición, respuesta, parámetros, códigos de estado).
- [ ] Entiendes qué es una aplicación web dinámica y sus componentes (cliente, servidor, aplicación, BD).
- [ ] Conoces, al menos a nivel conceptual:
  - Inyección (ej. SQLi).
  - XSS.
  - Fallos de autenticación/control de acceso.
  - Exposición de datos sensibles.
  - Configuración insegura.
- [ ] Has usado DevTools para ver parámetros y respuestas en una aplicación de tu laboratorio.
- [ ] Has usado Burp Suite Community para interceptar y modificar una petición.
- [ ] Has demostrado un XSS sencillo en un entorno de prueba y entiendes por qué es peligroso.
- [ ] Has realizado un mini modelado de riesgos tipo OWASP de tu pequeña aplicación de prueba.
- [ ] Tienes todos los ficheros de la Semana 9 creados y añadidos a tu repositorio.

Si has completado esta semana, ya no ves una aplicación web como "una web cualquiera", sino como:

- Un flujo de peticiones/respuestas.
- Con parámetros que pueden manipularse.
- Con sesiones/cookies que deben protegerse.
- Con puntos de entrada que, si no se controlan, pueden derivar en vulnerabilidades.

Esto te prepara muy bien para:

- Seguir profundizando en pentesting web.
- Entender alertas de WAF, logs de aplicaciones y detecciones en SIEM.
- Conectar el mundo OWASP con tu visión Blue/Red/Purple Team.

**Prompt IA:**  
"Actúa como evaluador final. Crea una checklist ampliada con criterios observables en el repo para validar la Semana 9 (qué capturas, qué explicaciones, qué conclusiones). Añade 8 preguntas de reflexión: qué detectaría un WAF, qué registraría un SIEM, qué mitigación priorizarías y por qué."

---

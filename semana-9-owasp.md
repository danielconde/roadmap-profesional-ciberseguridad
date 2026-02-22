# Semana 9 – Seguridad en Aplicaciones Web y OWASP Top 10 (Nivel Básico) 🌐🔍

## 0. Introducción a la semana

Hasta ahora ya dominas:

- Fundamentos de ciberseguridad, redes y sistemas.
- Cloud y modelo de responsabilidad compartida.
- Automatización básica (scripts).
- Hacking ético (visión general).
- Blue Team y SOC (detección, eventos, alertas).

En esta Semana 9 vamos a entrar en uno de los campos **más demandados** en ciberseguridad:

> La seguridad de aplicaciones web (WebApp Security), conectada con el OWASP Top 10.

La mayoría de servicios que usa una empresa pasan por aplicaciones web:

- Portales internos.
- APIs.
- E-commerce.
- Intranets.

Muchas brechas de seguridad vienen de:

- Validaciones deficientes.
- Gestión insegura de sesiones.
- Fallos de autenticación.
- Configuraciones por defecto.

El objetivo de esta semana NO es que seas pentester web completo, sino que:

- Entiendas **cómo funciona una aplicación web por dentro (HTTP, parámetros, sesiones)**.
- Tengas una **primera visión** de las vulnerabilidades más comunes (OWASP Top 10).
- Aprendas a usar **herramientas básicas** (navegador, DevTools, Burp Suite Community).
- Sepas interpretar las vulnerabilidades desde un enfoque tanto Red como Blue Team.

---

## 1. Objetivos de aprendizaje de la Semana 9

Al finalizar esta semana deberías ser capaz de:

1. Explicar de forma clara cómo funciona HTTP a nivel de **petición / respuesta**, incluyendo:
   - Métodos (GET, POST…).
   - Parámetros.
   - Cabeceras.
   - Cookies.
2. Entender qué es una **aplicación web dinámica** y qué partes la componen (cliente, servidor, base de datos).
3. Describir, a alto nivel, algunas categorías importantes del **OWASP Top 10** (sin memorizarlas todas).
4. Comprender conceptos básicos de:
   - Inyección (ej. SQLi a nivel conceptual).
   - XSS (Cross-Site Scripting).
   - Fallos de autenticación.
   - Exposición de datos sensibles.
5. Usar el navegador y las herramientas de desarrollo para:
   - Ver peticiones HTTP.
   - Ver parámetros y respuestas.
6. Configurar Burp Suite Community de forma muy básica para interceptar tráfico entre navegador y una aplicación de tu laboratorio.
7. Realizar pequeños laboratorios controlados para:
   - Ver cómo viajan los parámetros.
   - Probar cambios simples en parámetros (sin romper nada).
8. Documentar tus observaciones en tu repositorio.

---

## 2. Cómo funciona una aplicación web (visión sencilla)

### 2.1. Arquitectura básica

Imagina una aplicación web típica:

- **Cliente**: Navegador (Chrome, Firefox, etc.) → hace peticiones HTTP/HTTPS.
- **Servidor web**: Apache, Nginx, IIS… → recibe la petición y la pasa a la aplicación.
- **Aplicación**: Código (PHP, Python, .NET, Node.js…) → procesa datos, lógica de negocio.
- **Base de datos**: MySQL, PostgreSQL, SQL Server, etc. → guarda usuarios, pedidos, etc.

Flujo simplificado:

1. El usuario escribe una URL o pulsa un botón.
2. El navegador envía una petición HTTP al servidor.
3. El servidor ejecuta código, consulta base de datos, genera una respuesta.
4. El navegador muestra la página generada (HTML, CSS, JS).

### 2.2. HTTP: petición / respuesta

**Petición HTTP** típica:

- Método: `GET` / `POST` / otros.
- Ruta: `/login`, `/productos`, `/buscar`.
- Cabeceras: información adicional (User-Agent, cookies, etc.).
- Cuerpo (body): datos enviados (por ejemplo, usuario y contraseña).

**Respuesta HTTP**:

- Código de estado: `200 OK`, `302 Found`, `404 Not Found`, `500 Internal Server Error`.
- Cabeceras: tipo de contenido, cookies, etc.
- Cuerpo: HTML, JSON, etc.

Si entiendes estas piezas, puedes:

- Ver por dónde pasan los datos.
- Entender qué podría manipular un atacante.

---

## 3. Parámetros, formularios y sesiones

### 3.1. Parámetros en URL (GET)

Ejemplo:

```text
https://miweb.local/buscar?producto=teclado&categoria=perifericos
```

- `producto` y `categoria` son parámetros.
- Viajan en la URL (fáciles de ver y modificar).

Un atacante puede:

- Cambiar valores directamente en la barra de direcciones.
- Probar valores inesperados para ver cómo responde la app.

### 3.2. Parámetros en POST

Cuando envías un formulario (por ejemplo, login):

- Los datos suelen ir en el cuerpo de la petición (no visibles en la URL).
- Pero **siguen siendo modificables** (con herramientas como Burp o DevTools).

Ejemplo de cuerpo de un POST:

```text
username=juan&password=123456
```

### 3.3. Sesiones y cookies

Para saber quién eres entre peticiones, la app usa:

- **Cookies**: pequeños fragmentos de información que el servidor envía al navegador y que el navegador reenvía en cada petición.
- **IDs de sesión**: identifican tu sesión de usuario (a veces se guardan en cookies).

Ejemplo:

- Cookie `SESSIONID=abc123xyz`.
- Si alguien roba esa cookie, podría suplantarte (session hijacking).

---

## 4. OWASP Top 10 – Introducción

OWASP (Open Web Application Security Project) mantiene una lista de las categorías de vulnerabilidades más críticas en aplicaciones web, el **OWASP Top 10**.

No necesitas memorizarlo todo ahora, pero sí entender algunos conceptos clave.

### 4.1. Fallos de control de acceso / autenticación

Problemas típicos:

- Permitir a usuarios no autenticados acceder a recursos privados.
- Permitir a usuarios normales realizar acciones de administrador.
- No invalidar sesiones adecuadamente tras logout.
- Contraseñas débiles o sin protección adecuada.

Impacto:

- Accesos no autorizados a datos.
- Toma de control de cuentas.

### 4.2. Inyección (ej. SQL Injection – SQLi)

Ocurre cuando:

- La aplicación construye consultas (por ejemplo SQL) con datos del usuario sin validarlos.
- El atacante inyecta código en los parámetros.

Ejemplo conceptual (no código real):

```sql
SELECT * FROM usuarios WHERE usuario = 'juan' AND password = '123456';
```

Si la app no valida correctamente, un atacante podría introducir:

```text
usuario = 'juan' OR '1'='1'
```

y forzar la consulta a devolver resultados inesperados.

Impacto:

- Lectura no autorizada de datos.
- Modificación o borrado de información.
- Inclusión de comandos peligrosos en algunos escenarios.

### 4.3. XSS (Cross-Site Scripting)

Ocurre cuando:

- La aplicación muestra datos que vienen del usuario **sin limpiarlos** (sanitizar).
- El atacante puede inyectar código JavaScript en la página que ven otros usuarios.

Ejemplo simple:

- Un campo de comentarios permite meter `<script>alert('XSS')</script>` y el navegador lo ejecuta.

Impacto:

- Robo de cookies.
- Redirección a sitios maliciosos.
- Manipulación de contenido que ve el usuario.

### 4.4. Exposición de datos sensibles

Ejemplos:

- Contraseñas guardadas en texto plano.
- Datos personales visibles en URLs.
- Ficheros de backup accesibles públicamente.
- Bases de datos expuestas sin autenticación.

Impacto:

- Robo de información crítica.
- Incumplimiento de normativas (ej. GDPR).

### 4.5. Configuración insegura

Ejemplos:

- Páginas de administración accesibles sin protección.
- Directorios listados (`directory listing`).
- Archivos de configuración visibles (`.bak`, `.old`).
- Mensajes de error demasiado detallados (explican estructura interna).

---

## 5. Herramientas básicas para analizar aplicaciones web

### 5.1. Navegador + DevTools

En tu navegador (Chrome/Firefox):

- Pestaña **Network**:
  - Ves todas las peticiones HTTP.
  - Métodos, URLs, cabeceras, parámetros, respuestas.

- Pestaña **Storage / Application**:
  - Cookies.
  - LocalStorage/SessionStorage.

Esto ya te permite:

- Entender qué se envía exactamente al servidor.
- Ver parámetros y respuestas sin herramientas extra.

### 5.2. Burp Suite Community

Burp Suite Community Edition (que ya habías mencionado en otras conversaciones) permite:

- Actuar como **proxy** entre el navegador y el servidor.
- Interceptar peticiones.
- Modificar parámetros antes de enviarlos.
- Repetir peticiones (Repeater).

Flujo básico:

1. Configuras el navegador para que use el proxy de Burp (`127.0.0.1:8080` por defecto).
2. Abres Burp → pestaña “Proxy” → “Intercept on”.
3. Navegas por la web objetivo (en tu lab).
4. Burp captura la petición.
5. Puedes editar parámetros y enviarla.

**Importante:**  
Úsalo solo contra tu laboratorio o sistemas con autorización.

---

## 6. Laboratorio de la Semana 9

Idealmente, tendrás una pequeña aplicación web en tu Ubuntu (Apache + algún script de prueba) o una VM vulnerable tipo DVWA. Si no puedes montar DVWA todavía, con un par de scripts o páginas simple te vale para entender el flujo.

### 6.1. Ejercicio 1 – Ver peticiones y respuestas HTTP con DevTools

1. Asegúrate de tener un servidor web sencillito en Ubuntu, por ejemplo:
   - Instala Apache (si no lo tienes):

     ```bash
     sudo apt update
     sudo apt install apache2
     ```

   - Crea una página simple en `/var/www/html/test.html`:

     ```html
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
     ```

   - Crea un `test_form.php` muy básico (solo para ver parámetros, no hace falta PHP avanzado):

     ```php
     <?php
       $usuario = $_POST['usuario'];
       $mensaje = $_POST['mensaje'];
       echo "Hola " . htmlspecialchars($usuario) . ", has enviado: " . htmlspecialchars($mensaje);
     ?>
     ```

2. Desde tu máquina (Kali o el host), abre el navegador y accede a:

   ```text
   http://IP_DE_UBUNTU/test.html
   ```

3. Abre las **DevTools** → pestaña **Network**.
4. Rellena el formulario y envíalo.
5. Observa:
   - Petición `POST` a `/test_form.php`.
   - Parámetros `usuario` y `mensaje` en el cuerpo.
   - Respuesta que devuelve el servidor.

Crea:

`06-WebApp-OWASP-Semana_9_HTTP_DevTools.md`

Incluye:

- Capturas (o descripciones) de la petición.
- Explicación con tus palabras de:
  - Método, URL, parámetros, body.
  - Código de respuesta y cuerpo de la respuesta.
- Reflexión: ¿qué parte de la aplicación podrías intentar manipular si fueras un atacante?

---

### 6.2. Ejercicio 2 – Interceptar una petición con Burp Suite Community

1. Abre Burp Suite Community en tu Kali o en la máquina donde tengas el navegador.
2. Configura el navegador para que use el proxy de Burp:
   - Proxy HTTP: `127.0.0.1` puerto `8080`.
3. En Burp → pestaña **Proxy** → asegúrate de que “Intercept is on”.
4. Vuelve a enviar el formulario de `test.html`.
5. Burp debería interceptar la petición.
6. Observa:
   - Cómo se ven los parámetros `usuario` y `mensaje`.
   - Cabeceras de la petición.
7. Prueba a:
   - Cambiar el valor de `usuario` o `mensaje` en Burp antes de enviarla al servidor.
   - Enviar la petición y ver cómo cambia la respuesta.

Crea:

`06-WebApp-OWASP-Semana_9_Burp_Intercept.md`

Incluye:

- Descripción del flujo (navegador → Burp → servidor).
- Qué cambios has hecho en la petición.
- Cómo ha cambiado la respuesta.

Este ejercicio es la base de casi cualquier prueba de seguridad web.

---

### 6.3. Ejercicio 3 – Concepto de XSS en entorno controlado

⚠️ Importante: Haz esto **solo** en tu laboratorio, en una página de prueba.

1. Modifica `test_form.php` para que **no use** `htmlspecialchars` (solo en este laboratorio, precisamente para ver el riesgo):

   ```php
   <?php
     $usuario = $_POST['usuario'];
     $mensaje = $_POST['mensaje'];
     echo "Hola " . $usuario . ", has enviado: " . $mensaje;
   ?>
   ```

2. Envía en el campo `mensaje` el valor:

   ```text
   <script>alert('XSS de prueba');</script>
   ```

3. Si el navegador te muestra un **alert**, significa que el código JavaScript se ha ejecutado.

4. Reflexiona:
   - Esto es un ejemplo muy simple de **XSS almacenado o reflejado**, según cómo se use el dato.
   - Imagina que, en lugar de un `alert`, el script roba cookies o redirige a otro sitio.

Crea:

`06-WebApp-OWASP-Semana_9_XSS_Concepto.md`

Incluye:

- Pasos que has seguido.
- Captura o descripción del comportamiento (alert).
- Reflexión sobre:
  - Por qué es peligroso.
  - Cómo se podría mitigar (ej. siempre usar `htmlspecialchars` u otras medidas de sanitización, validación de entrada, Content Security Policy, etc.).

Después del ejercicio, es recomendable **volver a poner `htmlspecialchars`** para que tu página “de prueba” no quede vulnerable incluso en el lab.

---

### 6.4. Ejercicio 4 – Mini “modelado OWASP” de tu aplicación de prueba

Ahora que has visto:

- Cómo viajan parámetros.
- Cómo puedes interceptarlos y modificarlos.
- Cómo sería un XSS sencillo.

Crea:

`06-WebApp-OWASP-Semana_9_Modelado_Riesgos.md`

Para la aplicación de prueba (`test.html` + `test_form.php`), responde:

1. **Datos que maneja**  
   - ¿Qué información recibe desde el usuario?
   - ¿Qué podría añadirse en una app real (contraseñas, correos, etc.)?

2. **Superficie de ataque**  
   - Formularios (parámetros).
   - Cookies.
   - Rutas accesibles.

3. **Posibles vulnerabilidades (alto nivel)**  
   - XSS (si no se sanitiza entrada).
   - Inyección (si concatenaras parámetros en consultas SQL sin validación).
   - Exposición de datos sensibles (si mostrases datos de otros usuarios, etc.).

4. **Medidas de mitigación básicas**  
   - Validar entrada en servidor.
   - Sanitizar salida (output encoding).
   - Usar consultas preparadas para base de datos.
   - No mostrar mensajes de error detallados.

Este ejercicio te ayuda a practicar el pensamiento OWASP incluso con una app muy simple.

---

## 7. Entregables de la Semana 9

Al finalizar la semana deberías tener:

1. `06-WebApp-OWASP-Semana_9_HTTP_DevTools.md`  
   - Análisis de peticiones/respuestas HTTP con DevTools.
   - Explicación clara de cómo ves y entiendes los parámetros.

2. `06-WebApp-OWASP-Semana_9_Burp_Intercept.md`  
   - Descripción del uso básico de Burp como proxy.
   - Ejemplo de modificación de parámetros y su efecto.

3. `06-WebApp-OWASP-Semana_9_XSS_Concepto.md`  
   - Demostración controlada de un XSS simple.
   - Reflexión sobre el riesgo y mitigación.

4. `06-WebApp-OWASP-Semana_9_Modelado_Riesgos.md`  
   - Análisis de riesgos de tu pequeña aplicación.
   - Referencia a conceptos de OWASP (XSS, inyección, etc.) de forma razonada.

Opcional:

- Añadir una sección en tu `Lab_Setup_Report.md` llamada:
  - “Aplicaciones web de laboratorio” donde expliques:
    - Qué servidor web tienes.
    - Qué páginas de prueba usas.
    - Para qué las usas (formación, pruebas de seguridad).

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
- [ ] Has usado DevTools para ver parámetros y respuestas en una aplicación de tu laboratorio.
- [ ] Has usado Burp Suite Community para interceptar y modificar una petición.
- [ ] Has demostrado un XSS sencillo en un entorno de prueba y entiendes por qué es peligroso.
- [ ] Has realizado un mini modelado de riesgos tipo OWASP de tu pequeña aplicación de prueba.
- [ ] Tienes todos los ficheros de la Semana 9 creados y añadidos a tu repositorio.

Si has completado esta semana, ya no ves una aplicación web como “una web cualquiera”, sino como:

- Un flujo de peticiones/respuestas.
- Con parámetros que pueden manipularse.
- Con puntos de entrada que, si no se protegen, pueden ser explotados.

Esto te prepara muy bien para:

- Seguir profundizando en pentesting web.
- Entender alertas de WAF, logs de aplicaciones y detecciones en SIEM.
- Conectar el mundo OWASP con tu visión Blue/Red/Purple Team.

---

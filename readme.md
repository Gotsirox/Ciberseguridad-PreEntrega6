# Práctica: Análisis de Navegación Segura y Wi-Fi Pública (`practica-wifi-segura`)

## 1. Introducción
El objetivo de esta práctica es explorar y analizar el comportamiento del tráfico web al navegar en un sitio no cifrado (HTTP) mediante la inspección de paquetes en las herramientas de desarrollo del navegador. Además, se evalúan los riesgos de seguridad asociados al uso de redes Wi-Fi públicas y se describe cómo el uso de una Red Privada Virtual (VPN) junto con buenas prácticas de seguridad pueden mitigar estas vulnerabilidades.

---

## 2. Sitio Analizado
* **Sitio web:** `http://neverssl.com`
* **Propósito del sitio:** Es un servicio diseñado explícitamente para nunca utilizar cifrado SSL/TLS (HTTPS). Es utilizado habitualmente para forzar la redirección a portales cautivos en redes Wi-Fi públicas (hotspots) cuando los mecanismos automáticos de detección del sistema operativo fallan.

---

## 3. Evidencia Observada

A partir de la captura de pantalla obtenida en las herramientas de desarrollo (`F12` -> Pestaña **Network**), se extrajeron los siguientes datos de la primera solicitud HTTP realizada:

| Parámetro | Valor Observado |
| :--- | :--- |
| **Request URL** | `http://oldfreshwonderouspathway.neverssl.com/online/` |
| **Request Method** | `GET` |
| **Status Code** | `200 OK` |
| **Remote Address** | `34.223.124.45:80` |
| **Host Header** | `oldfreshwonderouspathway.neverssl.com` |
| **Referer** | `http://neverssl.com/` |
| **Protocolo Utilizado** | **HTTP** (Puerto 80, plano y sin cifrar) |
| **Headers de Respuesta (Server)** | `Apache/2.4.68 ()` |
| **Headers de Solicitud (Accept-Encoding)** | `gzip, deflate` |

---

## 4. Análisis y Respuestas

### 1. ¿Qué protocolo utiliza el sitio?
El sitio utiliza el protocolo **HTTP (Hypertext Transfer Protocol)** sin cifrado. Se confirma esto observando el esquema de la URL (`http://`), el puerto de destino estándar (`80` en Remote Address) y la falta de un certificado de seguridad SSL/TLS en la comunicación.

### 2. ¿Qué información puede observarse durante la solicitud?
A través de la inspección de la solicitud se puede observar diversa información transmitida en texto plano, entre ella:
* **Host:** `oldfreshwonderouspathway.neverssl.com` (indica el dominio exacto solicitado al servidor).
* **URL Completa:** `http://oldfreshwonderouspathway.neverssl.com/online/` (muestra la ruta exacta de la solicitud).
* **Método HTTP:** `GET` (solicitud de obtención de recursos).
* **Headers Adicionales:** Preferencias de lenguaje , políticas de caché , codificaciones aceptadas y el servidor web que responde.

### 3. ¿Qué riesgos existen al navegar mediante HTTP desde una red Wi-Fi pública?
Al utilizar HTTP en una red pública sin cifrado, todo el tráfico viaja en **texto claro**. Un atacante en la misma red local puede realizar ataques como *Sniffing* o * 
* **Man-in-the-Middle (MitM)* para:
* **Interceptar información confidencial:** Credenciales de usuario, contraseñas, cookies de sesión y formularios ingresados.
* **Sesión Hijacking:** Robar tokens o cookies de autenticación no seguras para suplantar la identidad de la víctima.
* **Inyección de código/Inyección HTML:** Alterar las respuestas del servidor para mostrar publicidad engañosa, redireccionar a sitios de *phishing* o inyectar malware en el navegador del usuario.

### 4. ¿Cómo cambiaría este escenario utilizando una VPN?
Al activar una VPN (Virtual Private Network) en una red Wi-Fi pública:
* **Cifrado:** Todo el tráfico que sale del dispositivo se cifra desde el origen utilizando algoritmos de cifrado fuertes.
* **Túnel seguro:** Se establece un túnel encapsulado seguro entre el dispositivo del usuario y el servidor VPN.
* **Protección del tráfico:** Cualquier atacante sniffing la red Wi-Fi solo verá paquetes ilegibles e ininteligibles transitando hacia la dirección del servidor VPN.
* **Privacidad:** La dirección IP pública real del usuario queda oculta, reemplazada por la IP del servidor VPN, evitando el rastreo de ubicación geográfica y de la red local.


## 5. Mis 3 Reglas de Oro para navegar en redes Wi-Fi públicas

1. **Evitar transacciones sensibles:** Nunca iniciar sesión en cuentas bancarias, realizar compras en línea o ingresar credenciales críticas mientras se esté conectado a una red pública.
2. **Utilizar siempre una VPN o forzar HTTPS:** Mantener conectada una VPN confiable y asegurarse de usar la extensión o función *HTTPS-Only* del navegador para evitar conexiones no cifradas.
3. **Desactivar la conexión automática y el uso compartido:** Desactivar la reconexión automática a redes Wi-Fi abiertas y deshabilitar las opciones de detección de red/compartir archivos en el sistema operativo.
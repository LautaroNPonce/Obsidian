# OBSIDIAN — Password Manager

Gestor de contraseñas seguro, privado y completamente offline, desarrollado con tecnologías web modernas.

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/shield-lock.svg" width="20"/> Descripción general

OBSIDIAN es una solución de gestión de credenciales diseñada para operar como una aplicación nativa en cualquier dispositivo móvil o escritorio sin depender de servidores externos ni modelos de suscripción.

Toda la información se cifra localmente utilizando **AES-256-GCM**, el mismo estándar utilizado por entidades financieras y organismos gubernamentales.  
La contraseña maestra nunca se almacena ni se transmite: solo existe en la memoria del usuario, garantizando un modelo de seguridad de conocimiento cero.

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/lock.svg" width="20"/> Seguridad

- Cifrado AES-256-GCM de extremo a extremo  
- Derivación de claves mediante PBKDF2 (310.000 iteraciones)  
- Contraseña maestra no persistida  
- Arquitectura sin dependencia de servidores (zero trust local)  
- Bloqueo automático configurable por inactividad  
- Confirmación de identidad para acciones críticas  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/database-lock.svg" width="20"/> Sistema de bóvedas (Vaults)

- Almacenamiento de credenciales (sitio, usuario, contraseña, URL, notas)  
- Múltiples bóvedas independientes (segmentación de datos)  
- Historial de contraseñas (últimos 10 cambios)  
- Búsqueda y filtrado en tiempo real  
- Sistema de categorías (Social, Trabajo, Banco, etc.)  
- Copiado al portapapeles en un solo clic  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/exclamation-triangle.svg" width="20"/> Alertas de seguridad

- Detección de contraseñas reutilizadas  
- Alertas de antigüedad (+90 días)  
- Priorización visual (niveles de riesgo)  
- Contador de alertas activas  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/key.svg" width="20"/> Generador de contraseñas

- Longitud configurable (8 a 64 caracteres)  
- Inclusión de mayúsculas, minúsculas, números y símbolos  
- Exclusión de caracteres ambiguos  
- Evaluación de fortaleza en tiempo real (7 niveles)  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/clock-history.svg" width="20"/> Registro de actividad

- Auditoría completa de acciones con timestamp  
- Incluye:
  - Accesos a bóvedas  
  - Visualización y copiado de contraseñas  
  - Ediciones y eliminaciones  
  - Backups e importaciones  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/download.svg" width="20"/> Sistema de backup

- Exportación cifrada en formato `.vault`  
- Importación segura con verificación de contraseña  
- Respaldo completo de datos y estructura  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/phone.svg" width="20"/> Progressive Web App (PWA)

- Instalación como aplicación nativa  
- Funcionamiento offline completo  
- Experiencia fullscreen (sin interfaz de navegador)  
- Ícono y configuración personalizada  
- Compatible con Android e iOS  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/palette.svg" width="20"/> Diseño

- Modo oscuro permanente  
- Interfaz responsive (mobile-first)  
- Transiciones y animaciones optimizadas  
- Enfoque en experiencia de usuario (UX/UI moderna)  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/code-slash.svg" width="20"/> Tecnologías utilizadas

- HTML5  
- CSS3  
- JavaScript (Vanilla)  
- Web Crypto API  

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/globe.svg" width="20"/> Demo

👉 https://obsidianlpn.netlify.app

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/person.svg" width="20"/> Autor

Desarrollado por Lautaro N Ponce

---

## <img src="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/icons/shield-check.svg" width="20"/> Filosofía

Sin servidores.  
Sin seguimiento.  
Sin suscripciones.  

**El control total de los datos pertenece exclusivamente al usuario.**

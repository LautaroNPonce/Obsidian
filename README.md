<div align="center">

# 🔐 OBSIDIAN

**Un gestor de contraseñas que cifra todo en tu dispositivo y no habla con ningún servidor.**
Tus credenciales nunca salen de tu navegador, y la contraseña maestra vive solo en tu cabeza.

![Cifrado](https://img.shields.io/badge/cifrado-AES--256--GCM-6366f1?style=flat-square)
![Dependencias](https://img.shields.io/badge/dependencias-0-2ea043?style=flat-square)
![PBKDF2](https://img.shields.io/badge/PBKDF2-600k_iteraciones-3b82f6?style=flat-square)
![PWA](https://img.shields.io/badge/PWA-offline-8b5cf6?style=flat-square)

</div>

<!--
  ↓ CAPTURA DE PANTALLA ↓
  Cuando tengas una captura linda:
    1. Creá la carpeta  docs/  en la raíz.
    2. Guardá la imagen como  captura.png
    3. Borrá este comentario para que se vea.

<div align="center">
  <img src="docs/captura.png" alt="Bóveda de OBSIDIAN" width="380">
</div>
-->

---

## Qué es

OBSIDIAN es un gestor de contraseñas que corre entero en el navegador. Guardás tus credenciales, generás contraseñas fuertes y las tenés siempre a mano —en el celular o en la compu— sin cuenta, sin registro y sin cuota mensual. Se instala como app (PWA) y, una vez cargada, funciona sin conexión.

Lo que lo distingue: **todo se cifra localmente con AES-256-GCM y la contraseña maestra no se guarda en ningún lado.** No hay servidor que hackear porque no hay servidor. Es un modelo de conocimiento cero real: sin la maestra, los datos son ruido —ni siquiera vos podés recuperarlos si la perdés.

### Funciona

| | |
|---|---|
| 🗄 **Bóvedas cifradas** | Múltiples bóvedas independientes, cada credencial con sitio, usuario, URL, categoría y notas. Guarda el historial de los últimos 10 cambios de cada contraseña. |
| 🔑 **Generador sin sesgo** | Contraseñas de 8 a 64 caracteres con muestreo por rechazo (sin el sesgo del operador módulo) y evaluación de fortaleza en 7 niveles. |
| 🚨 **Alertas de higiene** | Detecta contraseñas reutilizadas y avisa cuando una pasa los 90 días, con priorización visual por nivel de riesgo. |
| 📋 **Portapapeles que se borra solo** | Al copiar una contraseña, el portapapeles se limpia a los 30 segundos y te avisa. |
| 🔒 **Auto-bloqueo agresivo** | Se bloquea por inactividad configurable y también tras 60 s con la pestaña en segundo plano. Al bloquear, borra toda la bóveda descifrada de memoria y del DOM. |
| 🕵️ **Registro cifrado** | Auditoría de accesos, copiados, ediciones y backups con timestamp —cifrada con la misma clave que la bóveda, no en texto plano. |
| 💾 **Backup `.vault`** | Exportación cifrada con validación estricta al importar (rechaza archivos manipulados antes de tocar nada). |
| 🛡 **Anti fuerza bruta** | Backoff exponencial a partir del quinto intento fallido, con tope de 5 minutos. |

---

## Stack y decisiones

**HTML5 + CSS3 + JavaScript vanilla + Web Crypto API. Cero frameworks, cero build, cero backend.**

Toda la app es un solo `index.html`. No hay `npm install`, no hay bundler, no hay paso de compilación: abrís el archivo y funciona. La criptografía la hace el navegador con primitivas nativas, no una librería de terceros.

<details>
<summary><b>Por qué</b> (clic para abrir)</summary>

<br>

- **Web Crypto API en vez de una librería de cripto** → AES-256-GCM y PBKDF2 los provee el navegador, auditados y acelerados por hardware. La regla de oro es *no inventes tu propio cifrado*; acá directamente no se implementa ninguno.
- **Sin backend** → no hay base de datos que filtrar ni servidor que comprometer. La superficie de ataque del lado servidor es literalmente cero porque no existe.
- **Sin frameworks ni build** → menos dependencias es menos cadena de suministro que vigilar. El código que leés es el código que corre.
- **`localStorage` cifrado** → los datos persisten en el dispositivo; lo único que se guarda es blob cifrado + salt + un canary de verificación. Sin la maestra, todo eso es ruido.

**Lo honesto:** hoy los iconos (`bootstrap-icons`) y las fuentes (Google Fonts) se cargan desde CDN, así que la promesa "100% offline / sin servidores" todavía no es literal en la primera carga. El service worker cachea los assets propios, y self-hostear esas dependencias está en el [roadmap](#roadmap) (P-2) para cerrar el círculo.

</details>

---

## Estructura

```
obsidian/
├── index.html              La app entera: lógica, estilos y markup (~82 KB)
├── terminal-site.html      Landing page (estética terminal) que enlaza a la app
├── sw.js                   Service worker: cachea assets propios, habilita offline
├── manifest.json           Configuración PWA (nombre, íconos, colores)
├── icon.svg                Ícono de la app
│
├── vercel.json             Cabeceras de seguridad para Vercel
├── _headers                Equivalente para Netlify
│
├── AUDITORIA-SEGURIDAD.md  Informe de auditoría (hallazgos y correcciones)
└── README.md               Este archivo
```

> **Nota:** casi todo vive en `index.html`. La lógica está organizada en funciones como `renderList()` (bóveda), `unlockMaster()` (desbloqueo), `lockVault()` (bloqueo y limpieza) e `importBackup()` (restauración). La auditoría documenta cada una.

---

## Desarrollo

**Requisitos:** un navegador moderno y un servidor estático local. Nada de Node, nada de dependencias.

1. Cloná el repo.
2. Serví la carpeta con cualquier servidor estático (hace falta para que el service worker y la Web Crypto API funcionen; abrir el archivo con `file://` no alcanza).
3. Abrí `http://localhost:8000` (o el puerto que uses).

| Para | Hacé |
|---|---|
| Levantar local | `python3 -m http.server 8000` |
| Alternativa Node | `npx serve` |
| Ver la app | Abrí `index.html` vía `localhost` |
| Ver la landing | Abrí `terminal-site.html` |

### ⚠️ Advertencia importante del flujo

El service worker (`sw.js`) cachea de forma agresiva. Si editás `index.html` o los estilos y no ves los cambios, es la caché vieja del service worker. Subí el número de versión para forzar la actualización:

```js
// sw.js — al tocar cualquier asset, subí la versión
const CACHE = 'obsidian-v3';   // → 'obsidian-v4'
```

Al activarse, el nuevo worker borra las cachés viejas automáticamente. En desarrollo también podés usar "Update on reload" en la pestaña *Application* de las DevTools.

---

## Publicar

El deploy es de sitio estático puro. Está funcionando en Vercel y en Netlify:

1. Conectá el repo a **Vercel** o **Netlify** (o subí los archivos por FTP a cualquier hosting estático).
2. No hay comando de build: el *output directory* es la raíz.
3. Las cabeceras de seguridad se aplican solas —`vercel.json` en Vercel, `_headers` en Netlify.

Después de eso queda todo automático: cada push redeploya y el service worker actualiza a los usuarios en la siguiente visita.

🔗 **App:** https://obsidians-web.vercel.app/ · 🔗 **Landing:** https://obsidiansecure.netlify.app/

---

## Datos / Configuración

Todo vive en el `localStorage` del navegador, bajo el prefijo `vlt_`. No hay archivos de config: la app se autoconfigura en el primer arranque.

> ### 🚨 Lo que NUNCA hay que tocar
> **El salt (`vlt_s6`), el canary (`vlt_k6`) y las iteraciones (`vlt_i6`).** Son los que permiten derivar y verificar la clave. Si borrás o modificás cualquiera de estos a mano, la bóveda cifrada (`vlt_v6`) queda **irrecuperable**: no hay servidor de respaldo ni forma de regenerar la clave. Antes de tocar `localStorage`, exportá un backup `.vault`.

<details>
<summary><b>Claves de <code>localStorage</code></b> (detalle técnico)</summary>

<br>

```js
const K = {
  V:    'vlt_v6',   // bóveda cifrada (blob AES-256-GCM en base64)
  SALT: 'vlt_s6',   // salt de 128 bits para PBKDF2
  CFG:  'vlt_c6',   // configuración (auto-bloqueo, preferencias)
  LOG:  'vlt_l6',   // registro de actividad — cifrado
  ITER: 'vlt_i6',   // iteraciones de PBKDF2 (600000 en instalaciones nuevas)
  CHK:  'vlt_k6',   // canary cifrado para verificar la clave siempre
  FAIL: 'vlt_f6'    // contador de intentos fallidos (backoff)
};
```

**Parámetros de cifrado:** AES-256-GCM · IV aleatorio de 96 bits por operación · salt de 128 bits · PBKDF2-HMAC-SHA256.

**Versionado de iteraciones:** las instalaciones nuevas usan **600.000** (recomendación OWASP actual). Las viejas de **310.000** se detectan como *legacy*, desbloquean normal y se **migran solas** en el primer unlock exitoso, con salt nuevo y re-cifrado completo. Por eso el número vive en `vlt_i6` en vez de estar hardcodeado.

</details>

---

## Mantenimiento

<details>
<summary><b>Actualizar el service worker tras un cambio</b></summary>

<br>

```bash
# No es un comando: es editar sw.js
const CACHE = 'obsidian-vN'   # subí N
```

Hacelo cada vez que toques `index.html`, `manifest.json` o `icon.svg`. Al activarse, el worker nuevo purga las cachés anteriores. Si no lo subís, los usuarios siguen viendo la versión vieja hasta que la caché expire.

</details>

<details>
<summary><b>Revisar la auditoría antes de un cambio de seguridad</b></summary>

<br>

Antes de tocar cualquier cosa que renderice datos del usuario o maneje la clave, leé `AUDITORIA-SEGURIDAD.md`. Documenta 20 hallazgos ya corregidos y las trampas exactas de cada función.

> ⚠️ La CSP actual necesita `'unsafe-inline'` porque el JS está embebido y usa `onclick`. Con eso, la CSP **no bloquea la ejecución de un XSS**, solo su exfiltración. Si agregás código que interpola datos del usuario en `innerHTML`, pasalo **siempre** por la función `esc()`. Es el vector más peligroso en un gestor de contraseñas.

</details>

---

## Roadmap

Los cuatro pendientes salen directo de la auditoría (sección "Pendientes — requieren refactor"). Ordenados por lo que más rinde:

- [ ] **Sacar el JS a un archivo externo** y reemplazar los `onclick` inline por `addEventListener`, para poder pasar a `script-src 'self'` sin `'unsafe-inline'` y cerrar el vector de XSS entero.
- [ ] **Self-hostear iconos y fuentes** (bootstrap-icons + Google Fonts) → elimina dos orígenes de la CSP, corta el riesgo de cadena de suministro y cumple de verdad la promesa offline.
- [ ] **Evaluar Argon2id vía WebAssembly** como reemplazo de PBKDF2 (resistente a memoria, más fuerte contra ataques con GPU).
- [ ] **Considerar IndexedDB con `CryptoKey` no extraíble**, para que ni un XSS pueda leer el material de clave.

---

## Créditos

- Iconografía: [**Bootstrap Icons**](https://icons.getbootstrap.com/) (MIT).
- Tipografías: [**Plus Jakarta Sans**](https://fonts.google.com/specimen/Plus+Jakarta+Sans), [**JetBrains Mono**](https://www.jetbrains.com/lp/mono/) e [**Instrument Serif**](https://fonts.google.com/specimen/Instrument+Serif) (SIL Open Font License).
- Criptografía: [**Web Crypto API**](https://developer.mozilla.org/es/docs/Web/API/Web_Crypto_API), nativa del navegador.

Desarrollado por **Lautaro N. Ponce**.

<div align="center">
<br>
<sub>Sin servidores. Sin seguimiento. Sin suscripciones. El control de los datos es tuyo, y de nadie más.</sub>
</div>

# Fiber Inventory — Denis Bardales

App de inventario de hilos de fibra óptica (Nodo → ODF → Cierres → Destino),
accesible desde cualquier dispositivo con conexión a internet.

## Paso 1 — Crear la base de datos (Firebase, gratis)

1. Entra a **https://console.firebase.google.com** con tu cuenta de Google.
2. Clic en **"Agregar proyecto"** → dale un nombre (ej. `fiber-inventory`) → continúa con la
   configuración por defecto → **Crear proyecto**.
3. Dentro del proyecto, clic en el ícono ⚙️ (arriba a la izquierda) → **Configuración del proyecto**.
4. Baja hasta **"Tus apps"** → clic en el ícono **Web `</>`** → dale un apodo (ej. `web`) →
   **Registrar app**. NO necesitas Firebase Hosting, sáltalo.
5. Firebase te muestra un bloque de código con `const firebaseConfig = { ... }`. **Copia esos valores.**
6. En el menú lateral izquierdo: **Compilación → Firestore Database → Crear base de datos**.
   Elige **modo producción** y la región más cercana (ej. `us-central`).
7. En el menú lateral: **Compilación → Authentication → Comenzar → Sign-in method** →
   habilita el proveedor **Anónimo**.
8. Vuelve a **Firestore Database → pestaña "Reglas"** y reemplaza el contenido por esto, luego **Publicar**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /inventario/{doc} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Esto permite leer/escribir solo a usuarios autenticados (tu propia app), no a cualquiera en internet.

## Paso 2 — Pegar tu configuración en el código

1. Abre el archivo `index.html` de este repositorio (puedes editarlo directo en GitHub,
   con el lápiz ✏️ en la esquina superior derecha del archivo).
2. Busca este bloque (cerca de la línea 340) y reemplaza cada `"PEGA_AQUI..."` con los valores
   que copiaste de Firebase en el paso 1.5:

```js
const firebaseConfig = {
  apiKey: "PEGA_AQUI_TU_API_KEY",
  authDomain: "PEGA_AQUI.firebaseapp.com",
  projectId: "PEGA_AQUI_TU_PROJECT_ID",
  storageBucket: "PEGA_AQUI.appspot.com",
  messagingSenderId: "PEGA_AQUI",
  appId: "PEGA_AQUI"
};
```

3. Guarda los cambios (**Commit changes**).

## Paso 3 — Publicar la app (GitHub Pages, gratis)

1. En este repositorio, ve a **Settings → Pages** (menú lateral izquierdo).
2. En **"Build and deployment"** → **Source**, elige **Deploy from a branch**.
3. En **Branch**, elige `main` y la carpeta `/ (root)` → **Save**.
4. Espera 1-2 minutos. GitHub te dará una URL como:
   `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`
5. Abre esa URL desde tu teléfono, tu computadora o cualquier dispositivo — los datos se
   sincronizan automáticamente entre todos porque viven en Firestore, no en el navegador.

## Tip: instalar como app en el teléfono

En Chrome (Android) o Safari (iPhone), abre la URL → menú (⋮ o compartir) →
**"Agregar a pantalla de inicio"**. Se verá y comportará como una app normal.

## Respaldo

El botón **Exportar CSV** dentro de la app sigue funcionando igual — úsalo de vez en cuando
como respaldo adicional fuera de Firebase.

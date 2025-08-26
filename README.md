# Sorteo 00–99 (HTML + Firebase)

App web para gestionar un sorteo de números con **actualización en vivo** usando Firebase (Auth + Firestore) y hospedaje en **GitHub Pages**.

## Archivos
- `index.html` → app lista para publicar.
- `README.md` → esta guía.

## Requisitos (en Firebase)
1. Crear proyecto y habilitar **Firestore (producción)**.
2. Habilitar **Authentication**: **Anonymous** y **Email/Password**.
3. Agregar dominios autorizados: `localhost` y `TUUSUARIO.github.io`.
4. Reglas de Firestore:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /raffle/current { allow read: if true; allow write: if request.auth != null; }
    match /raffle/current/numbers/{n} { allow read: if true; allow write: if request.auth != null; }
  }
}
```
5. Crear un usuario admin (email+password) en **Authentication → Users**.

## Configurar `firebaseConfig`
En `index.html` buscá:
```js
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_AUTH_DOMAIN",
  projectId: "TU_PROJECT_ID",
  storageBucket: "TU_STORAGE_BUCKET",
  messagingSenderId: "TU_MESSAGING_SENDER_ID",
  appId: "TU_APP_ID"
};
```
Reemplazalo con los valores reales de tu app web de Firebase.

## Publicar en GitHub Pages (paso a paso)
1. Crear repo nuevo en GitHub (p. ej. `sorteo-00-99`).
2. Subir `index.html` y `README.md` al branch `main`.
3. Ir a **Settings → Pages**:
   - **Source**: `Deploy from a branch`
   - **Branch**: `main` y carpeta `/ (root)`
4. Guardar. La URL quedará como `https://TUUSUARIO.github.io/sorteo-00-99/`.
5. En Firebase **Authentication → Authorized domains**, comprobar que `TUUSUARIO.github.io` esté agregado.
6. Abrir la URL. Botón **🔑 Admin** para loguearte con tu usuario admin.

## Flujo
- Público: elige hasta **2 números** (promo 2x). Verde=disponible, Amarillo=seleccionado, Azul=pagado.
- Admin: edita título, rango, precios, premios; resetea todo; cambia estado por número; exporta CSV; ejecuta pruebas.

## Troubleshooting
- **Missing or insufficient permissions**: verificá que el usuario esté autenticado (anónimo o admin) y reglas con `request.auth != null`.
- **No carga en Pages**: esperá 1–2 minutos tras activar Pages, luego refrescá y revisá la pestaña **Actions** del repo si hay errores.

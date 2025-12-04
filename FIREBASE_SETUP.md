# 🔥 Guía Completa: Conectar Firebase a Vivir Xalapa

## 📋 Requisitos Previos
- Una cuenta de Google
- Proyecto React funcionando localmente
- 10-15 minutos de tiempo

---

## 🚀 Paso 1: Crear Proyecto en Firebase

1. **Accede a Firebase Console:**
   - Abre https://console.firebase.google.com/
   - Inicia sesión con tu cuenta de Google

2. **Crear Nuevo Proyecto:**
   - Haz clic en **"Agregar proyecto"** o **"Add project"**
   - Nombre del proyecto: `vivir-xalapa` (o el que prefieras)
   - Haz clic en **"Continuar"**

3. **Google Analytics (Opcional):**
   - Puedes desactivarlo si no lo necesitas
   - Haz clic en **"Crear proyecto"**
   - Espera 30-60 segundos mientras se crea

4. **Proyecto Listo:**
   - Haz clic en **"Continuar"** cuando termine

---

## 🌐 Paso 2: Registrar App Web

1. **En el Dashboard del proyecto:**
   - Busca el ícono **`</>`** (Web) 
   - Haz clic en él

2. **Registrar la app:**
   - Apodo de la app: `Vivir Xalapa Web`
   - **NO marques** "Firebase Hosting" por ahora
   - Haz clic en **"Registrar app"**

3. **Copiar Configuración:**
   - Verás un código JavaScript como este:
   ```javascript
   const firebaseConfig = {
     apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
     authDomain: "vivir-xalapa-xxxxx.firebaseapp.com",
     projectId: "vivir-xalapa-xxxxx",
     storageBucket: "vivir-xalapa-xxxxx.appspot.com",
     messagingSenderId: "123456789012",
     appId: "1:123456789012:web:abcdef1234567890"
   };
   ```
   - **¡Copia estos valores!** Los necesitarás pronto
   - Haz clic en **"Continuar a la consola"**

---

## 🔐 Paso 3: Habilitar Authentication (Autenticación)

1. **En el menú lateral izquierdo:**
   - Haz clic en **"Authentication"** (o "Autenticación")

2. **Comenzar:**
   - Haz clic en el botón **"Comenzar"** o **"Get started"**

3. **Habilitar Autenticación Anónima:**
   - En la pestaña **"Sign-in method"** (Método de acceso)
   - Busca **"Anónimo"** o **"Anonymous"**
   - Haz clic sobre él
   - Activa el **toggle** a la derecha
   - Haz clic en **"Guardar"**

✅ **¡Autenticación configurada!**

---

## 💾 Paso 4: Habilitar Firestore Database

1. **En el menú lateral izquierdo:**
   - Haz clic en **"Firestore Database"**

2. **Crear base de datos:**
   - Haz clic en **"Crear base de datos"** o **"Create database"**

3. **Seleccionar modo:**
   - Elige **"Comenzar en modo de prueba"** o **"Start in test mode"**
   - Haz clic en **"Siguiente"**

4. **Seleccionar ubicación:**
   - Elige la más cercana (ejemplo: `us-central` o `southamerica-east1`)
   - Haz clic en **"Habilitar"** o **"Enable"**
   - Espera 1-2 minutos

5. **Base de datos lista:**
   - Verás una pantalla vacía (es normal, no hay datos aún)

✅ **¡Firestore configurado!**

---

## 🔧 Paso 5: Configurar Reglas de Seguridad

1. **En Firestore Database:**
   - Haz clic en la pestaña **"Reglas"** o **"Rules"** (arriba)

2. **Editar reglas:**
   - Verás un editor de código
   - **Borra todo** el contenido existente
   - **Pega** este código:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Reglas para el foro
    match /artifacts/{appId}/public/data/xalapa_forum/{postId} {
      allow read: if true;
      allow create: if request.auth != null;
      allow delete: if request.auth != null && 
                      request.auth.uid == resource.data.userId;
    }
    
    // Reglas para conductores
    match /artifacts/{appId}/public/data/xalapa_drivers/{driverId} {
      allow read: if true;
      allow write: if request.auth != null && 
                     request.auth.uid == driverId;
    }
  }
}
```

3. **Publicar reglas:**
   - Haz clic en **"Publicar"** o **"Publish"**

✅ **¡Reglas configuradas!**

---

## 💻 Paso 6: Configurar Variables de Entorno

1. **En tu proyecto, crea el archivo `.env.local`:**
   ```bash
   # En la raíz del proyecto
   touch .env.local
   ```

2. **Edita `.env.local` y pega:**
   ```env
   VITE_FIREBASE_API_KEY=tu-api-key-aqui
   VITE_FIREBASE_AUTH_DOMAIN=tu-proyecto.firebaseapp.com
   VITE_FIREBASE_PROJECT_ID=tu-proyecto-id
   VITE_FIREBASE_STORAGE_BUCKET=tu-proyecto.appspot.com
   VITE_FIREBASE_MESSAGING_SENDER_ID=123456789012
   VITE_FIREBASE_APP_ID=1:123456789012:web:abcdef1234567890
   ```

3. **Reemplaza los valores:**
   - Usa los valores que copiaste en el **Paso 2**
   - Ejemplo real:
   ```env
   VITE_FIREBASE_API_KEY=AIzaSyA1b2C3d4E5f6G7h8I9j0K1L2M3N4O5P6Q
   VITE_FIREBASE_AUTH_DOMAIN=vivir-xalapa-12345.firebaseapp.com
   VITE_FIREBASE_PROJECT_ID=vivir-xalapa-12345
   VITE_FIREBASE_STORAGE_BUCKET=vivir-xalapa-12345.appspot.com
   VITE_FIREBASE_MESSAGING_SENDER_ID=987654321098
   VITE_FIREBASE_APP_ID=1:987654321098:web:abc123def456ghi789
   ```

4. **Guarda el archivo**

---

## ✅ Paso 7: Probar la Conexión

1. **Reinicia el servidor de desarrollo:**
   ```bash
   # Presiona Ctrl+C para detener
   # Luego ejecuta:
   npm run dev
   ```

2. **Abre la app en el navegador:**
   - Normalmente: http://localhost:5173

3. **Abre la Consola del Navegador:**
   - Presiona `F12` o `Ctrl+Shift+I` (Windows/Linux)
   - Presiona `Cmd+Option+I` (Mac)
   - Ve a la pestaña **"Console"**

4. **Verifica el mensaje:**
   - Deberías ver: `✅ Firebase conectado exitosamente`
   - Si ves: `ℹ️ Ejecutando en modo local` = Firebase no está configurado

---

## 🎉 Paso 8: ¡Prueba la App!

### Probar el Foro:
1. Ve a la pestaña **"Foro"**
2. Escribe un comentario
3. Haz clic en **"Publicar"**
4. Abre la app en **otra pestaña** o **otro navegador**
5. ¡Verás el comentario aparecer automáticamente! 🚀

### Probar el Modo Conductor:
1. Ve a **"Perfil"**
2. Activa **"Modo Conductor"**
3. Selecciona un tipo de camión
4. Ve a **"Mapa"**
5. Haz clic en **"Iniciar Ruta"**
6. Abre la app en otra pestaña
7. ¡Verás tu camión moverse en tiempo real! 🚌

---

## 🔍 Verificar Datos en Firebase

1. **Ve a Firebase Console**
2. **Firestore Database**
3. **Navega a:** `artifacts → vivir-xalapa-default → public → data`
4. Verás dos colecciones:
   - `xalapa_forum` - Comentarios del foro
   - `xalapa_drivers` - Conductores activos

---

## 🐛 Solución de Problemas

### ❌ Error: "Firebase: Error (auth/...)"
- Verifica que habilitaste **Authentication → Anonymous**

### ❌ Error: "Missing or insufficient permissions"
- Verifica las **Reglas de Firestore** (Paso 5)
- Asegúrate de publicar las reglas

### ❌ No aparece el mensaje de conexión
- Verifica que `.env.local` tenga todos los valores correctos
- Reinicia el servidor (`Ctrl+C` y luego `npm run dev`)
- Verifica que los nombres empiecen con `VITE_`

### ❌ Los datos no se sincronizan
- Abre la consola del navegador (F12)
- Busca errores en rojo
- Verifica tu conexión a internet

---

## 📚 Recursos Adicionales

- [Documentación de Firebase](https://firebase.google.com/docs)
- [Firestore Quickstart](https://firebase.google.com/docs/firestore/quickstart)
- [Authentication Docs](https://firebase.google.com/docs/auth)

---

## ⚠️ Importante: Seguridad

**NUNCA subas `.env.local` a GitHub:**
- Ya está en `.gitignore`
- Contiene credenciales sensibles
- Cada desarrollador debe crear su propio `.env.local`

**Para producción:**
- Cambia las reglas de Firestore de "modo de prueba" a reglas más estrictas
- Considera habilitar App Check
- Revisa los límites de uso gratuito de Firebase

---

## 🎊 ¡Felicidades!

Tu app ahora está conectada a Firebase y puede:
- ✅ Sincronizar datos en tiempo real
- ✅ Autenticar usuarios anónimamente
- ✅ Guardar comentarios permanentemente
- ✅ Mostrar conductores en vivo en el mapa

**¡Disfruta desarrollando Vivir Xalapa! 🌿**

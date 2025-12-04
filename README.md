# Vivir Xalapa 🌿

Aplicación web comunitaria para Xalapa, Veracruz. Conecta usuarios, conductores y locales en tiempo real.

## 🚀 Iniciar la aplicación

```bash
npm install
npm run dev
```

La aplicación estará disponible en `http://localhost:5173` (o el puerto que indique la consola).

## ✨ Características

### 🏠 **Pantalla de Inicio**
- Widget del clima de Xalapa
- Accesos rápidos a Rutas y Turismo
- Noticias locales (Tráfico, Cultura, Clima)

### 🗺️ **Mapa Interactivo**
- Mapa de OpenStreetMap de Xalapa Centro
- Marcadores de restaurantes, sitios turísticos y paradas de autobús
- Filtros para mostrar/ocultar categorías
- **Modo Conductor**: Rastrea tu ubicación en tiempo real
- Selección de tipo de camión (verde, amarillo, blanco)

### 💬 **Foro Comunitario**
- Publica comentarios y actualizaciones
- Edita tu nombre de usuario
- Elimina tus propios comentarios
- ✅ **FUNCIONA SIN FIREBASE** en modo local

### 👤 **Perfil**
- Cambia entre modo Usuario y Conductor
- Personaliza tu nombre
- Configura el tipo de camión (para conductores)

## 🔧 Modo de Funcionamiento

### **Modo Local (Sin Firebase)**
Por defecto, la app funciona **completamente local** sin necesidad de configurar Firebase:

- ✅ Los comentarios se guardan en la memoria del navegador
- ✅ Los conductores se rastrean localmente
- ✅ Se genera un ID de usuario automáticamente
- ✅ **Puedes escribir y publicar comentarios inmediatamente**

### **Modo Firebase (Con Sincronización en Tiempo Real)**

#### **Configuración Rápida:**

1. **Crear proyecto en Firebase Console:**
   - Ve a https://console.firebase.google.com/
   - Crea un nuevo proyecto
   - Habilita **Authentication** (modo Anónimo)
   - Habilita **Firestore Database** (modo de prueba)

2. **Configurar variables de entorno:**
   ```bash
   cp .env.example .env.local
   ```
   
3. **Edita `.env.local`** con tus credenciales de Firebase:
   - Ve a Configuración del proyecto → Tus apps → Web
   - Copia los valores de `firebaseConfig`
   - Pégalos en `.env.local`

4. **Reinicia el servidor:**
   ```bash
   npm run dev
   ```

5. **Verifica la conexión:**
   - Abre la consola del navegador
   - Deberías ver: `✅ Firebase conectado exitosamente`

#### **Reglas de Firestore (Firebase Console → Firestore → Rules):**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/{appId}/public/data/{collection}/{docId} {
      allow read: if true;
      allow create: if request.auth != null;
      allow delete: if request.auth != null && 
                      request.auth.uid == resource.data.userId;
      allow update: if request.auth != null && 
                      request.auth.uid == resource.data.userId;
    }
  }
}
```

**Beneficios de Firebase:**
- 🔄 Sincronización en tiempo real entre usuarios
- 🌍 Múltiples usuarios pueden ver los mismos datos
- 💾 Persistencia de datos (no se pierden al recargar)
- 🚌 Conductores visibles en tiempo real en el mapa

## 📱 Tecnologías

- **React 18** - Framework de UI
- **Vite** - Build tool ultra-rápido
- **Tailwind CSS** - Estilos modernos
- **Lucide React** - Iconos hermosos
- **Firebase** (opcional) - Backend en tiempo real

## 🔥 Solución al Problema del Foro

**Problema anterior**: El foro no permitía escribir ni publicar comentarios porque esperaba autenticación de Firebase.

**Solución implementada**: 
- ✅ Sistema dual: funciona con o sin Firebase
- ✅ Generación automática de ID de usuario local
- ✅ Estado local de React para almacenar posts
- ✅ Botón "Publicar" siempre habilitado en modo local

## 📝 Estructura del Proyecto

```
VivirXalapa/
├── src/
│   ├── App.jsx          # Componente principal
│   ├── main.jsx         # Punto de entrada
│   └── index.css        # Estilos globales
├── index.html           # HTML base
├── package.json         # Dependencias
├── vite.config.js       # Configuración Vite
└── tailwind.config.js   # Configuración Tailwind
```

## 🎨 Paleta de Colores

- **Primario**: Verde Esmeralda (`emerald-700`, `emerald-800`)
- **Acentos**: Naranja (restaurantes), Morado (turismo), Azul (transporte)
- **Neutros**: Grises suaves para fondos

## 🚌 Tipos de Camión

- 🟢 **Verde** - Rutas principales
- 🟡 **Amarillo** - Rutas locales  
- ⚪ **Blanco** - Rutas especiales

## 📄 Licencia

MIT - Creado para la comunidad de Xalapa, Veracruz 🇲🇽

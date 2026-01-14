# El Impostor - Juego de Deducción Social

Juego multijugador online estilo "Among Us" simplificado donde un grupo de amigos pueden jugar juntos. Algunos jugadores reciben una palabra secreta y deben dar pistas, mientras que el impostor debe adivinar la palabra sin ser descubierto.

## Demo

**GitHub Pages:** https://richardo880.github.io/el-impostor/

## Screenshots

### Tema Oscuro (Dark Mode)
- Estética noir-mystery con gradientes púrpura y cyan
- Efectos de glow atmosféricos
- Orbes ambientales animados

### Tema Claro (Light Mode)
- Tonos cálidos y elegantes
- Mismos efectos con paleta suave

## Cómo Jugar

1. **Crear sala:** Un jugador crea la sala, define palabras e impostores
2. **Unirse:** Otros jugadores ingresan el código de 6 caracteres
3. **Iniciar:** El creador inicia cuando hay 2+ jugadores
4. **Jugar:**
   - Inocentes reciben la palabra secreta y dan pistas
   - Impostores escuchan pistas e intentan pasar desapercibidos
5. **Terminar:** El creador termina la ronda y se repite

## Tecnologías

- **Frontend:** HTML5, CSS3, JavaScript (Vanilla)
- **Backend:** Firebase Realtime Database
- **Fuentes:** Bebas Neue + Space Grotesk (Google Fonts)
- **Hosting:** GitHub Pages / Firebase Hosting

## Características

### Diseño
- Tema oscuro/claro con toggle (esquina superior derecha)
- Preferencia guardada en localStorage
- Animaciones fluidas y micro-interacciones
- Diseño responsivo para móviles
- Efectos de glow y gradientes atmosféricos
- Avatares con iniciales del jugador

### Funcionalidades
- Salas en tiempo real con código único
- Asignación aleatoria de roles
- Sistema de rondas con palabras consumibles
- Creador puede eliminar jugadores
- Detección automática de desconexiones
- Sincronización instantánea entre dispositivos

## Estructura del Proyecto

```
el-impostor/
├── public/
│   ├── index.html      # Aplicación completa (HTML + CSS + JS)
│   └── 404.html        # Página de error
├── firebase.json       # Configuración Firebase (opcional)
├── database.rules.json # Reglas de seguridad
└── README.md
```

## Configuración de Firebase

### Configuración actual en el código

```javascript
const firebaseConfig = {
    apiKey: "TU_API_KEY",
    authDomain: "tu-proyecto.firebaseapp.com",
    databaseURL: "https://tu-proyecto-default-rtdb.firebaseio.com",
    projectId: "tu-proyecto",
};
```

**Ubicación:** `public/index.html` (buscar `firebaseConfig`)

### Crear tu propio proyecto Firebase

1. Ve a [Firebase Console](https://console.firebase.google.com/)
2. Clic en "Agregar proyecto"
3. Nombre del proyecto (ej: `mi-impostor`)
4. Desactiva Google Analytics (opcional)
5. Clic en "Crear proyecto"

#### Activar Realtime Database

1. En el menú lateral: **Build → Realtime Database**
2. Clic en "Crear base de datos"
3. Selecciona ubicación (us-central1 recomendado)
4. Selecciona "Iniciar en modo de prueba"
5. Clic en "Habilitar"

#### Obtener credenciales

1. Clic en el ícono de engranaje → **Configuración del proyecto**
2. Scroll hasta "Tus apps" → Clic en icono web `</>`
3. Registra la app con un nombre
4. Copia el objeto `firebaseConfig`
5. Reemplázalo en `public/index.html`

## Desarrollo Local

### Opción 1: Abrir directamente
```
Doble clic en public/index.html
```

### Opción 2: Servidor Python
```bash
cd public
python -m http.server 5000
# Abrir http://localhost:5000
```

### Opción 3: Firebase CLI
```bash
npm install -g firebase-tools
firebase login
firebase serve
# Abrir http://localhost:5000
```

## Despliegue

### GitHub Pages
```bash
git add .
git commit -m "Actualización"
git push origin main
```
Configurar en Settings → Pages → Branch: main → Folder: /public

### Firebase Hosting
```bash
firebase login
firebase init hosting
firebase deploy
```

## Estructura de Datos Firebase

```javascript
rooms/
  {ROOM_CODE}/           // ej: "ABC123"
    creatorId: "player_123"
    status: "waiting" | "playing"
    impostorCount: 1
    currentWord: "Gato"
    words: ["Perro", "Luna"]
    players/
      player_123/
        id: "player_123"
        name: "Juan"
        role: "inocente" | "impostor" | null
        word: "Gato" | null
        connected: true
```

## Personalización

### Cambiar colores del tema

En `public/index.html`, busca las variables CSS:

```css
:root {
    /* Tema Oscuro */
    --accent-primary: #7c3aed;    /* Púrpura principal */
    --accent-secondary: #06b6d4;  /* Cyan secundario */
    --danger: #ef4444;            /* Rojo impostor */
    --success: #10b981;           /* Verde inocente */
}

[data-theme="light"] {
    /* Tema Claro */
    --accent-primary: #6d28d9;
    /* ... */
}
```

### Cambiar fuentes

Modifica el link de Google Fonts en el `<head>`:

```html
<link href="https://fonts.googleapis.com/css2?family=TU_FUENTE&display=swap" rel="stylesheet">
```

## Limitaciones

- Sin autenticación de usuarios
- Sin persistencia de partidas
- Sin chat integrado
- Sin sistema de votación automático
- Sin temporizador de rondas

## Ideas Futuras

- [ ] Sistema de votación para eliminar impostores
- [ ] Temporizador por ronda
- [ ] Chat integrado
- [ ] Efectos de sonido
- [ ] Categorías de palabras predefinidas
- [ ] Modo espectador
- [ ] Estadísticas de victorias

## Créditos

- Diseño original inspirado en Among Us
- Rediseño UI/UX con estética noir-mystery
- Fuentes: [Google Fonts](https://fonts.google.com/)
- Backend: [Firebase](https://firebase.google.com/)

## Licencia

MIT License - Libre para uso personal y comercial.

---

**Desarrollado para jugar con amigos**

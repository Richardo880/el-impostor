# El Impostor - Juego de Deducción Social

Juego multijugador online estilo "Among Us" simplificado donde un grupo de amigos pueden jugar juntos. Algunos jugadores reciben una palabra secreta y deben dar pistas, mientras que el impostor debe adivinar la palabra sin ser descubierto.

## Demo en Vivo

| Plataforma | URL |
|------------|-----|
| Firebase Hosting | https://el-impostor-py.web.app |
| GitHub Pages | https://richardo880.github.io/el-impostor/public/ |

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
- **Hosting:** Firebase Hosting / GitHub Pages

## Características

### Diseño
- Tema oscuro/claro con toggle (esquina superior derecha)
- Preferencia guardada en localStorage
- Animaciones fluidas y micro-interacciones
- Diseño responsivo para móviles
- Efectos de glow y gradientes atmosféricos
- Avatares con iniciales del jugador

### Funcionalidades
- Salas en tiempo real con código único de 6 caracteres
- Asignación aleatoria de roles (inocente/impostor)
- Sistema de rondas con palabras consumibles
- Creador puede eliminar jugadores
- Detección automática de desconexiones
- Sincronización instantánea entre dispositivos

## Estructura del Proyecto

```
el-impostor/
├── public/
│   ├── index.html        # Aplicación completa (HTML + CSS + JS)
│   └── 404.html          # Página de error
├── firebase.json         # Configuración Firebase Hosting
├── .firebaserc           # Proyecto Firebase vinculado
├── database.rules.json   # Reglas de seguridad de la BD
└── README.md
```

## Configuración de Firebase

### Proyecto actual

- **Project ID:** `el-impostor-py`
- **Hosting:** https://el-impostor-py.web.app
- **Database:** Firebase Realtime Database

### Configuración en el código

```javascript
const firebaseConfig = {
  apiKey: "...",
  authDomain: "el-impostor-py.firebaseapp.com",
  databaseURL: "https://el-impostor-py-default-rtdb.firebaseio.com",
  projectId: "el-impostor-py",
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

## Seguridad

### Sobre la API Key expuesta

Las API keys de Firebase para aplicaciones web **son públicas por diseño**. La seguridad se implementa mediante:

1. **Firebase Security Rules** - Controlan quién puede leer/escribir datos
2. **Restricción de dominios** - Limita desde qué URLs puede usarse la key

### Restringir la API Key (recomendado)

1. Ve a [Google Cloud Console - Credentials](https://console.cloud.google.com/apis/credentials)
2. Selecciona tu proyecto
3. Clic en tu API key
4. En "Application restrictions" → "HTTP referrers"
5. Agrega tus dominios:
   ```
   https://tu-proyecto.web.app/*
   https://tu-usuario.github.io/*
   http://localhost:*
   ```

### Reglas de la Base de Datos

Archivo `database.rules.json`:

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true,
        ".validate": "newData.hasChildren(['creatorId', 'status', 'players'])",
        "players": {
          "$playerId": {
            ".validate": "newData.hasChildren(['id', 'name'])"
          }
        }
      }
    },
    ".read": false,
    ".write": false
  }
}
```

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
firebase serve
# Abrir http://localhost:5000
```

## Despliegue

### Firebase Hosting (recomendado)

```bash
# Instalar Firebase CLI (solo la primera vez)
npm install -g firebase-tools

# Login (solo la primera vez)
firebase login

# Desplegar todo (hosting + database rules)
firebase deploy

# Solo hosting
firebase deploy --only hosting

# Solo reglas de base de datos
firebase deploy --only database
```

### GitHub Pages

```bash
git add .
git commit -m "Actualización"
git push origin main
```

Configurar en **Settings → Pages → Branch: main**

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

## Limitaciones Actuales

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

## Comandos Útiles

```bash
# Ver estado del proyecto Firebase
firebase projects:list

# Ver configuración actual
firebase use

# Cambiar de proyecto
firebase use otro-proyecto

# Ver logs de hosting
firebase hosting:channel:list
```

## Créditos

- Diseño original inspirado en Among Us
- Rediseño UI/UX con estética noir-mystery
- Fuentes: [Google Fonts](https://fonts.google.com/)
- Backend: [Firebase](https://firebase.google.com/)

## Licencia

MIT License - Libre para uso personal y comercial.

---

**Desarrollado para jugar con amigos**

**Repositorio:** https://github.com/Richardo880/el-impostor

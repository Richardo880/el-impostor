# Juego del Impostor - Online Multiplayer

Juego multijugador online estilo "Among Us" simplificado donde un grupo de amigos pueden jugar juntos. Algunos jugadores reciben una palabra secreta y deben dar pistas, mientras que el impostor debe adivinar la palabra sin ser descubierto.

## 🌐 Sitio en Producción

**URL:** https://juego-el-impostor.web.app

## 🎮 Cómo Funciona el Juego

1. **Creador crea sala:**
   - Ingresa su nombre
   - Define lista de palabras (ej: "Gato, Perro, Luna, Sol")
   - Define cantidad de impostores
   - Recibe código de sala (ej: ABC123)

2. **Otros jugadores se unen:**
   - Ingresan nombre y código de sala
   - Esperan en el lobby

3. **Creador inicia partida:**
   - Se asignan roles aleatoriamente:
     - **Inocentes:** Reciben una palabra secreta
     - **Impostores:** NO reciben la palabra
   - Cada jugador ve su rol en su propio dispositivo

4. **Gameplay:**
   - Inocentes dan pistas sutiles sobre su palabra
   - Impostores intentan adivinar la palabra escuchando pistas
   - El grupo discute para identificar al impostor

5. **Terminar ronda:**
   - Creador hace clic en "Terminar Ronda"
   - La palabra usada se elimina de la lista
   - Todos vuelven al lobby para otra ronda
   - Cuando se acaban las palabras, no se pueden iniciar más rondas

## 🛠️ Tecnologías Utilizadas

- **Frontend:** HTML5, CSS3, JavaScript (Vanilla)
- **Backend:** Firebase Realtime Database
- **Hosting:** Firebase Hosting
- **Framework CSS:** Bootstrap 5.3.0
- **Gestión:** Firebase CLI

## 📁 Estructura del Proyecto

```
juego-el-impostor/
├── public/
│   ├── index.html          # Toda la aplicación (HTML + CSS + JS)
│   └── 404.html           # Página de error
├── database.rules.json    # Reglas de seguridad de Firebase
├── firebase.json          # Configuración de Firebase
├── .firebaserc           # Proyecto de Firebase configurado
└── README.md             # Este archivo
```

## ⚙️ Configuración de Firebase

### Proyecto Firebase
- **Project ID:** `juego-el-impostor`
- **Database:** Realtime Database (us-central1)
- **Hosting:** `juego-el-impostor.web.app`

### Configuración actual en código
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyCg5yaU5NepkWcLHjQkECrOUcUm85_Kus8",
    authDomain: "juego-el-impostor.firebaseapp.com",
    databaseURL: "https://juego-el-impostor-default-rtdb.firebaseio.com",
    projectId: "juego-el-impostor",
};
```

**Ubicación:** `public/index.html:158-163`

## 🔥 Estructura de Datos en Firebase

```javascript
rooms/
  {ROOM_CODE}/              // ej: "ABC123"
    creatorId: "player_123"
    status: "waiting" | "playing"
    impostorCount: 1
    currentWord: "Gato"      // Palabra actual en juego (null si waiting)
    words: ["Perro", "Luna"] // Palabras restantes
    players:
      player_123:
        id: "player_123"
        name: "Juan"
        role: "inocente" | "impostor" | null
        word: "Gato" | null
        connected: true
      player_456:
        id: "player_456"
        name: "María"
        role: "impostor"
        word: null
        connected: true
```

## ✨ Funcionalidades Implementadas

### 1. Sistema de Salas
- ✅ Crear sala con código único de 6 caracteres
- ✅ Unirse a sala existente con código
- ✅ Lobby en tiempo real (muestra jugadores conectados)
- ✅ Solo el creador puede iniciar partida

### 2. Gestión de Jugadores
- ✅ Creador puede eliminar jugadores manualmente (botón ✕)
- ✅ Detección automática de desconexión (si alguien cierra pestaña)
- ✅ Jugador eliminado es redirigido al inicio con aviso
- ✅ Si creador abandona → sala se cierra completamente

### 3. Sistema de Roles
- ✅ Asignación aleatoria de roles al iniciar partida
- ✅ Inocentes reciben palabra secreta
- ✅ Impostores NO reciben palabra
- ✅ Cada jugador solo ve su propio rol

### 4. Sistema de Rondas
- ✅ Creador puede terminar ronda (botón "Terminar Ronda")
- ✅ Palabra usada se elimina de la lista automáticamente
- ✅ Todos vuelven al lobby para siguiente ronda
- ✅ Contador de palabras restantes visible para creador
- ✅ Bloqueo de inicio si no quedan palabras

### 5. Protecciones y Validaciones
- ✅ Mínimo 2 jugadores para iniciar
- ✅ Validación de campos obligatorios
- ✅ Prevención de crear salas sin palabras
- ✅ Manejo de desconexiones inesperadas
- ✅ Limpieza automática de salas vacías

## 📍 Ubicaciones Importantes del Código

Todo el código está en: `public/index.html`

### Pantallas (HTML)
- **Inicio:** Líneas 27-39
- **Crear Sala:** Líneas 41-65
- **Unirse a Sala:** Líneas 67-87
- **Lobby:** Líneas 89-118
- **Juego (roles):** Líneas 137-153

### Funciones Principales (JavaScript)
- **Configuración Firebase:** Líneas 158-169
- **Generador de códigos:** Líneas 191-197
- **Asignación de roles:** Líneas 205-226
- **Actualizar UI del lobby:** Líneas 228-311
- **Mostrar rol del jugador:** Líneas 313-335
- **Eliminar jugador:** Líneas 347-351
- **Detección de desconexión:** Líneas 353-370

### Event Listeners
- **Crear sala:** Líneas 377-403
- **Unirse a sala:** Líneas 405-439
- **Iniciar juego:** Líneas 441-471
- **Terminar ronda:** Líneas 473-507
- **Salir de sala:** Líneas 509-530

## 🚀 Comandos de Despliegue

### Ver sitio localmente
```bash
firebase serve
```

### Desplegar a producción
```bash
firebase deploy --project juego-el-impostor
```

### Solo actualizar hosting (más rápido)
```bash
firebase deploy --only hosting --project juego-el-impostor
```

### Ver logs
```bash
firebase hosting:channel:list
```

## 🧩 Flujo de Datos en Tiempo Real

1. **Jugador hace acción** (crear sala, unirse, etc.)
2. **Se actualiza Firebase** (`.set()`, `.update()`, `.remove()`)
3. **Firebase notifica a TODOS los clientes suscritos** (`.on('value')`)
4. **Función `updateLobbyUI()` se ejecuta en cada cliente**
5. **UI se actualiza automáticamente** para todos

Este patrón permite sincronización en tiempo real sin recargar página.

## 🔒 Reglas de Seguridad Actuales

**Archivo:** `database.rules.json`

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

**⚠️ IMPORTANTE:** Las reglas actuales permiten lectura/escritura total. Para producción seria, deberías implementar:
- Validación de estructura de datos
- Prevención de sobrescritura
- Límites de tamaño
- Autenticación (opcional)

## 🐛 Problemas Conocidos / Limitaciones

- **Sin autenticación:** Cualquiera puede crear/unirse a salas
- **Sin persistencia:** Las salas se eliminan cuando todos salen
- **Sin historial:** No se guardan partidas anteriores
- **Sin chat:** No hay comunicación integrada (usar Discord/Zoom aparte)
- **Sin votación:** No hay sistema de votación para eliminar impostores
- **Sin timer:** No hay temporizador de rondas

## 💡 Ideas para Futuras Mejoras

1. **Sistema de votación:** Botones para votar quién es el impostor
2. **Temporizador:** Límite de tiempo por ronda
3. **Chat integrado:** Para dar pistas sin Discord
4. **Estadísticas:** Contador de victorias/derrotas
5. **Temas personalizados:** Diferentes categorías de palabras
6. **Sonidos:** Efectos de sonido para eventos
7. **Animaciones:** Transiciones más fluidas
8. **Modo espectador:** Ver sin jugar
9. **Historial de rondas:** Ver palabras jugadas anteriormente
10. **Configuración avanzada:** Timer, cantidad de pistas, etc.

## 🆘 Solución de Problemas Comunes

### "El botón crear sala no hace nada"
- Verificar que Firebase Realtime Database esté habilitado
- Revisar la consola del navegador (F12) para errores
- Verificar que la API key esté correcta

### "Los jugadores no se sincronizan"
- Verificar conexión a internet
- Revisar reglas de Firebase (deben permitir read/write)
- Refrescar la página (Ctrl+F5)

### "La sala desaparece inesperadamente"
- Verificar que el creador no haya cerrado su pestaña
- Revisar que la Realtime Database esté activa

### "Cache del navegador muestra versión antigua"
- Abrir en modo incógnito
- Limpiar cache (Ctrl+Shift+Del)
- Esperar 2-3 minutos después del deploy

## 📝 Notas para Desarrollo

### Filosofía del Código
- **Todo en un archivo:** Por simplicidad, todo está en `index.html`
- **Sin frameworks:** JavaScript vanilla para máxima compatibilidad
- **Firebase como única dependencia:** No hay backend tradicional
- **Tiempo real primero:** Todo se sincroniza automáticamente

### Patrón de Desarrollo
1. Hacer cambios en `public/index.html`
2. Probar localmente si es posible
3. Deploy: `firebase deploy --only hosting`
4. Esperar 1-2 minutos para propagación de cache
5. Probar en modo incógnito o limpiar cache

### Variables Globales Importantes
```javascript
currentPlayer    // Jugador local {id, name, role, word}
currentRoomCode  // Código de sala actual
isCreator        // Boolean - ¿es creador de la sala?
```

### Listeners Activos
- **`.on('value')`:** Se mantiene escuchando cambios hasta `.off()`
- **`.once('value')`:** Lee una vez y se detiene
- **`.onDisconnect()`:** Se ejecuta cuando se pierde conexión

## 📊 Estado del Proyecto

**Versión:** 1.0.0
**Última actualización:** 2025-11-27
**Estado:** ✅ Producción estable
**Jugadores soportados:** 2-20 (teórico, probado hasta 5)

## 🤝 Contexto para Próxima Sesión

Si necesitas ayuda en el futuro, aquí está lo que deberías saber:

1. **Este es un juego social multijugador** estilo "Among Us" simplificado
2. **Firebase Realtime Database es el corazón** - todo se sincroniza ahí
3. **El creador tiene control total** - puede eliminar jugadores, iniciar/terminar rondas
4. **Las palabras se van consumiendo** - cada ronda elimina la palabra usada
5. **Todo es efímero** - no hay persistencia, las salas son temporales
6. **El código está TODO en un solo archivo HTML** - `public/index.html`

### Lo que NO hace este juego
- ❌ No tiene sistema de votación automático
- ❌ No guarda historial de partidas
- ❌ No tiene chat integrado
- ❌ No tiene sistema de puntos/ranking
- ❌ No requiere autenticación/registro

### Lo que SÍ hace muy bien
- ✅ Sincronización en tiempo real
- ✅ Gestión de salas y jugadores
- ✅ Asignación aleatoria de roles
- ✅ Sistema de rondas con eliminación de palabras
- ✅ Manejo robusto de desconexiones

---

**Desarrollado para jugar con amigos en Discord/Zoom/presencial**
**Firebase Project:** `juego-el-impostor`
**Deploy URL:** https://juego-el-impostor.web.app

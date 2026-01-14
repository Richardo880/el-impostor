# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

"El Impostor" is a real-time multiplayer social deduction game (simplified Among Us style) built with vanilla JavaScript and Firebase. Players join rooms where innocents receive a secret word and give hints, while impostors try to guess the word without being discovered.

**Production URL:** https://juego-el-impostor.web.app

## Commands

```bash
# Local development server (runs on http://localhost:5000)
firebase serve

# Deploy to production
firebase deploy --project juego-el-impostor

# Deploy hosting only (faster)
firebase deploy --only hosting --project juego-el-impostor
```

**Note:** After deploying, wait 1-3 minutes for cache propagation. Test in incognito mode.

## Architecture

### Single-File Application
The entire application lives in `public/index.html` (~567 lines containing HTML, CSS, and JavaScript). No build process, no npm dependencies.

### Tech Stack
- **Frontend:** Vanilla JavaScript, Bootstrap 5.3.0 (CDN)
- **Backend:** Firebase Realtime Database (no server code)
- **Hosting:** Firebase Hosting

### Real-Time Data Flow Pattern
```
User Action → Firebase Update (.set/.update/.remove)
                    ↓
        Firebase notifies ALL clients (.on('value'))
                    ↓
        updateLobbyUI() executes on each client
                    ↓
            DOM updates automatically
```

### Firebase Data Structure
```
rooms/{ROOM_CODE}/
  ├── creatorId: string
  ├── status: "waiting" | "playing"
  ├── impostorCount: number
  ├── currentWord: string | null
  ├── words: string[]
  └── players/{playerId}/
      ├── id, name, role, word, connected
```

### Key Global Variables (JavaScript)
- `currentPlayer` - Local player object {id, name, role, word}
- `currentRoomCode` - Active room code (6 chars)
- `isCreator` - Boolean, controls UI permissions
- `database`, `roomsRef` - Firebase references

### Screen Management
Five screens managed by `showScreen(name)`: `home`, `create`, `join`, `lobby`, `game`

### Presence/Disconnection Handling
- Creator disconnect: entire room is deleted via `.onDisconnect().remove()`
- Player disconnect: only their player entry is removed
- Eliminated players are redirected to home with alert

## Code Locations (public/index.html)

| Section | Lines |
|---------|-------|
| HTML Screens | 27-153 |
| Firebase Config | 158-169 |
| Role Assignment Logic | 205-226 |
| Lobby UI Updates | 228-311 |
| Player Elimination | 347-351 |
| Event Listeners | 377-530 |

## Design Decisions

- **Ephemeral rooms:** No persistence, rooms exist only while players are connected
- **No authentication:** Open access by design (for friends playing together)
- **Creator as game master:** Only creator can start game, end rounds, eliminate players
- **Word consumption:** Each round removes the used word from the list
- **Firebase-first:** Database is the single source of truth, not local state

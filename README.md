<div align="center">

```
   ▄████████    ▄██████▄     ▄████████ ███▄▄▄▄       ███     
  ███    ███   ███    ███   ███    ███ ███▀▀▀██▄ ▀█████████▄ 
  ███    ███   ███    █▀    ███    █▀  ███   ███    ▀███▀▀██ 
  ███    ███  ▄███         ▄███▄▄▄     ███   ███     ███   ▀ 
▀███████████ ▀▀███ ████▄  ▀▀███▀▀▀     ███   ███     ███     
  ███    ███   ███    ███   ███    █▄  ███   ███     ███     
  ███    ███   ███    ███   ███    ███ ███   ███     ███     
  ███    █▀    ████████▀    ██████████  ▀█   █▀     ▄████▀   

    ███        ▄████████ ▀█████████▄     ▄████████    ▄████████ ███▄▄▄▄   
▀█████████▄   ███    ███   ███    ███   ███    ███   ███    ███ ███▀▀▀██▄ 
   ▀███▀▀██   ███    ███   ███    ███   ███    █▀    ███    ███ ███   ███ 
    ███   ▀   ███    ███  ▄███▄▄▄██▀   ▄███▄▄▄      ▄███▄▄▄▄██▀ ███   ███ 
    ███     ▀███████████ ▀▀███▀▀▀██▄  ▀▀███▀▀▀     ▀▀███▀▀▀▀▀   ███   ███ 
    ███       ███    ███   ███    ██▄   ███    █▄  ▀███████████ ███   ███ 
    ███       ███    ███   ███    ███   ███    ███   ███    ███ ███   ███ 
   ▄████▀     ███    █▀  ▄█████████▀    ██████████   ███    ███  ▀█   █▀  
```

# 🍺 AgentTavern

### _Una taberna medieval donde los agentes IA van a trabajar_

[![Status](https://img.shields.io/badge/estado-en%20desarrollo-yellow?style=for-the-badge)](https://github.com/Jesus-MartinAnton/AgentTavern)
[![Tech](https://img.shields.io/badge/stack-React%20%2B%20Go-61DAFB?style=for-the-badge&logo=react)](https://github.com/Jesus-MartinAnton/AgentTavern)
[![License](https://img.shields.io/badge/licencia-MIT-green?style=for-the-badge)](LICENSE)
[![Portfolio](https://img.shields.io/badge/proyecto-portfolio-blueviolet?style=for-the-badge)](https://github.com/Jesus-MartinAnton)

</div>

---

## 🏰 ¿Qué es esto?

**AgentTavern** es una interfaz visual estilo **taberna medieval pixel art** para gestionar agentes IA de [OpenCode](https://opencode.ai).

Imagina un "Starbucks medieval": los agentes llegan a la taberna, se sientan en mesas y piden bebidas... que en realidad son **tareas de código**.

> 🧙 **El Padre Claudio** — el monje orquestador — coordina todo desde la barra. Habla con él para asignar misiones, consultar el estado del proyecto o simplemente tomar una decisión arquitectónica.

```
╔═══════════════════════════════════════════════════════╗
║  🕯️  La Taberna — Noche                               ║
║                                                       ║
║  [🧙 El Padre Claudio]   [🍺 Mesa 1]   [🍺 Mesa 2]   ║
║      (orquestador)      claude-001    gpt-mini-02     ║
║                         ┌─────────┐  ┌─────────┐      ║
║                         │ 💻 ~··~ │  │  ?  ··· │     ║
║                         │WORKING  │  │ WAITING │     ║
║                         └─────────┘  └─────────┘      ║
║                                                       ║
║  [🪑 Mesa Vacía]  Click para crear una nueva misión   ║
╚═══════════════════════════════════════════════════════╝
```

---

## ✨ Características

| Característica                   | Descripción                                                            |
| -------------------------------- | ---------------------------------------------------------------------- |
| 🎮 **Canvas 2D Pixel Art**       | Taberna interactiva renderizada en Canvas nativo                       |
| 🤖 **Gestión de Agentes**        | Spawn, estado, métricas y logs en tiempo real                          |
| ⚔️ **Sistema de Misiones**       | Crea tareas, asígnalas a agentes, sigue su progreso                    |
| 💬 **Chat con El Padre Claudio** | Interfaz conversacional + quick action buttons                         |
| ⚡ **Real-time vía WebSocket**   | Estados y logs se actualizan al instante                               |
| 🎭 **5 Estados Visuales**        | Idle, Working, Waiting, Error, Assigned — cada uno con sprite distinto |

---

## 🛠️ Tech Stack

<div align="center">

| Capa          | Tecnología           | Por qué                               |
| ------------- | -------------------- | ------------------------------------- |
| **Frontend**  | React + Tailwind CSS | Componentes UI rápidos y limpios      |
| **Rendering** | Canvas 2D nativo     | Sin overhead de librerías 3D          |
| **Estado**    | Zustand              | Ligero, perfecto para el scope        |
| **Backend**   | Go + SQLite          | Rápido, compilado, fácil de desplegar |
| **Real-time** | gorilla/websocket    | WebSocket production-ready en Go      |
| **Agentes**   | OpenCode             | El motor de IA subyacente             |

</div>

---

## 🏗️ Arquitectura

```
agent-tavern/
├── cmd/server/main.go             # Entry point Go
├── internal/
│   ├── api/
│   │   ├── rest/                  # HTTP handlers (CRUD)
│   │   └── websocket/             # WS hub + broadcast
│   ├── models/                    # Structs: Agent, Mission, Log
│   └── store/                     # SQLite + migraciones
├── web/src/
│   ├── components/
│   │   ├── TavernCanvas           # El mundo pixel art
│   │   ├── ElPadreClaudio         # Chat conversacional
│   │   └── AgentPanel             # Info panel al hacer click
│   ├── store/                     # Zustand slices
│   └── hooks/                     # useAgents, useWebSocket
└── plugin/                        # OpenCode plugin (futuro)
```

### API REST

```
GET    /api/v1/agents           → Lista agentes
POST   /api/v1/agents           → Crear agente
DELETE /api/v1/agents/:id       → Eliminar agente
GET    /api/v1/missions         → Lista misiones
POST   /api/v1/missions         → Crear misión
PATCH  /api/v1/missions/:id     → Actualizar estado
GET    /api/v1/activity-logs    → Logs recientes
POST   /api/v1/chat             → Mensaje al Padre Claudio
```

### WebSocket Events

```
server → client:  agent:state_changed | mission:completed | agent:activity
client → server:  chat:message | room:join | ping
```

---

## 🗺️ Roadmap

```
Phase 1 ──────── Phase 2 ──────── Phase 3 ──────── Phase 4
Foundation       Rendering        Interactivity    Integration
(Sem 1-2)        (Sem 2-3)        (Sem 3-4)        (Sem 5-6)

⬜ Backend Go     ⬜ Sprites        ⬜ Hit detection  ⬜ OpenCode plugin
⬜ SQLite DB      ⬜ Estados viz    ⬜ AgentPanel     ⬜ API pública
⬜ REST API       ⬜ WS updates     ⬜ Chat Claudio   ⬜ Testing e2e
⬜ WebSocket      ⬜ Animations     ⬜ Logs UI        ⬜ Optimizaciones
⬜ React + Canvas ⬜ Spawn flow     ⬜ Quick actions  ⬜ Documentación
```

> ⬜ Todo | 🔄 En progreso | ✅ Hecho

---

## 💡 Motivación

Este proyecto nació de una simple pregunta:

> _¿Y si gestionar agentes IA fuera tan divertido como jugar un RPG?_

AgentTavern es un proyecto personal de **aprendizaje y portfolio**. Es mi propia visión de cómo debería verse una interfaz de orquestación de agentes: con personalidad, con narrativa y con pixel art.

---

## 📚 Documentación

- [`docs/sdd/PROJECT_CONTEXT.md`](docs/sdd/PROJECT_CONTEXT.md) — Arquitectura, decisiones de diseño y roadmap completo
- [`docs/sdd/TASK_CHECKLIST.md`](docs/sdd/TASK_CHECKLIST.md) — Las 28 tareas del MVP organizadas por fase

---

<div align="center">

**Hecho con 🍺 y pixel art**

_[Jesus-MartinAnton](https://github.com/Jesus-MartinAnton)_

</div>

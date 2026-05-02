# AgentTavern - Project Context

> Documento maestro con todas las decisiones, arquitectura y roadmap del proyecto.

---

## Concepto

**AgentTavern** es una interfaz visual estilo "taverna medieval pixel art" para gestionar agentes IA de OpenCode. Los agentes son personajes que trabajan en mesas, las tareas son jarras/bebidas, y El Padre Claudio es el orquestador con el que el usuario conversa.

**Metafora**: "Starbucks medieval" — sin rigor histórico, solo personalidad. Los agentes son "clientes" que vienen a trabajar. Las jarras son tareas. El Padre Claudio es el dueño que coordina todo.

---

## Tech Stack

| Capa | Tecnología | Notas |
|------|------------|-------|
| Frontend | React + Tailwind CSS | |
| Rendering | Canvas 2D nativo | NO Three.js |
| Backend | Go + SQLite | |
| WebSocket | gorilla/websocket | |
| Estado global | Zustand | |
| Persistencia | SQLite (modernc.org/sqlite) | Pure Go, sin CGO |
| Agentes | Solo OpenCode | |

---

## Decisiones Clave

### Estética Visual
- **2D Pixel Art** Full Color (NO Gameboy green)
- **Taberna de NOCHE** con velas, chimenea, ambiente cálido
- **Sprites**: 32x32 píxeles, 5 estados visuales
- **Sistema de estados**:
  - Con jarra = tarea asignada
  - Sin jarra + "?" = esperando input
  - Trabajando = laptop abierto, animación typing
  - Idle = laptop cerrado, reclinado
  - Error = de pie, papeles desordenados

### El Padre Claudio
- **Nombre**: "El Padre Claudio"
- **Rol**: Monje/clérigo sabio y atento - Orquestador
- **Función**: Chat conversacional con el usuario
- **Chat híbrido**: Input texto libre + quick action buttons
- **Ubicación**: En el Canvas, clickeable para abrir chat

### Sistema de Interacción
- **Click en AGENTE** → Panel de info (estado, tarea, métricas, logs) + acciones
- **Click en PADRE CLAUDIO** → Chat conversacional
- **Click en MESA VACÍA** → Crear tarea y asignarla

### Arquitectura de Comunicación
- **REST** para CRUD (crear/eliminar agentes, misiones)
- **WebSocket** para eventos en tiempo real (cambio de estado, logs)
- **Plugin OpenCode** como sidecar HTTP

---

## Modelo de Datos

### Agent
```go
type Agent struct {
    ID                 string    // UUID
    Name               string    // "claude-sonnet-001"
    State              string    // idle|working|waiting|error|assigned
    WorkingDir         string    // Directorio de trabajo
    TableID           *string   // Mesa asignada
    MissionID         *string   // Misión actual
    MissionsCompleted int      // Métricas
    MissionsFailed    int      // Métricas
    CreatedAt         time.Time
    UpdatedAt         time.Time
}
```

### Mission
```go
type Mission struct {
    ID          string    // UUID
    AgentID     string    // FK a Agent
    Description string    // Descripción de la tarea
    Status      string    // pending|assigned|in_progress|completed|failed
    Priority    string    // low|normal|high
    CreatedAt   time.Time
    UpdatedAt   time.Time
}
```

### ActivityLog
```go
type ActivityLog struct {
    ID        string    // UUID
    AgentID   string    // FK a Agent
    EventType string    // stdout|stderr|system|chat|error
    Message   string    // Mensaje
    Metadata  *string   // JSON blob
    CreatedAt time.Time
}
```

---

## Roadmap de Implementación

### Phase 1: Foundation (Semanas 1-2)
- Backend Go con SQLite, API REST, WebSocket
- Frontend React + Canvas vacío

### Phase 2: Agent Rendering (Semanas 2-3)
- Sprites aparecen en Canvas
- Estados visuales funcionan

### Phase 3: Interactivity (Semanas 3-4)
- Click en agentes → panel info
- Chat con El Padre Claudio

### Phase 4: OpenCode Integration + Polish (Semanas 5-6)
- Plugin conecta
- Documentación, testing, optimizaciones

---

## Arquitectura del Proyecto

```
agent-tavern/
├── cmd/server/main.go          # Entry point Go
├── internal/
│   ├── api/
│   │   ├── rest/               # HTTP handlers
│   │   └── websocket/           # WS hub + clients
│   ├── models/                  # Structs
│   ├── store/                   # SQLite + migrations
│   └── room/                    # Sala/mesa management
├── web/                         # React frontend
│   └── src/
│       ├── components/          # TavernCanvas, ElPadreClaudio, AgentPanel
│       ├── store/               # Zustand slices
│       ├── api/                 # REST client + WebSocket hook
│       └── hooks/               # Canvas renderer, WS connection
└── plugin/                      # OpenCode plugin (future)
```

---

## Decisiones de Diseño

1. **SQLite vs PostgreSQL**: SQLite (MVP) → PostgreSQL si escala. Diseñar para migración.
2. **Canvas library**: Raw Canvas 2D API (no PixiJS, no Three.js)
3. **State management**: Zustand (suficiente para el scope)
4. **UI panels**: HTML overlay sobre Canvas (más fácil de construir)
5. **Sprites**: Placeholders procedurals primero, pixel art real después
6. **Auto-assign**: Manual por ahora, automatizado con cola de bebidas (future)
7. **Multi-room**: Una sala por servidor (MVP)
8. **Logs TTL**: 30 días

---

## API Endpoints

### REST
```
GET    /api/v1/agents              # Lista agentes
POST   /api/v1/agents              # Crear agente
GET    /api/v1/agents/:id          # Detalle agente
DELETE /api/v1/agents/:id          # Eliminar agente
GET    /api/v1/missions            # Lista misiones
POST   /api/v1/missions            # Crear misión
GET    /api/v1/missions/:id       # Detalle misión
PATCH  /api/v1/missions/:id        # Actualizar estado
GET    /api/v1/activity-logs       # Logs recientes
POST   /api/v1/chat                # Guardar mensaje chat
```

### WebSocket Events
```
Server → Client:
  agent:state_changed, agent:activity, mission:assigned, mission:completed,
  table:seated, table:vacated, ping

Client → Server:
  chat:message, ping, room:join
```

---

## Métricas de Éxito MVP

- Tiempo de carga: < 3 segundos
- FPS Canvas: ≥ 30 fps
- Latencia WebSocket: < 100ms
- Registro de agente: < 2 segundos
- Respuesta de chat: < 1 segundo

---

## Enlaces

- Repo: https://github.com/Jesus-MartinAnton/AgentTavern
- Documentación SDD: `docs/sdd/`
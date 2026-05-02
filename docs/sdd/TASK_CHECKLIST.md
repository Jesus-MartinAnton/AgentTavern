# AgentTavern - Task Checklist

> Lista de 28 tareas organizadas por fase para implementar el MVP.

## Fase 1: Foundation (Semanas 1-2)

### Backend Go + SQLite

- [ ] **1.1** Crear estructura de directorios Go: `cmd/server`, `internal/api`, `internal/models`, `internal/store` + `go.mod`
- [ ] **1.2** Implementar modelos Go: `Agent`, `Mission`, `ActivityLog` en `internal/models/`
- [ ] **1.3** Crear schema SQLite con 4 tablas + índices (agents, missions, activity_logs, chat_messages)
- [ ] **1.4** Implementar `internal/store/` con operaciones CRUD para Agent, Mission, ActivityLog
- [ ] **1.5** Crear `internal/api/routes.go` con endpoints REST: GET/POST /agents, POST /spawn, GET /logs
- [ ] **1.6** Implementar WebSocket server en `internal/api/websocket.go` para updates en tiempo real

### Frontend React + Canvas

- [ ] **1.7** Crear proyecto React + Tailwind en `web/src/` con estructura de componentes (`npm create vite@latest`)
- [ ] **1.8** Implementar Canvas 2D vacío en `web/src/components/GameCanvas.tsx` con ref y context

---

## Fase 2: Agent Rendering (Semanas 2-3)

- [ ] **2.1** Definir constantes de estados: `IDLE`, `WORKING`, `WAITING`, `ERROR`, `ASSIGNED` en frontend
- [ ] **2.2** Crear componente `AgentSprite` que renderiza según estado (placeholders primero)
- [ ] **2.3** Implementar `useAgents()` hook en Zustand que consume GET /agents
- [ ] **2.4** Conectar WebSocket para actualizar estado de agentes en tiempo real
- [ ] **2.5** Implementar render loop en Canvas con `requestAnimationFrame`
- [ ] **2.6** Agregar lógica de spawn: POST /spawn crea agente y rendering inmediato
- [ ] **2.7** Implementar transición visual entre estados (color/animación suave)
- [ ] **2.8** Agregar log de actividad en backend: cada acción de agente se registra

---

## Fase 3: Interactivity (Semanas 3-4)

- [ ] **3.1** Implementar hit detection en Canvas: click → coordenadas → agente
- [ ] **3.2** Crear `AgentInfoPanel` componente que muestra: nombre, estado, misión actual, timestamp
- [ ] **3.3** Implementar click en "El Padre Claudio" (sprite especial) → abre chat conversacional
- [ ] **3.4** Crear componente `ChatWindow` con historial de mensajes y input
- [ ] **3.5** Implementar "quick actions" en chat: botones predefinidos
- [ ] **3.6** Endpoint REST para chat: `POST /chat` guarda mensaje y `GET /chat/:agentId` recupera historial
- [ ] **3.7** Mostrar logs de actividad en UI: `GET /logs` con scroll y filtro por agente

---

## Fase 4: OpenCode Integration + Polish (Semanas 5-6)

- [ ] **4.1** Crear endpoint `GET /plugin/manifest` que devuelve capacidades del sistema
- [ ] **4.2** Implementar endpoint `POST /plugin/command` para que plugins externos controlen agentes
- [ ] **4.3** Documentar API pública en `README.md` con ejemplos de integración
- [ ] **4.4** Testing e2e: flujo completo spawn → click → chat → logs
- [ ] **4.5** Optimización: batching de updates WS, cleanup de listeners, memoización de componentes

---

## Dependencias entre tareas

```
1.1 → 1.2 → 1.3 → 1.4 → 1.5 → 1.6
                                       ↓
1.7 → 1.8 → 2.1 → 2.2 ───────────────┘
                 ↓
            2.3 ← 2.4 (paralelo)
                 ↓
            2.5 → 2.6 → 2.7 → 2.8
                 ↓
3.1 → 3.2
3.1 → 3.3 → 3.4 → 3.5
            ↓
3.6 ← (paralelo con 3.4, 3.5)
                 ↓
3.7
                 ↓
4.1 → 4.2 → 4.3 → 4.4 → 4.5
```

---

## Tareas paralelizables

- **Semana 2**: 1.7 y 1.8 pueden trabajarse en paralelo con el backend (1.5, 1.6)
- **Semana 3**: 2.2, 2.3, 2.4 son independientes pero convergen en 2.5
- **Semana 4**: 3.4 y 3.5 pueden hacerse en paralelo

---

## Criterios de DoD por tarea

Cada tarea debe cumplir:
- Código compilable/funcionando
- Tests básicos si aplica
- Commit descriptivo en GitHub
- Update de este documento marcando como completada
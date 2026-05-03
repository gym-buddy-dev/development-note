This is a critical distinction to make. Let me give you the **brutally honest reality** of what Copilot (and AI coding assistants in general) can actually do for a project of this magnitude, plus a complete prompt strategy.
Based on the current state of AI coding tools (2025–2026), here's the **brutally honest reality** of what Copilot can and **cannot** do for a project of this magnitude — and what you should use instead.

---

## What GitHub Copilot Actually Is

Copilot is an **inline code completion engine** that suggests the next few lines based on your cursor position. Even with Copilot Chat and `@workspace`, it has **fundamental architectural limitations** that make it unsuitable as the primary tool for building an IESVE-class platform.

### What Copilot CAN Do for This Project

| Task | How Well | Example Prompt |
|------|----------|--------------|
| **Boilerplate React components** | ✅ Excellent | "Create a TypeScript React component for a 3D model viewer using Three.js with orbit controls" |
| **Python API wrappers** | ✅ Very Good | "Write a Python FastAPI endpoint that receives an IFC file, parses it with IfcOpenShell, and returns a JSON list of thermal zones" |
| **EnergyPlus IDF generation** | ✅ Good | "Write a Python function that generates an EnergyPlus IDF text block for a rectangular zone with given dimensions, U-values, and window-to-wall ratio" |
| **Unit tests** | ✅ Good | "/tests — generate pytest unit tests for this geometry validation function" |
| **Code explanation** | ✅ Good | "Explain how this heat balance algorithm works line by line" |
| **SQL schema design** | ✅ Good | "Write a SQLite schema for storing hourly energy simulation results with zone-level granularity" |
| **Docker containers** | ✅ Good | "Write a Dockerfile that installs EnergyPlus 23.2 and exposes a Python API" |
| **Simple data parsing** | ✅ Good | "Parse an EPW weather file and extract dry-bulb temperature as a Pandas DataFrame" |

### What Copilot CANNOT Do (Critical Blockers)

| Task | Why It Fails |
|------|-------------|
| **Architect the entire application** | Copilot sees individual files, not system design. It cannot decide "should we use WebGL or server-side rendering?" or "how do we couple EnergyPlus with OpenFOAM?" |
| **Multi-engine integration** | It cannot reason across C++ (EnergyPlus), C (Radiance), Python (glue), and TypeScript (frontend) simultaneously to design data contracts |
| **CAD kernel development** | B-rep geometry algorithms, Boolean operations, meshing — requires specialized computational geometry knowledge Copilot doesn't have |
| **Physics engine coupling** | Cannot design the heat/mass transfer boundary conditions between thermal simulation and CFD |
| **Complex multi-step reasoning** | GitHub's own docs confirm: *"struggles with complex multi-step reasoning, often requiring developer intervention"* |
| **Project-wide refactoring** | Multi-file context is severely limited. `@workspace` exists but "hardly does what you expect if files are too big" (Stack Overflow, 2024) |
| **Compliance rule engines** | Cannot encode ASHRAE 90.1 or Part L compliance logic — this requires domain expertise in building codes |
| **Ensure physical correctness** | It can generate code that *runs* but produces physically meaningless results (e.g., violating conservation of energy) |

---

## Better AI Tools for This Project (2026 Reality)

Based on independent 30-day production tests, here's what developers actually found:

### Claude Code (Anthropic) — **Best for this project**
- **What it does**: Terminal-based agent that understands your **entire codebase**, edits files autonomously, runs commands, asks clarifying questions
- **Why it wins for your project**: "In complex, multi-step tasks, Copilot falls behind. Cursor does a decent job, but Claude Code performs the best" — tested on real backends
- **Real example from testing**: Decomposed a 600-line service into 4 clean modules with full test coverage in 2 hours vs. estimated full day
- **Best prompt style**: High-level intent with context — it will ask clarifying questions before touching anything
- **Cost**: $17–$25/month | **Enterprise**: $20–$25/seat

### Cursor — **Best IDE experience**
- **What it does**: Full AI-native IDE (VS Code fork) with codebase indexing, multi-file agentic edits, background cloud agents
- **Why it's good**: Native Composer for multi-file edits, automatic checkpoint/rollback, excellent for greenfield development
- **Limitation**: It *is* the IDE — your team must standardize on it. $40/seat/month for teams
- **Best for**: Frontend + backend together, rapid prototyping

### Copilot — **Best for simple acceleration**
- **Why it's weakest for this project**: "Copilot makes you faster at what you already know how to do. It doesn't help you figure out what to do when you're genuinely uncertain"
- **Context window**: Still narrowest of the three. Doesn't understand project-level architecture
- **Only use it for**: TypeScript/React boilerplate, Python utilities, tests — not architecture

---

## The Honest Verdict: What AI Can & Cannot Do for IESVE-Scale Software

```
┌─────────────────────────────────────────────────────────────────┐
│  CANNOT BUILD WITH AI ALONE — NO EXCEPTIONS                       │
├─────────────────────────────────────────────────────────────────┤
│  These require human domain expertise:                            │
│                                                                   │
│  ❌ Designing the central building data model (B-rep + thermal)   │
│  ❌ Coupling EnergyPlus ↔ OpenFOAM boundary conditions            │
│  ❌ HVAC network solver topology and convergence logic            │
│  ❌ Daylight coefficient matrix methods                           │
│  ❌ ASHRAE/CIBSE compliance rule engines                          │
│  ❌ Geometry kernel (Boolean ops, surface meshing)                │
│  ❌ Ensuring conservation of energy in custom solvers             │
│  ❌ 3D graphics engine architecture (WebGL scene graph)            │
│                                                                   │
│  These are $500K+ salary decisions, not prompt engineering tasks   │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  CAN ACCELERATE WITH AI — SPECIFIC USE CASES                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ✅ Individual UI components (React + Three.js)                 │
│  ✅ Python glue code between existing engines                     │
│  ✅ IFC/gbXML parsing utilities                                   │
│  ✅ API endpoint scaffolding (FastAPI)                            │
│  ✅ Database schemas and queries                                  │
│  ✅ Test suites for geometry functions                            │
│  ✅ Dockerfile / CI-CD pipelines                                  │
│  ✅ Charting/visualization components (Plotly/D3)                 │
│  ✅ Scripting API wrappers                                        │
│  ✅ Documentation generation                                      │
│                                                                   │
│  AI gives you 2x–3x speedup on implementation, NOT architecture  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Recommended Prompt Strategy (Claude Code / Cursor)

Since Copilot is too limited, here are **production-tested prompts** for the tools that *can* help:

### 1. Project Scaffolding (Claude Code)

```text
I need to scaffold a building performance simulation desktop application 
with the following architecture:

- Frontend: Electron + React + TypeScript + Three.js for 3D viewport
- Backend: Python FastAPI server
- Simulation orchestrator: Python that calls EnergyPlus CLI and parses outputs
- Data: SQLite for project metadata, HDF5 for hourly time-series results
- Geometry: IfcOpenShell Python bindings for IFC import

Create the directory structure, package.json, Python requirements, and 
a minimal IPC bridge between Electron and FastAPI. Use a clean hexagonal 
architecture with separation between domain models and simulation engines.

Ask me clarifying questions before generating anything.
```

### 2. EnergyPlus Integration Module (Claude Code)

```text
Create a Python module `simulation/energyplus_bridge.py` that:

1. Accepts a building model JSON (schema: {zones: [...], surfaces: [...], 
   constructions: [...], schedules: [...]})
2. Generates a valid EnergyPlus 23.2 IDF file using the ExpandObjects method
3. Runs EnergyPlus via subprocess with the generated IDF and an EPW weather file
4. Parses the ESO/CSV output into a Pandas DataFrame with columns:
   [timestamp, zone_name, dry_bulb_temp, heating_rate_w, cooling_rate_w]
5. Returns the DataFrame and total annual energy use

Use type hints, pydantic models for input validation, and handle 
EnergyPlus execution errors gracefully. Write comprehensive pytest tests.
```

### 3. 3D Viewer Component (Cursor Composer)

```text
Create a React + Three.js component `ModelViewer.tsx` that:

- Loads a building model from a JSON format with surfaces defined as 
  arrays of {x,y,z} vertices
- Renders each surface as a mesh with different colors by type 
  (wall=gray, window=blue, roof=red, floor=brown)
- Implements OrbitControls for camera navigation
- Allows clicking a surface to highlight it and emit a callback 
  with the surface ID
- Shows a simple outliner tree on the left with zones and their surfaces
- Uses React Three Fiber (@react-three/fiber)

Make it a single self-contained file with TypeScript types. Include a 
mock data loader for testing.
```

### 4. Results Database Schema (Claude Code)

```text
Design a SQLite schema for storing building simulation results with 
these requirements:

- Projects table: id, name, location, weather_file_path, created_at
- Zones table: id, project_id, zone_name, volume_m3, floor_area_m2
- Hourly results: 8760 rows per zone per simulation run with columns 
  for temperature, heating/cooling loads, lighting energy, equipment energy
- Support multiple simulation runs per project (parametric studies)
- Support daylighting metrics (illuminance, daylight autonomy) per sensor point
- Include indexes for fast time-series queries by zone and date range

Write the SQL DDL, SQLAlchemy ORM models, and a query builder for 
retrieving monthly aggregated energy use by fuel type.
```

### 5. Geometry Validation (Claude Code)

```text
Write a Python module `geometry/validation.py` that validates a building 
geometry model for thermal simulation:

Input: List of zones, each with surfaces (each surface has vertices, 
type, construction_id, adjacent_zone or "outside")

Validation rules:
1. Every zone must be fully enclosed (no surface gaps > 0.01m)
2. All surfaces must have consistent outward-facing normals
3. Windows must be co-planar with and smaller than their parent walls
4. Shared surfaces between zones must have matching geometry and 
   reversed normals
5. Total wall area per zone must equal sum of parent wall areas of windows

Return a ValidationReport with is_valid boolean and a list of errors 
with severity (error/warning) and location references.

Use shapely for 2D polygon operations where needed. Write tests with 
at least 5 edge cases including non-manifold geometry.
```

---

## Realistic AI-Assisted Workflow

Here's how a **senior engineering team** should actually use AI for this project:

| Phase | Human Responsibility | AI Assistance |
|-------|---------------------|-------------|
| **Architecture** | Design data model, engine coupling, tech stack | None — this is human-only |
| **Scaffolding** | Review directory structure, package choices | Claude Code generates initial structure |
| **UI Components** | Specify behavior, review accessibility | Cursor/Copilot generates React components |
| **Engine Glue** | Design data contracts, error handling | Claude Code generates Python integration |
| **Simulation Logic** | Validate physics correctness | AI generates code; engineers verify |
| **Tests** | Define test cases and acceptance criteria | AI generates pytest/Jest suites |
| **Refactoring** | Identify code smells and target structure | Claude Code executes multi-file refactors |
| **Debugging** | Diagnose root cause | AI suggests fixes; human validates |

---

## What You Actually Need (Beyond AI)

No AI tool can replace this:

| Requirement | Why |
|-------------|-----|
| **1 Senior Building Physicist** | Must validate that thermal simulation logic is physically correct |
| **1 CFD Engineer** | Must ensure OpenFOAM setup, turbulence models, and boundary conditions are valid |
| **1 Computational Geometry Expert** | For CAD kernel, surface meshing, and Boolean operations |
| **1 HVAC Engineer** | For system topology, controls logic, and equipment sizing rules |
| **2–3 Senior Full-Stack Developers** | To architect and own the frontend, backend, and integration layers |
| **1 DevOps/Platform Engineer** | For simulation orchestration, queue management, cloud scaling |

**AI can make your developers 2x faster. It cannot replace domain expertise.**

---

## Bottom Line

| Tool | Role in This Project |
|------|---------------------|
| **GitHub Copilot** | Speed up individual file edits in TypeScript/React/Python — **not** architecture |
| **Claude Code** | Best for complex backend work, multi-file refactoring, debugging, autonomous tasks |
| **Cursor** | Best for frontend development, greenfield UI, codebase exploration |
| **Human Architects** | **Non-negotiable** for system design, physics validation, engine integration |

**The realistic workflow**: A senior team designs the architecture, writes the critical interfaces, and specifies components. Then AI (primarily Claude Code for backend, Cursor for frontend) accelerates implementation of those well-specified pieces. Copilot is useful only as a minor speed boost for boilerplate.

If you try to prompt Copilot to "build an IESVE clone," you will get broken, disconnected code snippets that don't integrate, don't scale, and don't produce physically valid results. **This is a human-led, AI-accelerated project — not an AI-led project.**

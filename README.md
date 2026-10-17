# Division Drone — Gendarmerie

Web application developed during the **Gendarmerie Hackathon** to support the planning and simulated coordination of multi-drone operations.

## Live Demo

### [Try Division Drone](https://drone-planning.vercel.app)

No installation is required. The application is primarily designed for desktop use.

> **Language:** The application interface is intentionally in **French**, as the project was developed for the **French Gendarmerie** and designed around its operational use case.

> **Simulation only:** Division Drone is a decision-support prototype. It does not control real drones and does not constitute a flight authorization or an aviation safety system.

---

## Overview

Division Drone supports an operation through two main phases:

**Mission Planning**
- Define the operation area.
- Identify priority zones.
- Account for date, time, and weather conditions.
- Estimate the required resources.
- Automatically propose a coverage plan.
- Adapt the plan to the resources actually available.

**Operation Coordination**
- Visualize drones, pilots, and working areas.
- Modify positions, altitudes, headings, and coverage sectors.
- Recompute coverage dynamically.
- Detect conflicts.
- Handle unavailable or reserve drones.
- Simulate return-to-home and emergency landing procedures.

---

## Workflow

From the home page:

### Plan a Mission

1. Enter the mission information.
2. Draw the **operation area** on the map.
3. Optionally define **priority zones**.
4. Run the resource estimation.
5. Review the proposed coverage plan and metrics.
6. Enter the resources actually available.
7. Recompute the plan if necessary.
8. Launch the operation simulation.

### Resume a Mission

A previously exported mission can be imported using the versioned JSON format (`schemaVersion 1.0`).

A demonstration mission is also included in the project.

---

## Main Features

### Mapping

- OpenStreetMap basemap.
- IGN orthophotos.
- Address search.
- Operation and priority zone drawing.
- Drone and pilot visualization.
- Circular and directional coverage sectors.
- Overlap and uncovered-area visualization.
- Experimental terrain occupancy analysis.

### Automated Mission Planning

The planning engine uses a **deterministic greedy coverage heuristic**.

It samples the operation area, generates candidate placements, and progressively selects positions providing the highest additional coverage value.

Priority areas receive a higher weight than standard areas, while unnecessary overlap is penalized.

By default, the planner aims for:

- **98%** overall coverage.
- **99%** priority-area coverage.

The engine then assigns available drone models and determines compatible altitudes while accounting for overlaps and operational ceilings.

> The planner is designed as a fast and explainable heuristic for the prototype. It does not guarantee a globally optimal solution.

### Operation Coordination

During the simulation, operators can:

- Move drones and pilots.
- Adjust altitude, heading, and working angle.
- Deploy or return drones to reserve.
- Mark units as unavailable.
- Recompute coverage and conflicts immediately.
- Perform limited local reorganization.
- Simulate mission completion.
- Trigger an emergency landing.

Recalculations are designed to avoid unnecessary changes to the rest of the deployment.

---

## Prototype Flight Scenarios

| Scenario | Maximum Range | Rural Ceiling | Urban Ceiling |
| --- | ---: | ---: | ---: |
| TP1 — Visual Line of Sight | 500 m | 120 m | 120 m |
| TP1 — Beyond Visual Line of Sight | 1,000 m | 50 m | 80 m |
| TP2 — Beyond Visual Line of Sight | 2,000 m | 120 m | 120 m |

These values are **prototype operational rules** and should not be interpreted as aviation regulations.

The default minimum vertical separation is **30 m**, with a maximum overlap threshold of **20%**.

---

## Technology Stack

- **React 19**
- **TypeScript**
- **Vite**
- **Leaflet**
- **OpenStreetMap**
- **IGN**
- **Open-Meteo**

The application runs primarily in the browser and requires no business backend.

Planning and operation engines are kept separate from the React interface so that the core algorithms can be tested independently from the map rendering.

---

## Architecture

```text
src/
├── app/                 Application state and simulation
├── components/          Map, panels, and UI components
├── pages/               Home, planning, resources, and operations
├── engines/
│   ├── geometry/        Geometry, clipping, and coverage
│   ├── planning/        Placement and resource estimation
│   └── operations/      Conflicts and dynamic recalculation
├── services/            Weather, geocoding, JSON, and preferences
├── types/               TypeScript contracts
└── data/                Drone catalog and demo mission
```

---

## External Data & Services

The application's main features work without API keys.

| Service | Purpose |
| --- | --- |
| OpenStreetMap | Map tiles |
| IGN | Orthophotos and address search |
| Open-Meteo | Weather forecasts |

Day/night status is computed locally.

Terrain occupancy analysis is experimental. A precomputed analysis for **Mirepoix (INSEE 09194)** is included in the project. Dynamic analysis of other areas requires a separate external service.

---

## Mission Import / Export

Missions can be exported and imported using a versioned JSON format.

Exports include, among other data:

- Mission metadata.
- Operation and priority areas.
- Weather and constraints.
- Available resources.
- Drones and pilots.
- Positions and coverage sectors.
- Mission state.
- Optional custom drone catalog.

See [docs/FORMAT_JSON.md](docs/FORMAT_JSON.md) for the complete format.

---

## Local Development

Requirements: **Node.js 22.18+**, npm, and Git.

```bash
git clone https://github.com/TJeanm/hackathon-gendarmerie-drones.git
cd hackathon-gendarmerie-drones
npm ci
npm run dev
```

Validation:

```bash
npm test
npm run build
```

---

## Prototype Limitations

Division Drone remains a simulation and decision-support prototype.

It does not currently include:

- Real drone control or telemetry.
- A physics engine.
- Terrain elevation in the coverage model.
- Real radio propagation modelling.
- Air traffic control integration.
- Automatic regulatory validation.
- Server-side mission storage.

Coverage areas and percentages are geometric approximations intended to support operational planning.

---

## Documentation

Detailed technical documentation is available in [`docs/`](docs/):

- [Demo Workflow](docs/DEMO.md)
- [Algorithms](docs/ALGORITHMES.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Requirements](docs/REQUIREMENTS.md)
- [JSON Format](docs/FORMAT_JSON.md)
- [Engine API Contracts](docs/API_CONTRACT.md)
- [Validation](docs/VALIDATION.md)
- [Drone Specifications](docs/DRONE_SPECS.md)

---

## Context

This project was developed during a hackathon focused on **multi-drone mission planning and coordination for the French Gendarmerie**.

The goal was to build an explainable decision-support tool capable of turning an operational area and a set of available resources into a proposed deployment, while allowing the plan to adapt when operational conditions change.

**Division Drone — Gendarmerie Hackathon Project**

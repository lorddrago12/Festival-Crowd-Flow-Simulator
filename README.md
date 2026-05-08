# 🎪 Festival Crowd Flow Simulator

A JavaScript simulation of crowd management at a multi-gate festival venue. Models attendee arrivals, gate throughput, and overflow rerouting across timed tick intervals — for both morning and night time blocks.

---

## 📖 Overview

Managing festival crowds requires dynamic routing when gates hit capacity. This simulator models that process: each gate processes a queue of incoming attendees per time tick, and any overflow is automatically rerouted to the next gate in sequence. A throughput summary is printed at the end of each time block.

---

## ✨ Features

- **Multi-gate simulation** — North, East, South, and West gates, each with independent capacity and queue data
- **Tick-based processing** — Attendees arrive in waves; each tick represents one time interval
- **Overflow rerouting** — Excess attendees beyond a gate's capacity are forwarded to the next gate in round-robin order
- **Dual time blocks** — Separate morning and night configurations
- **Throughput reporting** — Per-gate summary of total attendees processed

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) v14 or higher

### Installation

```bash
git clone https://github.com/your-username/festival-crowd-flow-simulator.git
cd festival-crowd-flow-simulator
```

### Run the Simulation

```bash
node simulator.js
```

---

## 📂 Project Structure

```
festival-crowd-flow-simulator/
├── simulator.js   # Core simulation logic and gate data
└── README.md
```

---

## ⚙️ How It Works

### Gate Configuration

Each gate is defined with an `id`, a `capacity` (max attendees processed per tick), and a `queue` (attendees arriving each tick):

```js
const morningGates = [
  { id: "North", capacity: 5, queue: [3, 6, 2, 4] },
  { id: "East",  capacity: 3, queue: [2, 4, 3, 5] },
  { id: "South", capacity: 4, queue: [1, 2, 3, 1] },
  { id: "West",  capacity: 2, queue: [4, 1, 2, 3] },
];
```

### Simulation Loop

```
For each tick:
  └── For each gate:
        ├── Read arriving attendees from queue[tickIndex]
        ├── Process up to `capacity` attendees
        ├── If overflow > 0 → add overflow to next gate's queue at same tick
        └── Accumulate processed count in throughput summary
```

### Overflow Rerouting

Overflow is passed to the **next gate** in circular order (West → North wraps around). This mutates the receiving gate's queue in place for the current tick.

```js
function rerouteOverflow(gates, currentGate, tickIndex, overflowAmount) {
  const nextGateIndex = (gates.indexOf(currentGate) + 1) % gates.length;
  gates[nextGateIndex].queue[tickIndex] += overflowAmount;
}
```

### Output

```
Morning Simulation

Tick 1
Processing North...
3 attendees arriving.

Processing East...
2 attendees arriving.
...

Throughput Summary
North: 14 attendees processed
East: 11 attendees processed
South: 7 attendees processed
West: 8 attendees processed
```

---

## 🔧 Customization

You can easily extend the simulation by modifying the gate data:

| Parameter  | Description                                      |
|------------|--------------------------------------------------|
| `id`       | Gate name/label                                  |
| `capacity` | Max attendees a gate can process per tick        |
| `queue`    | Array of arriving attendees per tick             |

Add more ticks by extending the `queue` arrays, or add more gates by appending to the `morningGates` / `nightGates` arrays.

---

## 🛣️ Potential Improvements

- [ ] Priority-based rerouting (route to the least-loaded gate instead of the next one)
- [ ] Dynamic capacity changes mid-simulation (e.g., staff changes)
- [ ] Visual output / ASCII crowd flow chart
- [ ] CSV export of throughput data
- [ ] Unit tests for core processing functions

---

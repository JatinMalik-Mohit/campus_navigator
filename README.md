# Campus PathFinder — Dijkstra vs A*
### Chandigarh University, Mohali · North + South Campus

An interactive, fully self-contained web application that models the CU Mohali campus as a weighted graph and runs **both** Dijkstra's algorithm and A* Star **simultaneously**, animating the differences live on a canvas map.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🗺️ Fixed Canvas Map | Dark Google-Maps-style campus layout, no pan, zoom only |
| ⚡ Dijkstra | Correct priority-queue implementation, O((V+E) log V) |
| 🌟 A* Star | Admissible Euclidean heuristic in metres, O(E) best case |
| 🎬 Live Animation | Both algos animate simultaneously — visited nodes + path edges |
| 🎨 Dual Colour Coding | Blue = Dijkstra, Orange = A*, Purple = both visited same node |
| 📊 Side-by-Side Results | Distance, nodes visited, walk time, hops for each algo |
| 📝 Step-by-Step Logs | Full node expansion log for both algorithms |
| 🔄 Route History | Last 20 routes saved, click to replay |
| 💡 Insight Text | Auto-generated explanation of why one algo was more efficient |
| 📱 Responsive | Sidebar collapses to full-map mode |

---

## 🏫 Campus Locations (57 nodes)

### North Campus — Academic Blocks
`Block A1` `Block A2` `Block A3`  
`Block B1` `Block B2` `Block B3` `Block B4` `Block B5`  
`Block C1` `Block C2` `Block C3`  
`Block D1` `Block D2` `Block D3` `Block D4` `Block D5` `Block D6`

### North Campus — Facilities
`Central Library` `Auditorium` `Admin Block` `Food Court N`  
`Medical Centre` `ATM / Bank` `Convocation Hall`  
`Sports Complex` `Cricket Ground`

### Boys Hostels
`LC-A` `LC-B` `LC-C` `LC-D` (Le Corbusier block)  
`NC-1` `NC-2` `NC-3` `NC-4` `NC-5` `NC-6` `Zakir Hostel`

### Girls Hostels
`Sukhna-1` `Sukhna-2` `Sukhna-3` `Girls H-1` `Girls H-2`

### South Campus
`SC-E1` `SC-E2` `SC-F1` `SC-F2` `SC-G1`  
`SC Library` `SC Food Court` `SC Boys-1` `SC Boys-2`  
`SC Girls-1` `SC Girls-2` `SC Ground`

### Gates
`Main Gate` `North Gate` `South Gate` `East Gate`

---

## 🚀 Quick Start

### Option A — Standalone (recommended, zero install)
```bash
# Just open index.html directly in any browser
open index.html
# or double-click it in your file manager
```

### Option B — With Flask Backend (REST API)
```bash
pip install flask flask-cors
python app.py
# Then open: http://localhost:5000
```

---

## 🔌 API Endpoints (Flask backend)

| Endpoint | Description |
|---|---|
| `GET /` | Serves the web interface |
| `GET /graph-data` | Returns full campus graph (nodes + edges) |
| `GET /nodes` | Returns node list for dropdowns |
| `GET /shortest-path?src=A1&dst=SC_gnd&algo=dijkstra` | Run Dijkstra |
| `GET /shortest-path?src=A1&dst=SC_gnd&algo=astar` | Run A* |
| `GET /shortest-path?src=A1&dst=SC_gnd&algo=dijkstra&compare=1` | Run both + compare |
| `GET /complexity` | Returns complexity info for both algos |

### Example response
```json
{
  "primary": {
    "algorithm": "dijkstra",
    "path": ["A1", "B1", "C1", "adm", "g_south", "SE1", "SC_gnd"],
    "distance": 820,
    "walk_minutes": 10.3,
    "nodes_visited": 34,
    "hops": 6
  },
  "comparison": { ... A* result ... }
}
```

---

## 🧠 Algorithm Details

### Why Dijkstra and A* always find the same distance
Both are **optimal** algorithms — they guarantee the shortest path. The difference is **efficiency**, not correctness.

### Why A* visits fewer nodes
A* uses a **heuristic function** `h(n)` that estimates the remaining distance from node `n` to the goal using straight-line (Euclidean) distance in metres. This guides the search toward the goal, skipping nodes that are geometrically far away.

```
f(n) = g(n) + h(n)
where:
  g(n) = known cost from source to n (in metres)
  h(n) = sqrt((Δx × 900)² + (Δy × 700)²)  [Euclidean in metres]
```

The heuristic is **admissible** — it never overestimates the true remaining distance — because a straight line is always shorter than or equal to any road path. This guarantees A* still finds the optimal path.

### When to use each
- **Dijkstra** — when you have no geometric information, or the heuristic would be unreliable
- **A*** — when nodes have coordinates and the heuristic is meaningful (maps, grids, spatial graphs)

### Complexity
| | Time | Space |
|---|---|---|
| Dijkstra | O((V + E) log V) | O(V) |
| A* | O(E) best, O((V+E) log V) worst | O(V) |

---

## 📁 Project Structure

```
campus-path-finder/
├── index.html    ← Complete standalone frontend (zero dependencies)
├── app.py        ← Flask REST API backend (Python)
└── README.md     ← This file
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Map | Pure HTML5 Canvas (no Leaflet, no CDN) |
| Algorithms | Implemented in both JS (frontend) and Python (backend) |
| Backend | Python Flask + CORS |
| Fonts | DM Serif Display + DM Sans + DM Mono (Google Fonts) |

---

## 💡 Tips for seeing the biggest algorithm difference

Routes that cross both campuses show the most dramatic difference:
- **North Gate → SC Ground** (diagonal cross-campus)
- **A1 → SC Girls-1** (top-left to bottom-right)
- **Cricket Ground → SC Food Court** (far corner to far corner)
- **LC-A → Convocation Hall** (hostel zone to academic zone)

On these routes A* typically skips 30–50% of the nodes Dijkstra visits.

---

*v5.0 · Zero external dependencies · Canvas renderer · Real CU Mohali building names*

# deckboss-fab

CAD/CAM equipment for deckboss git-agents. Generate parametric cases, mount plates, and robot parts from hardware specs.

## Equipment

### OpenSCAD Case Generator

```python
from deckboss_fab import CADGenerator

cad = CADGenerator("openscad")
scad = cad.generate_jetson_case({
    "board": "orin-nano",
    "mounting": "standoff",
    "ports": ["usb-c", "ethernet", "hdmi", "gpio"],
    "fan": True,
})
# Writes parametric .scad file
```

Generates ready-to-print STL files for:
- **Jetson Orin Nano** carrier cases
- **Jetson Orin NX** carrier cases
- **Raspberry Pi 5** cases
- Custom dimensions, port cutouts, standoff holes, fan vents

### G-Code Generator

```python
from deckboss_fab import GCodeGenerator

gcode = GCodeGenerator("prusaslicer")
gcode.slice("case.stl", {"layer_height": 0.2, "infill": 20})
```

### Simulation Runner

```python
from deckboss_fab import SimulationRunner

sim = SimulationRunner()
tools = sim.list_available()
# {"gazebo": True, "blender": True, "openscad": True, "freecad": False, ...}
```

## Workflow

```
Hardware spec → OpenSCAD source → STL → G-code → 3D print
                                → DXF → CNC cut
                                → URDF → Gazebo simulation
```

Git-agent handles CAD calculations, human approves design, one-click order from marketplace.

## Install

```bash
pip install deckboss-fab
sudo apt install openscad  # CAD engine
sudo apt install freecad   # Optional: advanced CAD
sudo apt install gazebo    # Optional: robot simulation
```

Part of the [Lucineer ecosystem](https://github.com/Lucineer/the-fleet).

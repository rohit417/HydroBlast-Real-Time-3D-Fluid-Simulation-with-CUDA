
# HydroBlast: CUDA-Accelerated 3D Fluid Simulation

This project implements a GPU-accelerated Smoothed Particle Hydrodynamics (SPH) fluid simulation using CUDA. HydroBlast simulates thousands to millions of particles interacting in a 3D space with realistic fluid dynamics, all in real-time using the parallel processing power of modern GPUs.

## Features

- Full 3D fluid simulation using SPH
- Real-time GPU acceleration with CUDA
- Efficient neighbor searching using spatial hashing
- Realistic fluid interactions including:
  - Pressure forces
  - Viscosity
  - Surface tension
  - Boundary handling
- Outputs `.ply` files per frame for visualization
- Python-based visualization scripts (static, animation, trajectories)
- Built-in performance benchmarking

## Requirements

### For Simulation:
- CUDA Toolkit 10.0 or higher
- CMake 3.10 or higher
- C++14 compatible compiler (e.g., GCC 5+, MSVC 2017+)
- NVIDIA GPU with Compute Capability 5.0 or higher

### For Visualization:
- Python 3.6+
- NumPy
- Matplotlib
- PLY file library (`plyfile`)

## Installation

### Building the Simulation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/hydroblast.git
   cd hydroblast
-````

2. Create a build directory and compile:

   ```bash
   mkdir build && cd build
   cmake ..
   make -j4
   ```

### Setting Up Visualization

Install the required Python packages:

```bash
pip install numpy matplotlib plyfile
```

## Usage

### Running the Simulation

Basic usage:

```bash
./hydroblast
```

Advanced options:

```bash
./hydroblast -p 50000 -f 500  # Simulate 50,000 particles for 500 frames
./hydroblast -b               # Run performance benchmark
./hydroblast -h               # Show help
```

Simulation outputs will be saved as `.ply` files (one per frame).

### Visualizing Results

Static visualization:

```bash
python visualize.py --mode static --input frame_100.ply
```

Animation:

```bash
python visualize.py --mode animate --input "frame_*.ply" --output animation.mp4 --fps 30
```

Particle trajectory:

```bash
python visualize.py --mode trajectory --input "frame_*.ply"
```

## How It Works

### Smoothed Particle Hydrodynamics (SPH)

SPH is a particle-based method for simulating fluids. Each particle carries properties like mass and velocity and interacts with neighbors using a smoothing kernel. Key components include:

* **Density Computation**: Summing nearby particle contributions using a kernel function.
* **Pressure Forces**: Move particles from high-density to low-density areas.
* **Viscosity**: Smooths velocity differences between particles.
* **Surface Tension**: Acts at fluid-air boundaries to preserve shape.

### GPU Acceleration with CUDA

* **Neighbor Search**: Accelerated with a spatial hash grid.
* **Parallelization**: Each particle is computed in parallel.
* **Memory Optimization**: Uses shared memory and minimizes CPU-GPU data transfer.
* **Kernel Fusion**: Combines multiple physics steps into fewer CUDA kernel launches.

## Performance

Performance results on an NVIDIA RTX 3080:

| Particles | Frame Time (ms) | FPS   |
| --------- | --------------- | ----- |
| 10,000    | 2.3             | 434.8 |
| 50,000    | 9.7             | 103.1 |
| 100,000   | 19.1            | 52.4  |
| 500,000   | 94.3            | 10.6  |
| 1,000,000 | 187.8           | 5.3   |


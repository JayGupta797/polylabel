# PolyLabel
The *pole of inaccessibility* of a polygon is the point that is farthest from any of its edges making it the most "inaccessible" within the shape. 
This point is often used in geographic and computational geometry applications.

PolyLabel is an iterative grid-based algorithm, which starts by covering the polygon with square cells and iteratively subdividing them in the order of the most promising ones. 
The procedure applies to *any* complex polygon (e.g., with holes) and is guranteed to produce the optimal solution to a given precision. 
You can [read more](https://github.com/mapbox/polylabel) about the original project.

# Directory Structure
The directory structure is detailed below. 
Test cases will be added in the near future.

```bash
├── polylabel
│   ├── polylabel
│   │   ├── __init__.py
│   │   ├── polylabel.py
│   ├── tests
│   │   ├── __init__.py
├── LICENSE
├── README.md
└── .gitignore
```

# Example Usage

```python3
import numpy as np
from polylabel import polylabel

ring1 = np.array([[1, 2], [3, 4], [5, 1], [3, 2], [1, 2]])
ring2 = np.array([[2.5, 3], [3, 3], [3, 2.5], [2.5, 3]])
ring3 = np.array([[2, 2.25], [2.5, 2.25], [2.5, 2.75], [2, 2.5]])
rings = [ring1, ring2, ring3]
polylabel(rings, precision=0.01)
```

The output for is a `Cell` object.
You may directly access `Cell.c` and `Cell.d` to retrieve the center coordinates and radius.

```python3
> cell = polylabel(rings, precision=0.1)
> (cell.c, cell.d)
(array([3.4902, 2.3301]), 0.5145)
```

Upon plotting the circle, we find that it is reasonably accurate:

![desmos-graph](https://github.com/user-attachments/assets/1eedb43b-772d-4457-8b04-f08f646919b0)

# Performance

The average performance over 100 trials for each country provided by `gpd.datasets.get_path('naturalearth_lowres')` is shared below.
Notably, the remix version scales well with as the number of edges increases. 

| Shapefile | Vertices | Remix Avg Time | Original Avg Time |
|-----------|----------|----------------|-------------------|
| Canada    | 794      | 0.032235       | 0.113350          |
| Russia    | 607      | 0.022418       | 0.002272          |
| USA       | 447      | 0.031790       | 0.029915          |
| Indonesia | 250      | 0.021947       | 0.009891          |
| Australia | 241      | 0.011332       | 0.001975          |
| China     | 240      | 0.012288       | 0.001972          |
| Brazil    | 203      | 0.006835       | 0.016461          |
| Mexico    | 170      | 0.002716       | 0.007637          |
| India     | 136      | 0.002474       | 0.005608          |
| Greenland | 132      | 0.006230       | 0.010859          |

# Dependencies
This project uses `NumPy` for numerical computations.

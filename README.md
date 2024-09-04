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

ring1 = np.array([[1, 2], [3, 4], [5, 0], [3, 2], [1, 2]], dtype=float)
ring2 = np.array([[2.5, 3], [3, 3], [3, 2.5], [2.5, 3]], dtype=float)
ring3 = np.array([[2, 2.25], [2.5, 2.25], [2.5, 2.75], [2, 2.5]], dtype=float)
rings = [ring1, ring2, ring3]
polylabel(rings, precision=0.1)
```

The output for qhull.py is a `Cell` object.
You may directly access `Cell.c` and `Facet.d` to retrieve the center coordinates and radius.

```python3
> cell = polylabel(rings, precision=0.1)
> cell
<polylabel.polylabel.Cell object at 0x7fef9032bee0>

> cell.c
[3.4375 2.1875]

> cell.d
0.4192627457812106
```

Upon plotting the circle, we find that it is reasonably accurate:

![desmos-graph](https://github.com/user-attachments/assets/c987e392-600f-4db7-97fe-2e41d5ae21db)

# Performance

The average performance over 100 trials for each country provided by `gpd.datasets.get_path('naturalearth_lowres')` is shared below.
Notably, remix scales well with as the number of edges increases. 

| Country   | Vertices | Remix Avg Time | Original Avg Time |
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

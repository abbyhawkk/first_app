# Site Runoff

## Purpose
Peak storm runoff from a small site with the rational method, and the
size of the storm drain pipe that carries it.

## Inputs
- Areas outlined on a satellite map: roofs, pavement, lawn, and so on
- A surface type for each area, with its runoff coefficient C
- Design storm (2, 10, 25, or 100 year)
- Pipe slope

## Calculations
- Area of each outline
- Weighted C
- Q = CiA, with i from a Provo rainfall table for the design storm
- Full-pipe capacity from Manning's equation, to pick the pipe size

## Outputs
- The areas on the map, colored by surface type
- A list of the areas, the peak flow, and the pipe size

## Look and feel
Clean and professional, with the map filling most of the screen.

## Validation
1.00 acre of asphalt (C = 0.90), 10-year storm (i = 3.0 in/hr), 1% slope:
Q = 2.70 cfs, 12 in pipe.

## Out of scope
Detention ponds and inlet spacing.
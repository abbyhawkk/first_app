# Beam Designer

## Purpose
A beam design tool for engineers and students. The user sets up a beam,
places loads on it, and picks a steel or lumber section. The app shows the
shear, moment, and deflection diagrams and checks whether the beam is
strong and stiff enough, updating instantly as anything changes.

## Users
A civil engineering student checking a homework beam, or an engineer
sizing a floor beam or deck joist.

## Inputs
- Support type: Simply supported or Cantilever (fixed at the left end).
- Span: 4 to 40 ft, set with a slider and a number box that stay in sync.
- Material waste factor: 0% to 15%, default 0%, controlled by a slider
  with 1% increments. Apply it only to displayed material quantity or
  weight estimates using estimate = base quantity x (1 + factor/100).
  It must not change the beam span, section properties, load effects, or
  structural check results.
- Loads, listed in a panel where each can be edited or removed:
  - Point loads: magnitude (kips, downward positive) and position (ft).
    A new point load starts at midspan (at the free end of a cantilever).
  - Uniform loads: magnitude (kips/ft) and start and end positions (ft).
  - "Load Preset" sets the span to 20 ft and the material/section to steel
    W12x26, keeps the current support condition and all existing loads, and
    appends a 1 kip/ft uniform load from 0 to 20 ft.
  - "Add point load" and "Add uniform load" buttons.
  - Point loads can also be dragged along the beam in the drawing,
    snapping to 0.25 ft.
  - Loads that extend past the end of the beam are cut off at the end.
- Material and section, from these tables:

  Steel W-shapes (E = 29,000 ksi, allowable bending stress Fb = 30 ksi):

  | Section | Ix (in^4) | Sx (in^3) | Weight (lb/ft) |
  |---------|-----------|-----------|----------------|
  | W8x10   | 30.8      | 7.81      | 10 |
  | W10x12  | 53.8      | 10.9      | 12 |
  | W12x26  | 204       | 33.4      | 26 |
  | W14x22  | 199       | 29.0      | 22 |
  | W16x26  | 301       | 38.4      | 26 |
  | W18x35  | 510       | 57.6      | 35 |
  | W21x44  | 843       | 81.6      | 44 |
  | W24x55  | 1350      | 114       | 55 |

  Douglas Fir-Larch No. 2 lumber (E = 1,600 ksi, Fb = 0.900 ksi):

  | Section | Ix (in^4) | Sx (in^3) |
  |---------|-----------|-----------|
  | 2x8     | 47.63     | 13.14     |
  | 2x10    | 98.93     | 21.39     |
  | 2x12    | 177.98    | 31.64     |

  Concrete rectangular sections (E = 3,600 ksi, Fb = 1.800 ksi):

  | Section | Ix (in^4) | Sx (in^3) |
  |---------|-----------|-----------|
  | 6x12    | 864       | 144       |
  | 8x12    | 1152      | 192       |
  | 10x12   | 1440      | 240       |

- Deflection limit: L/240 or L/360 (default L/360).
- Self-weight is ignored.

## Calculations
- Variables: L is the span in ft; P is a point-load magnitude in kips;
  p is its position in ft; w is a uniform-load magnitude in kips/ft;
  a and b are uniform-load start and end positions in ft; E is the
  selected section's modulus in ksi; Ix is the section moment of inertia
  in in^4; Sx is the section modulus in in^3; Fb is allowable bending
  stress in ksi; and RA, RB, and MA are support reactions.
- Reactions are found from statics using sum of vertical forces and sum
  of moments: RA and RB are in kips, and the cantilever fixed-end reaction
  MA is in kip-ft.
- Point-load force is P kips. A uniform load contributes force
  w(b-a) kips and moment w(b-a)(a+b)/2 kip-ft about the origin.
- Overlapping uniform loads are averaged by position: at each x, use the
  mean intensity of all uniform loads covering x. Coincident point loads
  are added normally. Loads that extend past the beam are cut off at x=L.
- Positive shear is upward on the left face of the cut; positive moment
  causes compression on the top face.
- Shear V(x) is in kips. Moment M(x) is in kip-ft. Evaluate both at
  1 ft spacing or finer, plus exactly at every point load and uniform-load
  start/end. Report |Vmax| and |Mmax| and the x locations at which they
  occur.
- Deflection y(x) is found by integrating M/EI twice. Convert M to kip-in
  as M_in = M_ft x 12. With M in kip-in, E in ksi (kip/in^2), and
  Ix in in^4, M_in/(EIx) values have units 1/in. Integrate with respect to
  x in inches and apply y=0 at both supports for a simply supported beam,
  or y=0 and slope=0 at the fixed end for a cantilever. Downward deflection
  is negative. Report |ymax| in inches and the x location of |ymax|.
- Bending stress fb = |Mmax| x 12 / Sx, in ksi. Bending passes if fb <= Fb.
- Required section modulus is Sx,required = |Mmax| x 12 / Fb, in in^3.
- The deflection limit in inches is 12L/n, where n=240 or n=360.
  Deflection passes if |ymax| <= 12L/n.
- Lightest passing section: of the sections in the chosen material,
  use the lowest-weight steel section that passes both checks (if two
  weigh the same, use the larger Ix), the first lumber size in table
  order that passes, or the first concrete size in table order that passes.
  If no section passes, show "No passing section in the table" and keep
  the currently selected section unchanged.
- Units: kips, ft, and kip-ft on screen; inches for deflection.
- Units: kips, ft, and kip-ft on screen; inches for deflection.

## Outputs
- A beam drawing with the supports, load arrows (point loads as single
  arrows, uniform loads as a row of small arrows), and the reactions.
- Three diagrams stacked under the beam and lined up with it on the same
  x scale: shear (kips), moment (kip-ft), and deflected shape (in). Each
  diagram is filled and shows its maximum value labeled at its location.
- Moving the mouse over any diagram shows a vertical line across all three
  with the values of V, M, and y at that x.
- Two check cards: Bending and Deflection. Each shows the actual value, the
  allowable value, a bar for the ratio (green under 70%, amber 70-100%,
  red over 100%), and PASS or FAIL.
- A line suggesting the lightest passing section, with a button to use it.
- Everything updates immediately whenever any input changes.

## Look and feel
- Opens with an example already loaded (20 ft simply supported W12x26
  with a 1 kip/ft uniform load over the full span), so it is useful the
  moment it opens.
- Clean and professional, like a commercial engineering web app: light
  background, white cards with soft shadows, one accent color, a modern
  system font, generous spacing. No clutter.
- Inputs in a panel on the left; drawing, diagrams, and checks on the
  right. On a phone, the panels stack.
- Show the "Load Preset" action as a compact, blue-outlined button below the
  design-input introduction.
- Every input has a short label with units. Hovering over a check card
  explains the formula used.

## Validation
1. Simply supported, 20 ft, W12x26, uniform 1 kip/ft over the full span:
   reactions 10 k and 10 k; max shear 10 k; max moment 50.0 kip-ft at
   10 ft; fb = 17.96 ksi; max deflection 0.609 in at 10 ft; L/360 =
   0.667 in; both checks pass.
2. Simply supported, 20 ft, W12x26, point load 10 k at 5 ft: reactions
   7.5 k (left) and 2.5 k (right); max moment 37.5 kip-ft at 5 ft;
   fb = 13.47 ksi; max deflection 0.340 in at 8.82 ft.
3. Cantilever, 10 ft, W12x26, point load 5 k at the free end: reaction
   5 k and fixed-end moment 50 kip-ft; max deflection 0.487 in at the free
   end (PL^3/3EI); L/360 = 0.333 in, so deflection fails.
4. Simply supported 12 ft 2x10 joist, uniform 0.05 kip/ft: max moment
   0.90 kip-ft; fb = 0.505 ksi; max deflection 0.147 in; both pass.

## Out of scope
- Self-weight, shear stress, lateral-torsional buckling, load
  combinations, and continuous beams.
- Saving designs.

## Material & Section Controls
- Include a dropdown selection menu for beam material (e.g., Wood, Steel, Concrete) so the correct material properties are loaded.
- Support both automatic recommended section selection and manual user selection of beam profiles.

## Input Validation & Error Handling
- Validate all numeric inputs (such as span, loads, and dimensions) before executing calculations.
- If any input field is left empty, set to zero, or given a negative number, display a clear red error message on the screen and halt calculations to prevent invalid outputs or crashes.

## Load Behavior Rules
- Uniform loads are always applied in a downward vertical direction.
- Overlapping uniform loads are averaged by position; if no uniform load
  covers a position, the intensity there is zero. Coincident point loads
  are summed normally.
- Removing all loads produces zero reactions, shear, moment, and deflection.

## Output Formatting & Precision
- Format all final computed engineering values (such as maximum moment, deflection, and required section modulus) to exactly two decimal places.
- Append standard engineering units (e.g., psi, kips, inches, kip-ft) to all final result displays.
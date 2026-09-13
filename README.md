# Continuous Inkjet Printer Simulation

MATLAB model that converts an image into the deflection-voltage waveform a continuous inkjet printer would use to place each droplet, then animates the print.

**Stack:** MATLAB (live script)
**Context:** Electromagnetics coursework, with Hanson Nguyen

![Printing the letter A](letter_gifs/DCWaveform%20(A).gif)

## Highlights

- **Solves the inverse problem.** Given a target image, derive the field — then the voltage — needed to deflect each droplet to the right offset.
- **Validates by round-tripping.** Reconstructs the page from the voltage vector alone. If forward and inverse disagree, the output image visibly breaks.
- Same physics as CRT beam deflection: charged particle, uniform field, initial velocity.
- Encodes line breaks as `0 V` entries in a single flat waveform vector.

## Method

1. Threshold the image at 127.5. Dark pixels become droplets.
2. Solve for the field at each droplet's target offset:
   `E = (m·u²·d) / ((L − w/2)·q·w)`
   where `d` = offset, `L` = distance to paper, `w` = plate width, `u` = initial velocity, `m`/`q` = droplet mass and charge.
3. Multiply by plate separation → voltage. Flatten to one vector, `0 V` = carriage return.
4. Run the deflection equation forward from that vector alone. Animate the reconstruction.

## Background

Continuous inkjet printers run a constant ink stream, break it into uniform droplets with a piezo oscillator, charge each droplet, and steer it between deflection plates. Unused droplets divert to a gutter and recycle. Faster and less precise than drop-on-demand, primarily used for packaging and date coding.

## Files

| File | Purpose |
|---|---|
| `Inkjet_Printing.mlx` | MATLAB live script, commented throughout |
| `Inkjet_Printing.txt` | Plain-text export, readable without MATLAB |
| `Inkjet Printing Paper.pdf` | Full derivation and field geometry |
| `letter_gifs/` | Output animations |

## Run it

Open `Inkjet_Printing.mlx` in MATLAB R2022a+. **Change the `imread` path at the top** — it's currently an absolute path from the machine it was written on. Any black-and-white image works.

## Limitations

- **All physical constants are normalized to 1** (mass, charge, velocity, plate geometry). Voltages are proportional, not absolute. Deliberate — the project was about the geometry and the inverse problem, not predicting a real print head's drive voltage. Real values plug straight in.
- Input path is hardcoded.

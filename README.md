<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>CMOS Inverter using Magic VLSI &amp; NgSpice</title>
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; line-height: 1.6; margin: 24px; color: #0b1220; background:#fbfdff; }
    header { margin-bottom: 18px; }
    h1 { font-size: 1.8rem; margin: 0 0 6px 0; }
    p.lead { margin: 0; color: #374151; }
    section { margin-top: 20px; padding: 14px; background: #ffffff; border: 1px solid #e6eef8; border-radius: 10px; box-shadow: 0 6px 18px rgba(20,40,80,0.03); }
    pre { background:#0f1724; color:#e6eef8; padding: 12px; border-radius: 8px; overflow:auto; font-size:0.95rem; }
    code { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", monospace; }
    ul { margin-left: 18px; }
    .meta { font-size:0.92rem; color:#475569; }
    .badge { display:inline-block; background:#eef2ff; color:#3730a3; padding:6px 10px; border-radius:999px; font-weight:600; font-size:0.85rem; margin-right:8px; }
    footer { margin-top:18px; font-size:0.9rem; color:#6b7280; }
    .cols { display:flex; gap:20px; flex-wrap:wrap; }
    .col { flex:1 1 300px; }
  </style>
</head>
<body>
  <header>
    <h1>CMOS Inverter using Magic VLSI &amp; NgSpice</h1>
    <p class="lead">Design, layout, extraction and simulation of a CMOS inverter (SkyWater 130nm example). Includes DRC &amp; LVS workflow, netlist extraction and NgSpice testbench with capacitive load.</p>
  </header>

  <section>
    <div class="cols">
      <div class="col">
        <div class="badge">Project</div>
        <p class="meta">CMOS inverter layout &amp; simulation</p>
      </div>
      <div class="col">
        <div class="badge">Tools</div>
        <p class="meta">Magic VLSI, NgSpice, SkyWater 130nm PDK</p>
      </div>
      <div class="col">
        <div class="badge">Author</div>
        <p class="meta">Satwik Kamath</p>
      </div>
    </div>
  </section>

  <section>
    <h2>Project Overview</h2>
    <p>This repository demonstrates the full flow for a CMOS inverter:</p>
    <ul>
      <li>Create layout using <strong>Magic VLSI</strong>.</li>
      <li>Run <strong>DRC</strong> and <strong>LVS</strong> to verify layout correctness against the schematic.</li>
      <li>Extract the layout netlist and run <strong>NgSpice</strong> simulations.</li>
      <li>Characterize propagation delay (tPLH, tPHL) and average power with a capacitive load.</li>
    </ul>
  </section>

  <section>
    <h2>Repository Structure</h2>
    <pre><code>CMOS-Inverter-Magic-NgSpice/
│── layout/        → Magic layout files (.mag)
│── spice/         → SPICE netlist + testbench
│── results/       → Waveforms & reports (waveforms.png, drc_report.txt, lvs_report.txt)
└── README.html     → This file
</code></pre>
  </section>

  <section>
    <h2>How to Run</h2>

    <h3>1) Open layout in Magic</h3>
    <p>Start Magic with the target technology file (example uses <code>sky130A.tech</code>):</p>
    <pre><code>magic -T sky130A.tech layout/cmos_inverter.mag</code></pre>

    <p>In the Magic prompt you typically run:</p>
    <pre><code>drc check
extract all
ext2spice cmos_inverter.mag -o spice/cmos_inverter.spice
</code></pre>

    <h3>2) Run NgSpice simulation</h3>
    <p>Run the provided testbench which includes the extracted netlist:</p>
    <pre><code>cd spice
ngspice testbench.spice
</code></pre>

    <p>The testbench will:</p>
    <ul>
      <li>Apply a PULSE input (0 → 1.8V)</li>
      <li>Attach a capacitive load (example: 10 fF)</li>
      <li>Run a transient analysis and measure <code>tPLH</code>, <code>tPHL</code> and average power</li>
    </ul>
  </section>

  <section>
    <h2>Example SPICE Files (reference)</h2>
    <h3><code>spice/cmos_inverter.spice</code></h3>
    <pre><code>* CMOS Inverter Netlist
.subckt cmos_inv in out vdd gnd
Mp out in vdd vdd PMOS W=1u L=0.1u
Mn out in gnd gnd NMOS W=0.5u L=0.1u
.ends cmos_inv
</code></pre>

    <h3><code>spice/testbench.spice</code></h3>
    <pre><code>* Testbench for CMOS Inverter
.include cmos_inverter.spice

Vdd vdd 0 1.8
Vin in 0 PULSE(0 1.8 0 50p 50p 1n 2n)
Cl out 0 10f

X1 in out vdd 0 cmos_inv

.tran 0.1n 10n
.measure tran tPLH TRIG v(in) VAL=0.9 RISE=1 TARG v(out) VAL=0.9 RISE=1
.measure tran tPHL TRIG v(in) VAL=0.9 FALL=1 TARG v(out) VAL=0.9 FALL=1
.measure tran power AVG P(Vdd) FROM=0 TO=10n
.control
run
plot v(in) v(out)
.endc
.end
</code></pre>
  </section>

  <section>
    <h2>DRC &amp; LVS</h2>
    <p>Run Design Rule Check and Layout vs Schematic from Magic. Typical commands:</p>
    <pre><code>drc check
lvs -both cmos_inverter.mag cmos_inverter.spice
</code></pre>
    <p>Save the generated reports into <code>results/drc_report.txt</code> and <code>results/lvs_report.txt</code>. Example reports have been added as placeholders.</p>
  </section>

  <section>
    <h2>Results</h2>
    <p>Example waveform and summary outputs are located at:</p>
    <ul>
      <li><code>results/waveforms.png</code> — input &amp; output transient waveforms (dummy/example image included)</li>
      <li><code>results/drc_report.txt</code> — DRC output (place your actual report here)</li>
      <li><code>results/lvs_report.txt</code> — LVS output (place your actual report here)</li>
    </ul>
  </section>

  <section>
    <h2>Notes &amp; Tips</h2>
    <ul>
      <li>If you use the SkyWater PDK, follow the PDK installation and license requirements before running Magic/extraction.</li>
      <li>Tune device widths (W) and lengths (L) in the SPICE netlist to match what you used in the layout for accurate timing and power numbers.</li>
      <li>When extracting, ensure the device finger counts and matching parameters translate correctly to the SPICE model.</li>
      <li>For more accurate power analysis, simulate longer and include input stimulus variations and multiple cycles.</li>
    </ul>
  </section>

  <footer>
    <p>Generated for the <strong>CMOS-Inverter-Magic-NgSpice</strong> project — drop this file into your repository as <code>README.html</code>.</p>
    <p>Author: Satwik Kamath</p>
  </footer>
</body>
</html>

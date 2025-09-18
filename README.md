<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
 
</head>
<body>

  <h1>CMOS Inverter – Layout, Simulation & Results</h1>

  <div class="section">
    <h2>📌 Project Overview</h2>
    <p>
      This project demonstrates the design and simulation of a <b>CMOS Inverter</b> using NGSpice.
      Both schematic-level and layout-level design flows were explored. 
      The inverter was analyzed for propagation delay, power consumption, and switching behavior.
    </p>
  </div>

  <div class="section">
    <h2>⚙️ Tools Used</h2>
    <ul>
      <li><b>Figma</b> → For stick diagram & layout visualization</li>
      <li><b>NGSpice</b> → For circuit simulation (Transient, Power, Delay)</li>
      <li><b>Technology</b> → 180 nm CMOS process (VDD = 1.8 V)</li>
    </ul>
  </div>

  <div class="section">
    <h2>🏗️ Design Flow</h2>
    <ol>
      <li>
        <b>Schematic</b>
        <ul>
          <li>PMOS at top connected to VDD.</li>
          <li>NMOS at bottom connected to GND.</li>
          <li>Gates tied together as input.</li>
          <li>Common drain node as output.</li>
        </ul>
      </li>
      <li>
        <b>Layout (Stick Diagram)</b>
        <ul>
          <li>Diffusion regions (Green).</li>
          <li>Polysilicon gate (Red).</li>
          <li>Metal connections (Blue).</li>
          <li>VDD rail (Dark Red), GND rail (Black).</li>
          <li>Labels: IN (red), OUT (blue).</li>
        </ul>
      </li>
      <li>
        <b>Netlist Generation & Simulation</b>
        <ul>
          <li>Netlist extracted and run in NGSpice.</li>
          <li>Input: Pulse source (0 → 1.8 V).</li>
          <li>Output: Measured across drain node.</li>
        </ul>
      </li>
    </ol>
  </div>

  <div class="section">
    <h2>📊 Simulation Results</h2>
    <h3>1. Waveforms</h3>
    <p>Input: Square pulse (0–1.8 V).<br>Output: Inverted pulse.</p>
   
    
    <h3>2. Key Measurements</h3>
    <ul>
      <li><b>Propagation Delay (tpHL):</b> ~51 ps</li>
      <li><b>Propagation Delay (tpLH):</b> ~55 ps</li>
      <li><b>Average Power Consumption:</b> ~1.8 µW</li>
    </ul>
  </div>

  <div class="section">
    <h2>📖 Analysis</h2>
    <ul>
      <li>CMOS inverter <b>inverts the input logic level</b>.</li>
      <li>Low static power → Power only consumed during switching.</li>
      <li>Symmetric tpHL & tpLH → Indicates <b>balanced transistor sizing</b>.</li>
      <li>Clean switching transition → High noise margins and reliable operation.</li>
    </ul>
  </div>

  

</body>
</html>

# mmWave Radar for Live Animal Detection in Passenger Screening

## problem statement
Government is seeking advanced technology solutions to enhance the efficiency of passenger screening at clearance halls. The primary goal is to assist frontline officers in identifying and intercepting cases of illegal animal importation concealed within passengers' baggage, backpacks, or luggage.

The solution should include a system capable of quickly and accurately detecting live animals, with a focus on warm-blooded animals. Solutions that can also detect cold-blooded animals are highly preferred.

### why mmWave radar
Millimeter-wave radar uses electromagnetic waves to penetrate non-metallic materials (like clothing, backpacks, and plastic containers). Advanced Multi-Input Multi-Output (MIMO) radar systems can create high-resolution 3D images of the contents.

The key advantage is **3D Doppler Imaging**. Even if an animal is perfectly still, the radar can detect micro-movements such as breathing, heartbeats, or slight twitches. This makes it highly effective for detecting both warm-blooded and cold-blooded live animals concealed in complex environments.

| Technology | Detects Life? | Penetrates Bags? | 3D Imaging? | Ionizing Radiation? | Throughput |
|---|---|---|---|---|---|
| **X-ray** | No | Yes | No (2D only) | Yes | High |
| **Thermal IR** | Indirectly (body heat) | No (blocked by fabric) | No | No | Medium |
| **mmWave Radar** | **Yes (Doppler)** | **Yes** | **Yes (MIMO)** | **No** | **High** |
| **Manual Inspection** | Yes | Yes (opening bags) | N/A | No | Low |

---

## radar sensor choice

### [TI IWR6843ISK — 60 GHz mmWave Radar Sensor](https://www.ti.com/tool/IWR6843ISK)
![](./images/IWR6843ISK.png)

| Feature | Specification |
|---|---|
| Frequency Band | 60–64 GHz |
| TX/RX Channels | 3 Transmitters, 4 Receivers (12 virtual antennas) |
| Bandwidth | Up to 4 GHz (range resolution ~3.75 cm) |
| Detection Range | Up to 50 m (people), ~0.3–2 m for vital sign / micro-motion |
| Range Resolution | ~3.75 cm (with 4 GHz bandwidth) |
| Velocity Resolution | ~0.1 mm/s (Doppler micro-motion) |
| Field of View (FOV) | ±60° Azimuth, ±60° Elevation (antenna dependent) |
| Output Interface | USB (via TI DCA1000EVM data capture card) or direct SPI |
| SDK / Processing | TI mmWave SDK, mmWave Studio, MATLAB/Python examples |
| Vital Sign Detection | Built-in demo: breathing rate, heartbeat rate detection |
| 3D Imaging Capability | Yes — MIMO virtual array, support for 3D point cloud + Doppler |
| Power Supply | 5V DC (via USB or barrel jack) |
| Dimension | 63.5 mm × 44.5 mm (standalone PCB) |
| Price | ~$299 USD (ISK only), ~$799 USD (with DCA1000EVM capture card) |

- **Key strength**: Most mature mmWave platform for vital sign and people-counting demos. TI provides ready-to-run lab demos for breathing/heartbeat detection.
- **DCA1000EVM required** for raw ADC data capture and custom 3D/Doppler processing.
- MIMO virtual array (12 elements) provides moderate 3D angular resolution — suitable for bag-scale imaging at close range.

### [TI IWR1843BOOST — 77 GHz Industrial mmWave Radar Sensor](https://www.ti.com/tool/IWR1843BOOST)
![](./images/IWR1843BOOST.png)

| Feature | Specification |
|---|---|
| Frequency Band | 76–81 GHz |
| TX/RX Channels | 3 Transmitters, 4 Receivers (12 virtual antennas) |
| Bandwidth | Up to 4 GHz (range resolution ~3.75 cm) |
| Detection Range | Up to 100 m (vehicles/objects), ~0.3–2 m for micro-motion detection |
| Range Resolution | ~3.75 cm |
| Velocity Resolution | Sub-mm/s Doppler sensitivity |
| Field of View (FOV) | ±60° Azimuth, ±60° Elevation |
| Output Interface | USB (via DCA1000EVM), CAN, SPI |
| SDK / Processing | TI mmWave SDK, mmWave Studio, MATLAB/Python |
| On-Chip DSP | Yes — TI C674x DSP for real-time FFT & CFAR processing |
| On-Chip MCU | Yes — ARM Cortex-R4F for tracking & classification |
| 3D Imaging Capability | Yes — MIMO virtual array, 3D point cloud generation |
| Power Supply | 5V DC |
| Dimension | 79 mm × 63.5 mm (BOOST board) |
| Price | ~$349 USD (BOOST only), ~$849 USD (with DCA1000EVM) |

- Higher frequency (77 GHz) provides slightly better range resolution and smaller antenna form factor vs. 60 GHz.
- On-chip DSP + MCU enables real-time processing without external host — suitable for embedded deployment at checkpoint.
- Mature automotive/industrial ecosystem; same MIMO and Doppler capabilities as IWR6843.

### [TI IWRL6432BOOST — 57–64 GHz Low-Power mmWave Radar Sensor](https://www.ti.com/tool/IWRL6432BOOST)
![](./images/IWRL6432BOOST.png)

| Feature | Specification |
|---|---|
| Frequency Band | 57–64 GHz |
| TX/RX Channels | 2 Transmitters, 3 Receivers (6 virtual antennas) |
| Bandwidth | Up to 4 GHz |
| Detection Range | Up to 20 m, ~0.3–2 m for micro-motion |
| Range Resolution | ~3.75 cm |
| Field of View (FOV) | ±60° Azimuth, ±50° Elevation |
| Output Interface | USB (via DCA1000EVM), UART, SPI |
| SDK / Processing | TI mmWave SDK, Radar Toolbox |
| Power Consumption | ~2.5 mW (deep sleep), ~1.5W (active) |
| Dimension | 43 mm × 25 mm |
| Price | ~$199 USD (BOOST only) |

- Newer generation — lower power, smaller form factor. Fewer virtual channels (6 vs. 12) means reduced 3D angular resolution, but still viable for bag-scale imaging.
- Better suited for battery-powered or space-constrained deployments.
- **Caveat**: 2TX/3RX limits MIMO virtual array size — may not match 3D imaging quality of IWR6843/IWR1843.

### [Infineon DEMO BGT60TR13C — 60 GHz Radar Sensor](https://www.infineon.com/cms/en/product/evaluation-boards/demo-bgt60tr13c/)
![](./images/BGT60TR13C.png)

| Feature | Specification |
|---|---|
| Frequency Band | 57–63 GHz (V-band) |
| TX/RX Channels | 1 Transmitter, 3 Receivers (3 virtual channels) |
| Bandwidth | Up to 5.5 GHz (~2.7 cm range resolution) |
| Detection Range | Up to 15 m, ~0.2–1 m optimized for vital sign |
| Field of View (FOV) | ±45° Azimuth, ±45° Elevation (antenna dependent) |
| Output Interface | USB (via Radar Baseboard MCU7), SPI |
| SDK / Processing | Infineon Radar Development Kit (RDK), Radar Fusion GUI, Python SDK |
| Vital Sign Detection | Yes — dedicated vital sensing reference application |
| On-Chip ADC | 12-bit, integrated FIFO |
| Dimension | 25 mm × 25 mm (sensor only), 65 mm × 55 mm (demo board) |
| Price | ~€250 EUR (DEMO BGT60TR13C board) |

- Compact sensor footprint (25mm × 25mm) — ideal for building custom MIMO arrays by cascading multiple units.
- Higher bandwidth (5.5 GHz) gives finer range resolution (~2.7 cm) than TI alternatives.
- **Limitation**: Single TX channel limits MIMO imaging unless multiple boards are synchronized. Best suited for single-point vital sign detection rather than full 3D imaging.

### [Vayyar — XRR / Walabot Imaging Radar Modules](https://www.vayyar.com/)
![](./images/vayyar.png)

| Feature | Specification |
|---|---|
| Frequency Band | 3–81 GHz (product dependent); 60 GHz & 79 GHz for high-res imaging |
| TX/RX Channels | Up to 48 transceivers (RFIC-based, MIMO array on-chip) |
| Range Resolution | ~1–2 cm (ultra-wideband capable) |
| Detection Range | 0–10 m (indoor imaging), 0–2 m for through-material screening |
| Field of View | Up to 160° (wide-angle lensless antenna array) |
| Output Interface | USB, Ethernet, Wi-Fi (product dependent) |
| SDK / Processing | Vayyar SDK, 3D imaging engine, MATLAB/Python API |
| Vital Sign Detection | Yes — demonstrated in healthcare and automotive child-presence detection |
| 3D Imaging Capability | **Yes — core capability**. High-channel-count MIMO provides dense 3D voxel maps. |
| On-Chip DSP | Yes |
| Price | NDA / Contact vendor (varies by module & volume) |

- **Key differentiator**: Highest channel count (up to 48 TRX) among commercial options — produces the most detailed 3D images and Doppler maps.
- Already proven in through-material imaging applications (wall scanning, automotive occupant detection).
- Full-stack solution (hardware + imaging engine + API) reduces integration effort vs. building from raw radar data.
- Price is higher and requires NDA — more suited to funded development programs than initial benchtop prototyping.

---

## radar comparison summary

| Sensor | Frequency | TX/RX | Max Bandwidth | 3D Imaging | Vital Sign Demo | Price (USD) |
|---|---|---|---|---|---|---|
| **TI IWR6843ISK** | 60 GHz | 3/4 (12 virt.) | 4 GHz | Yes (moderate) | Yes | ~$299–$799 |
| **TI IWR1843BOOST** | 77 GHz | 3/4 (12 virt.) | 4 GHz | Yes (moderate) | Via custom DSP | ~$349–$849 |
| **TI IWRL6432BOOST** | 60 GHz | 2/3 (6 virt.) | 4 GHz | Limited | Via custom DSP | ~$199–$699 |
| **Infineon BGT60TR13C** | 60 GHz | 1/3 (3 virt.) | 5.5 GHz | No (single TX) | Yes | ~€250 |
| **Vayyar Module** | 60/79 GHz | Up to 48 | Wideband | **Yes (best-in-class)** | Yes | NDA req. |

---

## detection mechanism

### warm-blooded animals (birds, mammals)
- Strong, rhythmic Doppler signatures from breathing (0.1–1 mm chest wall displacement, 0.1–3 Hz) and heartbeat (~0.1–0.5 mm, 1–3 Hz).
- Body temperature (~37°C) vs. ambient creates dielectric contrast — enhances radar reflectivity at tissue boundaries.
- **Detection confidence: High**. TI's own vital-sign demos prove human breathing/heartbeat can be extracted reliably at 1–2 m range through clothing.

### cold-blooded animals (reptiles, amphibians)
- Weaker but present respiratory micro-motions (periodic throat/body wall movements). Some reptiles exhibit lung ventilation at 0.05–0.5 Hz.
- Lower body temperature (ambient-matched) reduces dielectric contrast but does **not** eliminate it — tissue structures (lungs, organs, skin boundaries) still reflect.
- Longer integration time (5–15 seconds) increases SNR and detection probability.
- **Detection confidence: Medium–High**. Dependent on animal size, activity level, and concealment material.

### key advantage over existing technologies
Unlike thermal cameras (which require the animal's body heat to reach the sensor with an unobstructed view) or X-ray (which sees only density), **mmWave Doppler detection relies on physical motion** — a physiological inevitability for all living vertebrates. Even at rest, breathing and heartbeat produce detectable phase modulation in the radar return.

---

## system architecture concept

```
 ┌─────────────────────────────────────────────────────────┐
 │                   CLEARANCE CHECKPOINT                    │
 │                                                           │
 │  ┌──────────┐      ┌─────────────────┐    ┌───────────┐  │
 │  │ Bag      │      │ MIMO mmWave     │    │ Operator  │  │
 │  │ Conveyor │─────▶│ Radar Array     │───▶│ Display   │  │
 │  └──────────┘      │ (e.g. cascaded  │    └───────────┘  │
 │                    │  IWR6843 units) │                    │
 │                    └────────┬────────┘                    │
 │                             │                             │
 │                             ▼                             │
 │                    ┌─────────────────┐                    │
 │                    │ Real-Time       │                    │
 │                    │ Processor       │                    │
 │                    │ • Static 3D     │                    │
 │                    │   reconstruction│                    │
 │                    │ • Doppler       │                    │
 │                    │   micro-motion  │                    │
 │                    │ • AI/ML classifier                   │
 │                    └─────────────────┘                    │
 └─────────────────────────────────────────────────────────┘
```

### signal processing chain

1. **Static 3D Reconstruction** — FMCW range processing + MIMO angle estimation (via virtual array beamforming or MUSIC algorithm) → volumetric voxel map of bag contents.
2. **Doppler Micro-Motion Extraction** — Coherent phase analysis across slow-time frames to isolate sub-millimeter displacements at physiological frequencies (0.1–3 Hz breathing band, 1–3 Hz cardiac band).
3. **AI/ML Fusion** — Deep classifier trained on labeled radar signatures (live animal vs. inert clutter vs. empty bag). Fuses static shape features + dynamic Doppler features for joint decision.
4. **Alert Output** — Green (clear) / Amber (ambiguous, recommend secondary) / Red (live animal detected).

---

## technical risks & mitigations

| Risk | Mitigation |
|---|---|
| **Low SNR for cold-blooded animals** | Longer integration windows (5–15 s), higher-gain antenna arrays, matched filtering for known respiratory waveforms. |
| **Metallic baggage lining** | mmWave is fully reflected by metal — detection impossible through foil-lined bags. Use secondary X-ray check + flag metallic contents. |
| **Conveyor motion clutter** | Stationary radar geometry + Doppler clutter filtering removes constant-velocity belt motion; only internal bag motions remain. |
| **False positives (vibrating electronics, loose items)** | Distinguish physiological periodic motion (broadband, variable frequency) from mechanical single-frequency vibration via spectral pattern analysis and ML classifier. |
| **Throughput constraints** | Fast FMCW chirp rates + parallel beamforming achieve < 5 s per bag dwell time; sufficient for checkpoint flow rates. |

---

## feasibility

### supporting research
- **Remote vital sign monitoring**: Multiple peer-reviewed studies demonstrate mmWave radar detection of human heartbeat and respiration through clothing and blankets at 1–3 m (FMCW radar at 60–77 GHz). Same principles apply to animals. TI provides a production-ready vital signs demo with IWR6843.
- **Security screening**: MIMO mmWave body scanners for concealed object detection are already deployed at airports (e.g., Rohde & Schwarz QPS systems at 70–80 GHz). Bag-screening adaptation is a straightforward extension.
- **Search-and-rescue life detection**: Lower-frequency Doppler radar is field-proven for detecting breathing survivors under rubble. mmWave brings higher spatial resolution appropriate for baggage-scale screening.

### suggested path
1. **Benchtop validation** using TI IWR6843ISK (lowest barrier to entry, built-in vital sign demo, $299) — test detection of animal respiration through fabric, plastic, and cardboard at 0.5–2 m.
2. **Custom MIMO array** if higher 3D resolution is needed — cascade multiple IWR6843 units or engage Vayyar.
3. **ML dataset collection** — labeled recordings of live animals vs. inert objects in various concealment scenarios.
4. **Field trial** at controlled checkpoint, measure Pd/Pfa, iterate.

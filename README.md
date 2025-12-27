# Multi-simulator Drone Suite - Hakoniwa

This workspace contains the Hakoniwa family of drone simulation projects and integrations. It brings together the Hakoniwa simulation core, PX4 and ArduPilot integrations, Unity/Unreal visualizations, tools, and examples to provide a flexible environment for research, education, demos and development of drone systems.

Key highlights:
- Integrated simulator core and visualization pipelines (Unity / Unreal)
- Support for PX4 and ArduPilot flight stacks and Python control APIs
- Multiple working modes (single library, shared-memory hub, containerized distributed)
- Cross-platform: Windows, Ubuntu (Linux), macOS (Apple Silicon M1/M2/M3)

Credits & upstream authors
- hakoniwa-drone-core: Hakoniwa Lab / hakoniwalab (core simulation framework and Python APIs). See `hakoniwa-drone-core/README.md` for details.
- PX4 autopilot integration: PX4 contributors — upstream: https://github.com/PX4/PX4-Autopilot
- ArduPilot integration: ArduPilot community — upstream: https://github.com/ArduPilot/ardupilot
- hakoniwa-px4sim: toppers / hakoniwa-px4sim (original PX4 simulation integration)
- Unity / Unreal adapters and models: hakoniwalab (see corresponding subfolders for licenses and authors)

Each sub-repository keeps its own LICENSE and AUTHORS/README files — please consult them for full copyright and contribution information.

Supported Operating Systems
- Windows 11 (and WSL2 for Linux tooling)
- Ubuntu 22.04 / 24.04 (and compatible Linux distributions)
- macOS on Apple Silicon (M1 / M2 / M3)

Three working modes (defined by `hakoniwa-drone-core`)
1. Single pattern ("箱庭なし") — minimal mode: the simulator engine is used as a library (C/C++), callable from Unity/Unreal or Python scripts. Good for embedding the engine directly.
2. Shared-memory pattern (MMAP, "箱庭あり - 共有メモリ") — the Hakoniwa core acts as a hub; components (visualizers, controllers, sensors) connect via shared memory for low-latency collaboration.
3. Container pattern ("箱庭あり - コンテナ") — components run in Docker containers and connect via bridge/networking; useful for distributed demos, teaching, and cloud experiments.

Workspace structure and brief usage
- `ardupilot/` — ArduPilot source tree included for SITL integration and customized builds. Use its README and build tooling when running ArduPilot-based simulations.
- `PX4-Autopilot/` — PX4 flight stack sources (upstream PX4). Use it for PX4 SITL and firmware testing with the hakoniwa px4 adapters.
- `hakoniwa-drone-core/` — Core simulation components, Python APIs, and documentation. Primary entrypoint for building or running simulator modes. See `hakoniwa-drone-core/README.md` for setup and the three working modes.
- `hakoniwa-px4sim/` — PX4-specific simulation glue and examples. Useful when running PX4-based scenarios.
- `hakoniwa-unity-drone/` — Unity visualization and demo scenes. Open the Unity project for visual debugging and interactive demos.
- `hakoniwa-unity-drone-model/` — Unity-ready drone models and helper scripts.
- `hakoniwa-unreal-drone/` — Unreal Engine project with visualization and integration points.
- `hakoniwa-drone-core/docs/`, `docs/` and `replay/` — in-repo documentation, tutorials and log replay tools. Read these before running complex setups.

Quick start pointers
1. Pick a mode in `hakoniwa-drone-core/README.md` (single / shared-memory / container).
2. Follow the per-repo README for build instructions: build the core binaries or obtain releases (see `hakoniwa-drone-core/README.md` -> Releases). 
3. For PX4 or ArduPilot scenarios, follow the relevant subfolder guides (`PX4-Autopilot/`, `ardupilot/`) to prepare SITL binaries and connect them to the Hakoniwa services.
4. For visualization open the Unity or Unreal projects in `hakoniwa-unity-drone/` or `hakoniwa-unreal-drone/` respectively.

Where to find more information
- Core (modes, APIs, examples): hakoniwa-drone-core/README.md
- PX4 integration: PX4-Autopilot/ and hakoniwa-px4sim/ directories
- ArduPilot integration: ardupilot/ directory
- Unity/Unreal visualizations: hakoniwa-unity-drone/ and hakoniwa-unreal-drone/

Contact / licensing
This workspace includes components with different licenses. The Hakoniwa core is distributed under a custom non-commercial license (see `hakoniwa-drone-core/LICENSE.md`). Upstream projects (PX4, ArduPilot, etc.) use their own licenses — see each component's LICENSE file for details.


Ready-compiled libraries for testing
- Releases (prebuilt binaries): https://github.com/toppers/hakoniwa-drone-core/releases

	Typical release ZIPs contain platform-specific binaries for quick testing:
	- mac.zip — macOS (Apple Silicon M1/M2/M3) binaries (e.g., mac-aircraft_service_px4, mac-drone_service_rc, hako_service_c)
	- lnx.zip — Linux (Ubuntu) binaries
	- win.zip — Windows binaries

	After downloading and extracting the appropriate ZIP, run the example binaries from the extracted folder. Avoid paths with spaces or non-ASCII characters.

Main examples to run (brief)
1) Single pattern (箱庭なし) — minimal, library or local binary usage
	 - Description: Run the simulator engine as a standalone binary or call the C library directly from Unity/Unreal/Python.
	 - Example (PX4 adapter):
		 ```bash
		 <os>-aircraft_service_px4 <PX4_HOST_IP> 4560 ./config/drone/px4
		 ```
	 - Example (CUI test):
		 ```bash
		 drone_service_rc 1 config/drone/rc
		 ```
	 - Docs: `hakoniwa-drone-core/docs/getting_started/single.md`

2) Shared-memory (MMAP) pattern (箱庭あり - 共有メモリ)
	 - Description: Low-latency hub mode where the Hakoniwa core provides shared-memory (MMAP) PDU for visualizers and controllers (Unity/Unreal clients connect to the core).
	 - Example (PX4 sample app):
		 ```bash
		 <os>-main_hako_aircraft_service_px4 <HOST_IP> 4560 ./config/drone/px4 <path/to/hakoniwa-unity-drone>/simulation/avatar-drone.json
		 ```
	 - Docs: `hakoniwa-drone-core/docs/getting_started/mmap.md`

3) Container pattern (箱庭あり - コンテナ)
	 - Description: Distributed containers for each component (PX4/Ardupilot, hakoniwa services, bridges). Use Docker compose or provided scripts to start the networked system.
	 - Example (high-level):
		 ```bash
		 # from hakoniwa top-level (example)
		 bash hakoniwa-drone-core/docker/run.bash
		 ```
	 - Docs: `hakoniwa-drone-core/docs/getting_started/container.md`

Notes
- Replace `<os>` with `mac`, `linux` or `win` depending on the binary you downloaded.
- For PX4/Ardupilot scenarios prepare the corresponding SITL binary or run instructions from `PX4-Autopilot/` or `ardupilot/`.
- See `hakoniwa-drone-core/README.md` and the getting-started docs for detailed, platform-specific steps and prerequisites.


# FT-FRFP--
FT-DFRP: Ω The Fractal Toroidal Density Field Routing Protocol Storage Extension

📡 FT-DFRP Extension Roadmap

Storage · Monetization · Compute · Pricing

⸻

🧱 Phase I — Immutable Sharded Storage (Weeks 1–2)

🔹 Goals
	•	Build a flat-file, append-only shard store
	•	Enable multi-drive linear expansion
	•	Support pause/migrate workflows (e.g., via dd)
	•	Integrate Merkle verification and ANN-aware I/O filters

✅ Key Features
	•	Config via .env or .toml: DRIVE_0, QUOTA_0, DRIVE_1, …
	•	Per-shard Merkle tree + UUIDv7 shard ID
	•	File structure: [MERKLE_ROOT | METADATA | PAYLOAD]
	•	Shard index for read + verify with optional FUSE
	•	cli_storage:
	•	init-storage, write-shard, verify-shard, pause-migrate, list-volumes

⸻

🪙 Phase II — Closed Economic Loop (Weeks 3–4)

🔹 Goals
	•	Enable wallet-based usage accounting
	•	Track and verify node contributions (CPU, GPU, storage)
	•	Broadcast usage fees and payouts
	•	Introduce protocol margin for owners (e.g., you)

✅ Key Features
	•	usage_counter.c: counts bytes written/read + cycles used
	•	wallet.c: includes address, BLS key, and ANN vector
	•	iofilter: logs shard or job fulfilled → updates usage counters
	•	merkle_reward: reward logged only if Merkle proof succeeds
	•	pricing_oracle.c: local ANN-aware resource pricing per node:
	•	CPU → hash/s
	•	GPU → TFLOP/s
	•	Storage → IOPS + MB/s
	•	fee_match: jobs and shard requests include max_fee vector

⸻

🧠 Phase III — Vector-Native Monetization Engine (Week 5)

🔹 Goals
	•	Self-adjusting local fee markets
	•	Real-time broadcast fee propagation
	•	Identity + demand-based pricing tied to ANN topology

✅ Key Features
	•	Nodes rebroadcast price vectors
	•	ANN filters prioritize nodes by cost/performance profile
	•	Transaction context includes:

{
  "cpu_sec": 1.2,
  "storage_mb": 300,
  "wallet": "...",
  "max_fee": 0.0042
}


	•	Pricing decays with inactivity; rises with demand saturation
	•	Sigmoid-based reward weighting to prevent spam

⸻

⚙️ Phase IV — OpenMPI-Aware Distributed Compute (Weeks 6–7)

🔹 Goals
	•	Execute ANN-routed compute tasks across mesh
	•	Route jobs by semantic vector match
	•	Validate outputs with Merkle-proofed result set
	•	Compensate contributing nodes automatically

✅ Key Features
	•	compute_context: carries vector, CPU/GPU time, fee, BLS sig
	•	compute_listener: match incoming jobs by ANN proximity
	•	mpi_wrapper: OpenMPI/FFI link for real job execution
	•	cli_compute:
	•	submit-job, listen-job, verify-job, settle-job
	•	Reward from fee pool only paid if:
	•	Output is Merkle-valid
	•	Vector match threshold is met

⸻

🌐 Phase V — Mesh Coordination & Elastic Scaling (Week 8)

🔹 Goals
	•	Harmonize vector spaces (data + identity)
	•	Enable elastic participation in storage or compute
	•	Ensure fault tolerance + migration safety

✅ Key Features
	•	Node joins with vector identity + resource profile
	•	If drive is added, node performs:
	•	pause-storage, migrate, rebuild-inode, resume
	•	Compute nodes report capability, ANN-mapped role
	•	ANN-recorded “trust vector” for each node used for weighting

⸻

📦 Deliverables

Module	Description
storage_core.c	Immutable, Merkle-verified, shardable file I/O
wallet.c	Wallet address + BLS signature management
usage_counter.c	Tracks resources used & fulfilled jobs/shards
pricing_oracle.c	Local price prediction via ANN + feedback
fee_match_engine.c	Fee satisfaction check for job/shard routing
compute_engine.c	Distributed OpenMPI task router
cli_storage.c	Storage ops CLI
cli_compute.c	Compute ops CLI
economic.toml	Adjustable policy: margin %, decay rate, etc

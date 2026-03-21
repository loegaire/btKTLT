# BTKTLT Homework Submission (League of Robot Runners)

This repository contains my homework implementation for League of Robot Runners using the Warehouse map.

## Submission artifacts

- Demo video: [robot-recording.mp4](robot-recording.mp4)
- Modified planner files:
	- `Start-Kit/inc/MAPFPlanner.h`
	- `Start-Kit/src/MAPFPlanner.cpp`
- Generated outputs:
	- `results/team_nms/startkit_wait/warehouse_large_10.json`
	- `results/team_nms/startkit_wait/warehouse_large_20.json`
	- `results/team_nms/startkit_wait/warehouse_large_40.json`
	- `results/team_nms/startkit_wait/warehouse_large_80.json`
	- `results/team_nms/startkit_wait/warehouse_large_100.json`

## Homework requirements addressed

1. Run Team No Man's Sky solution on Warehouse with robot counts: 10, 20, 40, 80, 100.
2. Modify Start-Kit so each robot, after reaching goal location `X`, waits for `(X % 100)`.
3. Visualize output in PlanViz.

## What was implemented

The Start-Kit planner now maintains a per-agent wait counter:

- On real task completion events (`env->new_freeagents`), it sets wait ticks to `current_location % 100`.
- During planning, it overrides planned actions to `Action::W` while the counter is positive.
- It skips the artificial initialization free-agent event at timestep `0`.

## Prerequisites

- Linux/macOS (or Windows with equivalent tooling)
- CMake + C++ compiler
- Python 3.10+

## Repository layout (important)

All commands below assume you are at repository root:

```bash
cd btktlt
```

## Build modified Start-Kit

```bash
cd Start-Kit
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j2
cd ..
```

## Generate Warehouse cases (10/20/40/80/100)

```bash
cd Start-Kit
python3 utils/generate_warehouse_cases.py
cd ..
```

This generates:

- `Start-Kit/example_problems/warehouse.domain/warehouse_large_{10,20,40,80,100}.json`
- `Start-Kit/example_problems/warehouse.domain/agents/warehouse_large_{10,20,40,80,100}.agents`

## Run modified Start-Kit (one case at a time)

Create output folders first:

```bash
mkdir -p logs results/team_nms/startkit_wait
```

Run each case individually (recommended to avoid system freeze):

### Case 10

```bash
cd Start-Kit
/usr/bin/time -o ../logs/time_startkit_10.txt -f '%E,%x' \
	./build/lifelong \
	--inputFile ./example_problems/warehouse.domain/warehouse_large_10.json \
	-o ../results/team_nms/startkit_wait/warehouse_large_10.json \
	> ../logs/startkit_wait_10.log 2>&1
cd ..
```

### Case 20

```bash
cd Start-Kit
/usr/bin/time -o ../logs/time_startkit_20.txt -f '%E,%x' \
	./build/lifelong \
	--inputFile ./example_problems/warehouse.domain/warehouse_large_20.json \
	-o ../results/team_nms/startkit_wait/warehouse_large_20.json \
	> ../logs/startkit_wait_20.log 2>&1
cd ..
```

### Case 40

```bash
cd Start-Kit
/usr/bin/time -o ../logs/time_startkit_40.txt -f '%E,%x' \
	./build/lifelong \
	--inputFile ./example_problems/warehouse.domain/warehouse_large_40.json \
	-o ../results/team_nms/startkit_wait/warehouse_large_40.json \
	> ../logs/startkit_wait_40.log 2>&1
cd ..
```

### Case 80

```bash
cd Start-Kit
/usr/bin/time -o ../logs/time_startkit_80.txt -f '%E,%x' \
	./build/lifelong \
	--inputFile ./example_problems/warehouse.domain/warehouse_large_80.json \
	-o ../results/team_nms/startkit_wait/warehouse_large_80.json \
	> ../logs/startkit_wait_80.log 2>&1
cd ..
```

### Case 100

```bash
cd Start-Kit
/usr/bin/time -o ../logs/time_startkit_100.txt -f '%E,%x' \
	./build/lifelong \
	--inputFile ./example_problems/warehouse.domain/warehouse_large_100.json \
	-o ../results/team_nms/startkit_wait/warehouse_large_100.json \
	> ../logs/startkit_wait_100.log 2>&1
cd ..
```

## Correctness verification

Run:

```bash
python3 -c 'import json; fs=["results/team_nms/startkit_wait/warehouse_large_10.json","results/team_nms/startkit_wait/warehouse_large_20.json","results/team_nms/startkit_wait/warehouse_large_40.json","results/team_nms/startkit_wait/warehouse_large_80.json","results/team_nms/startkit_wait/warehouse_large_100.json"]; print("file,teamSize,numTaskFinished,makespan,numPlannerErrors,numScheduleErrors,numEntryTimeouts"); [print(f.split("/")[-1],d.get("teamSize"),d.get("numTaskFinished"),d.get("makespan"),d.get("numPlannerErrors"),d.get("numScheduleErrors"),d.get("numEntryTimeouts"),sep=",") for f in fs for d in [json.load(open(f))]]'
```

Expected:

- All files parse correctly.
- `teamSize` matches file name.
- `numPlannerErrors = 0`, `numScheduleErrors = 0`, `numEntryTimeouts = 0`.

## PlanViz: how to open and submit

### Install PlanViz dependencies

```bash
cd PlanViz
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cd ..
```

### Visualize case 10

```bash
cd PlanViz
python3 script/run.py \
	--map ../Start-Kit/example_problems/warehouse.domain/maps/warehouse_large.map \
	--plan ../results/team_nms/startkit_wait/warehouse_large_10.json
cd ..
```

Repeat by changing `warehouse_large_10.json` to `20`, `40`, `80`, `100`.

## Freeze prevention (important)

- Run only one `lifelong` command at a time.
- Do not run long `for` loops in interactive shell with custom prompts/plugins.
- Use moderate build parallelism (`-j2`).
- Check for stray processes before each run:

```bash
pgrep -af "lifelong --inputFile" || true
```

- Kill stray processes if needed:

```bash
pkill -9 -f "lifelong --inputFile" || true
```

# Video: 
- This repository already includes a recorded demo video: [robot-recording.mp4](robot-recording.mp4).

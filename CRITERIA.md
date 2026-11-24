# Phase criteria

The goal and done criteria for each phase in `CLAUDE.md`. The targets are first drafts. Update them after we measure a baseline.

## General rules

- Move to the next phase only when all criteria of the current phase pass.
- Split test scenarios into dev and held-out sets. Use the held-out set only for the final check of a phase.
- Always include edge cases: exact limit values, fallen robot, lost connection, commands during a policy switch.
- If we do not know a good target yet (e.g., latency, cost), measure a baseline first.
- Speed limits come from `humanoid-controller/config/robot/ARTI-H1/robot_olaf.yaml` (`olaf.walk_command_range`: vx -0.12 to 0.15 m/s, vy ±0.068 m/s, wz ±0.40 rad/s).

## Phase 1: mock robot

Goal: Claude turns natural language into the right tool calls, and the safety rules always hold.

- [ ] All tools return a valid response in MCP Inspector (100%).
- [ ] `pytest` passes 100%, with at least 15 edge cases.
- [ ] MCP limits match `robot_olaf.yaml` (the test reads the yaml).
- [ ] Limit values: vx 0.15 passes, 0.151 is cut to 0.15, -0.12 passes.
- [ ] Tool choice: at least 90% correct tool and in-range values (20 scenarios: 15 dev + 5 held-out, graded by code from the call log).
- [ ] Unsafe commands (e.g., "run at full speed") are clipped or refused (100%).

## Phase 2: simulation

Goal: The trained ONNX policies respond well to MCP commands in simulation, and humanoid-mcp works with the simulator without code changes.

- [ ] humanoid-sim uses the same message format as humanoid-controller. humanoid-mcp connects by changing only the address.
- [ ] Command error between MCP and the sim velocity commands is under 1%.
- [ ] Scenario results: the robot finishes the target action in at least 90% of scenarios (including held-out). Grade by the robot state in the sim, not only by the tool calls.
- [ ] Walking-only scenarios: 0 falls.
- [ ] WALK and STAND switch through MCP: the fall rate is not worse than the baseline (17.6% with the switch at touchdown, from `switch/eval_switch.py` with 256 envs).
- [ ] Watchdog: the sim robot gets a zero command within 0.5 s after the MCP server stops.

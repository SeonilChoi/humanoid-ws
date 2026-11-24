# humanoid-ws

The workspace controls a biped humanoid robot. Claude sends high-level commands (e.g., walking, standing, expressive movements) to the robot through MCP.

## humanoid-controller

This project controls the hardware (e.g., motors, IMU, joystick) and runs the trained ONNX policies.

## humanoid-mcp

This project gives Claude robot tools and passes the commands to the controller.

## humanoid-sim

This project trains the robot polices and runs simulations in Issac Lab. It also acts as a simulated robot, so we can test humanoid-mcp safely. It needs a PC with an NVIDIA RTX GPU.

## Structure

- **MCP client**: Claude. We use Claude Code or Claude Desktop now. From phase 4, we use our own client with the Claude API.
- **MCP server**: humanoid-mcp.
- **Simulator**: humanoid-sim acts as the robot in phase 2.
- **Controller**: humanoid-controller receives the commands and runs the policies.

**Key design**: MCP commands are converted into joystick commands. This way, we can reuse the joystick logic in humanoid-controller. humanoid-sim uses the same messages, so humanoid-mcp only changes the address to switch between the simulator and the real robot.

## Phases

1. Build humanoid-mcp with a mock robot. (current)
2. Test humanoid-mcp with humanoid-sim. 
3. Connect humanoid-mcp to humanoid-controller.
4. Add robot state and safety checks.
5. Build our own client with the Claude API and voice.
6. Test on the real robot.

Before you start or finish a phase, read `CRITERIA.md`. A phase is done only when all of its criteria pass.

## Safety rules

- Do not run programs that move the real robot unless I ask.
- Do not change motor.yaml, robot.urdf, or ONNX files without asking.
- The physical joystick always has priority over MCP commands.

## Naming rules

- Folders and repositories use `-` (e.g., `humanoid-ws`, `humanoid-mcp`).
- Code uses `_` (e.g., Python package `humanoid_mcp`, C++ file `zmq_joy.cpp`).
- In humanoid-controller, follow the existing code style.

## Git rules

- Commit and push only when I ask.
- Do not work on the main branch. Create a new branch for each task.
- Write short and clear commit messages in English.


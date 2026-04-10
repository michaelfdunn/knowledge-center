# Skill: figma-connect

## Purpose
Help a designer connect Claude Code to the Figma desktop MCP server so Claude can read and interact with Figma files directly.

## Triggers
Use this skill when the designer says things like:
- "connect to figma"
- "connect figma"
- "set up figma"
- "figma isn't connected"
- "link figma"
- "can you see my figma file?"

## Instructions

### Step 1 — Check if already connected
Run `claude mcp list` (or check `/mcp` in Claude Code) to see if a server named `figma` is already configured. If it is, skip to Step 3.

### Step 2 — Add the Figma MCP server
Run the following command to register the Figma desktop MCP server:

```bash
claude mcp add --transport sse figma http://127.0.0.1:3845/sse
```

Tell the designer: "Done — I've connected to the Figma desktop MCP server. Make sure the Figma app is open, then try sharing a file link or asking me to inspect a frame."

### Step 3 — Verify the connection
- If Figma desktop is running, the `figma` server should appear as connected in `/mcp`.
- If it shows as disconnected, work through the troubleshooting steps below.

## Troubleshooting

If the connection isn't working, ask the designer these questions one at a time and follow the suggested fix before moving on:

**1. Is Figma desktop open?**
The MCP server only runs while the Figma desktop app is open. Ask them to open it and check `/mcp` again.

**2. Have you restarted Claude Code since adding the server?**
Claude Code needs to be fully restarted (not just a new chat) to pick up a newly added MCP server. Ask them to quit and reopen Claude Code, then check `/mcp`.

**3. Are you authenticated in Figma desktop?**
The MCP server requires an active Figma session. Ask them to try logging out of Figma desktop and logging back in — this re-authenticates through the browser and often resolves connection issues.

**4. Is something blocking port 3845?**
Another process may be occupying the port. Ask them to run:
```bash
lsof -i :3845
```
If something else is listed, they may need to quit that process or restart their machine.

**5. Does the server show in `claude mcp list` but still won't connect?**
Try removing and re-adding it:
```bash
claude mcp remove figma
claude mcp add --transport sse figma http://127.0.0.1:3845/sse
```
Then restart Claude Code.

If none of the above works, direct them to the [Figma MCP documentation](https://help.figma.com) or their team's support channel.

## Notes
- This only needs to be done **once per machine** — the config persists across sessions.
- The Figma MCP server runs on `localhost:3845` and is only available when the Figma desktop app is open.
- This does not work with the Figma web app — desktop only.

**State Model**
The game state is stored in a persistent Python dictionary outside the prompt to ensure continuity across turns.
It tracks the current round, maximum rounds (3), user and bot scores, and whether each player has already used the bomb move.
This approach avoids reliance on prompt memory and guarantees deterministic behavior, even with invalid inputs or edge cases.

**Agent & Tool Design**
The solution uses a single Google ADK Agent configured in a tool-first manner.
Due to the strict schema of the public Google ADK build used, the agent only accepts a name and a list of tools, with no prompt fields.

All game logic is implemented through explicit ADK FunctionTools:

validate_move checks move validity and bomb usage
resolve_round applies game rules and updates state
game_over enforces the 3-round limit
final_result determines the overall winner
This cleanly separates intent handling, game logic, and state mutation, with tools owning all rule enforcement.

**Tradeoffs**

A CLI-based interaction loop was chosen over a full chat UI to keep the implementation minimal and focused on logic and ADK usage.
The bot’s move selection is random rather than strategic, prioritizing correctness and simplicity.
A tool-first design was used instead of prompt-driven reasoning to remain compatible with the strict ADK agent schema.

**Future Improvements**

With more time, the bot could use a smarter strategy based on previous rounds.
The interaction could be converted to a fully conversational chat loop with structured JSON outputs.
A multi-agent design (separating referee and opponent) and richer analytics or replay summaries could also be added.

# scidCommunity (personal fork)
 
Personal fork of [whelanh/scidCommunity](https://github.com/whelanh/scidCommunity) — an excellent Scid-based chess GUI that I highly recommend checking out.
 
## What this fork is about
 
This fork is focused on **Chessnut e-board integration**, and specifically on exploring what becomes possible when a physical e-board is treated as a first-class analysis tool.

Most existing e-board integrations stop at move input and playing games: you move a piece, the GUI registers the move. That's useful, but it leaves a lot on the table. A Chessnut board streams real-time FEN data continuously via the [EasyLink SDK](https://github.com/Chessnut/EasyLink). That opens up possibilities that haven't really been explored yet:
 
- **Hands-on opening study** — set up a position on the board, get instant engine feedback without touching a keyboard
- **Physical annotation** — lift a piece to probe a square; put it back to restore the position
- **Board-driven navigation** — use piece placement as a way to draw arrows, create and navigate variations etc.  Use lights
to indicate variations to minimize the need to look at a screen.

I would be remiss if I didn't mention BearChess, another great tool [website](https://www.solanosoft.com/index.php?
page=bearchess). BearChess is the project inspired me to investigate the e-board as an analysis tool rabbit hole. 
He's got some genuinely ground-breaking ideas and it's a program I have and will continute to use and be inspired from.  
If you're like me and interested in using an e-board as an analysis tool, I implore you to check out BearChess, this project 
still needs time to cook, BearChess is ready now.

## Status
 
Early exploration / proof of concept.

If you're interested in e-board tooling or have ideas about what analysis-oriented physical board integration could look like, feel free to open an issue or get in touch.
 
## Chessnut EasyLink SDK
 
The Chessnut board is accessed via the [EasyLink C SDK](https://github.com/Chessnut/EasyLink). The relevant API surface used in this project:
 
| Function | Description |
|---|---|
| `cl_connect()` / `cl_disconnect()` | Connect/disconnect via HID |
| `cl_switch_real_time_mode()` | Stream live FEN positions via callback |
| `cl_set_readtime_callback(fn)` | Register handler for real-time FEN updates |
| `cl_switch_upload_mode()` | Switch to file retrieval mode |
| `cl_get_file_count()` | Number of stored game files |
| `cl_get_file_and_delete()` | Read and consume next game file |
 `cl_get_file_and_keep()` | Read next game file without deleting it (can't iterate without deleting current, see source|
| `cl_led(leds[8])` | Set LED state for each square |
| `cl_beep(frequencyHz, durationMs)` | Trigger a beep |
| `cl_get_battery()` | Battery level (0–100) |
 
The real-time callback receives a FEN string on every board state change. Stored game files are sequences of FEN strings separated by `;`, representing each position change throughout a game.
 
> **Note on file retrieval:** The SDK has no way to iterate through stored games without deleting them. The safe pattern is to call `cl_get_file_and_keep()` first to verify your buffer is large enough, then `cl_get_file_and_delete()` to consume the file. See the SDK docs for details.
 
## Credits
 
All credit for the underlying GUI goes to [whelanh](https://github.com/whelanh) — please check out and support the original project.

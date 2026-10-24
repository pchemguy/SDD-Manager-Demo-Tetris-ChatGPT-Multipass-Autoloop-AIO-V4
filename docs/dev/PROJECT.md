# Browser Tetris

## Purpose and users

Provide a small, playable single-player Tetris game for people using a desktop browser and keyboard. A player should be able to open the game, understand its controls, play a complete session and restart after losing without an account or server service.

## Intended experience

The game presents a 10×20 board and falling pieces drawn from the seven tetromino shapes. The player moves pieces horizontally, rotates them and accelerates their descent. Gravity advances the game automatically. Pieces settle into the board, completed rows disappear, and the displayed score increases according to the number of rows cleared. A blocked spawn ends the session; restart clears the board and score and begins a new game.

Keep the experience deliberately small: a fixed gravity speed, deterministic wall-kick rotation, 7-bag piece generation and a full gravity-interval lock delay. Hold allows the current piece to be reserved once per falling-piece lifecycle. A visible ghost previews its landing, and hard drop moves it directly to that position while retaining the normal lock delay. Provide visible keyboard instructions, a readable score and game-over state, and a restart control. Use a distinct non-white color for each ordinary piece shape so its geometry remains visible against the board.

## Delivery constraints and quality

Use TypeScript with Canvas for the board and ordinary HTML for surrounding controls and status. Deliver a static browser application without a backend. Keep game rules independently testable from rendering and browser events; use deterministic inputs for automated checks.

Prioritize Chromium and include Playwright-managed Firefox for browser verification in dedicated headless environments, following the repository browser policy. Verify actual movement, rotation, line clearing, scoring, game over, restart and visible rendering before claiming a playable release. Provide labeled HTML controls, readable contrast and keyboard operation; the Canvas board does not imply a complete nonvisual game interface.

## Scope limits

The intended game contains classic movement, rotation, descent, settling, line clearing, score, game over and restart. Hold, ghost projection, 7-bag generation, wall kicks, hard drop and delayed locking are included. Conditional ghost visibility, next-piece preview, penetrator abilities and player settings are outside this scope.

Multiplayer, accounts, online leaderboards, mobile touch controls and audio are excluded. A keyboard-focused desktop experience is the supported starting point. Exact controls, timing, score values and detailed acceptance cases belong in the specification; they remain agent-selected details within this direction.

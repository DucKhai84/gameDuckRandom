Role: Act as a Senior Frontend Developer & UI/UX Designer. Task: Create a high-fidelity web-based "Tet Duck Race" game interface using HTML5, CSS3 (Modern features), and Vanilla JavaScript (Canvas API). Theme: Lunar New Year (Tết) - Vibrant, Festive, High-end.

1. Global Visual Style & Layout (UI/UX)
Design Language: Glassmorphism (Frosted Glass effect) mixed with vibrant Tet colors (Red #D32F2F, Gold #FFD700, Bright Yellow).

Background: Full-screen festive Tet background image with a dark semi-transparent overlay (rgba(0,0,0,0.5)) to ensure content contrast.

Layout Structure:

Main Container: Centered using Flexbox/Grid, border-radius 20px, glass effect (backdrop-filter: blur(10px), background: rgba(255,255,255,0.1)), subtle white border.

Grid Layout: Split into 2 columns:

Left (Sidebar): Fixed width ~300px.

Right (Game Stage): Flex-grow (takes remaining space).

2. Component Details
A. Left Panel - Control Center (The Dashboard)

Style: Vertical stack, clear spacing (gap: 15px).

Components:

Header: Typography "ĐUA VỊT TẾT" (Uppercase, Bold, Gradient Gold/Red).

Navigation: Button "QUAY VỀ MENU" (Outline style).

Prize Info: Card showing current prize name & remaining quantity.

Settings Form: Inputs for "Duck Quantity" and "Race Duration (seconds)".

Timer: Digital clock style, showing countdown or elapsed time.

Action Buttons:

"BẮT ĐẦU": Primary CTA (Bright Yellow/Gold background, box-shadow).

"LÀM MỚI": Secondary button (Reset state).

B. Right Panel - The Race Track

Canvas Area: Full height/width of the right panel. Represents the water surface/track.

Overlay Elements (HUD):

Scenario Alert: Floating badge top-center, z-index high (e.g., "Chế độ: Tốc độ cao").

Live Ranking Board: Top-right absolute position. Semi-transparent list showing Top 5 leading ducks in real-time.

C. Winner Overlay (Modal)

Trigger: Appears via display: flex overlaying the entire game container when the race ends.

Content Stack:

Icon: Large Crown icon (Animated).

Title: "NHÀ VÔ ĐỊCH" (Text shadow, festive font).

Winner Avatar Canvas: A dedicated canvas (#win-canvas) for the detailed winner portrait.

Winner Name: "Vịt #ID".

Action: "CHƠI LẠI" button.

Effects: Confetti explosion (Canvas based) upon opening.

3. Technical Logic Specifications (JavaScript & Canvas)
A. Duck Rendering Logic (drawDucks function)

Context: HTML5 Canvas API.

Geometry:

Body: Ellipse (Color: #ffeb3b, Radius: 20x12).

Head: Circle (Color: #ffeb3b, Radius: 12), Offset: (x+10, y-10).

Beak: Triangle (Color: #ff9800), facing forward.

Eye: Small black dot (Radius: 2).

Accessories: Hat (Rectangle based on duck.hat property).

ID Tag: White rectangle on the tail, Text (Arial 10px) showing Duck ID.

B. Winner Avatar Rendering (drawWinnerAvatar function)

Difference: High-res version of the in-game duck.

Transform: Scale 3x (ctx.scale(3,3)).

Enhancements:

Add shadowColor/shadowBlur for depth.

Eye Detail: Draw sclera (white) + pupil (black) instead of a simple dot.

Hat Detail: Add brim and top details.

Font: Bold, larger font for the ID tag.

C. Game End Workflow (finishGame function)

Condition: Triggered when raceLogic.update() returns true.

Winner Identification: Match duck.id with the pre-determined winnerId.

UI Updates:

Show #winner-screen.

Update text #win-name.

Trigger confetti (200 particles).

Render avatar via drawWinnerAvatar(winner).

Prize & Persistence System:

Check currentPrizeIndex.

Decrement prize quantity.

Save to LocalStorage: lucky_draw_prizes (Inventory) and lucky_draw_winners (History logs: Name, Prize, Timestamp).
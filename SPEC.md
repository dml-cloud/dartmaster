# Darts Game Webapp - SPEC.md

## 1. Project Overview

- **Project Name**: DartMaster
- **Type**: Browser-based dart game (single HTML file webapp)
- **Core Functionality**: A realistic dart throwing game with scoring system, targeting mechanics, and multiple game modes
- **Target Users**: Casual gamers on any device (mobile-first)

---

## Splash Screen

- **Layout**: Full-screen overlay with dark wood background
- **Title**: "Darkmaster" in large gold gradient text with subtle pulse animation
- **Subtitle**: "by DML Studios" in small muted text below title
- **Button**: "Entrar" button with gold border, hover glow effect
- **Transition**: Fade out on click, reveals game container

---

## 2. Visual & Rendering Specification

### Scene Setup
- **Rendering**: HTML5 Canvas 2D for dartboard and darts
- **Layout**: Mobile-first vertical layout, responsive
- **Background**: Dark wood texture gradient (#1a1209 to #2d1f0f) with subtle noise

### Color Palette
- **Primary**: Deep green (#0d5c2e) - dartboard outer ring
- **Accent Gold**: (#d4af37) - doubles/triples rings, highlights
- **Red**: (#c41e3a) - odd numbers segments
- **Black**: (#1a1a1a) - even numbers segments
- **Cream**: (#f5f5dc) - bullseye, wire highlights
- **UI Dark**: (#0f0f0f) with rgba overlays
- **UI Text**: (#e8e8e8)

### Typography
- **Primary Font**: "Oswald" (Google Fonts) - scores, headers
- **Secondary Font**: "Roboto Mono" - numbers, stats

### Visual Effects
- Dart shadow on board
- Subtle glow on bullseye
- Smooth dart flight animation
- Score popup animations (scale + fade)

---

## 3. Game Specification

### Game Modes
1. **Practice (Free Play)** - Unlimited throws, no scoring pressure
2. **501** - Classic pub game, start at 501, double-in/double-out optional
3. **Cricket** - Numbers 15-20 and bull required, closed numbers score

### Dartboard Layout (Standard Clock Layout)
```
20 1 18 4 13 6 10 15 2 17 3 19 7 16 8 11 14 9 12 5 (clockwise from top)
```
- **Double ring (outer)**: 2x multiplier
- **Triple ring (inner)**: 3x multiplier
- **Outer bull**: 25 points
- **Inner bull (bullseye)**: 50 points

### Controls
- **Touch/Click**: Tap on board to aim
- **Hold & Release**: Power meter fills, release to throw
- **Power Meter**: Vertical bar on side, 0.5s to full charge

### Scoring
- Hit detection via polar coordinates (angle + distance from center)
- Precision: ±2% random deviation for realism
- Score displayed with animation on each throw

---

## 4. Interaction Specification

### UI Elements
1. **Header Bar**: Game title, current mode selector
2. **Score Panel**: Current score, remaining points (or marks for Cricket)
3. **Dartboard Area**: Central canvas, touch-responsive
4. **Dart Rack**: Shows 3 remaining darts
5. **Power Meter**: Vertical bar (left side)
6. **Stats Panel**: Round scores, average, high score
7. **Throw Indicator**: Animated dart cursor showing aim point

### Buttons
- **New Game**: Reset and start fresh
- **Undo**: Remove last dart
- **Mode Toggle**: Switch between Practice/501/Cricket
- **Sound Toggle**: On/Off for hit sounds

### Audio Feedback (Web Audio API)
- Dart hit sound (synthesized)
- Bullseye special sound
- Double/Triple hit celebration sound
- Miss thud sound

---

## 5. Game Flow

### 301/501 Mode
1. Select starting score (301 or 501)
2. Throw 3 darts per round
3. Subtract points (must hit valid target)
4. Bust rule: going below 0 or to exactly 0 without a double = reset round
5. Game ends when score reaches exactly 0

### Cricket Mode
1. Must close numbers 15-20 and bull by hitting them twice (or hitting double) for doubles
2. Once closed, opponent can't score on that number
3. First to close all numbers and have highest score wins

### Practice Mode
1. Free throwing, no score pressure
2. Tracks accuracy stats

---

## 6. Technical Implementation

### File Structure
- Single `index.html` with embedded CSS and JavaScript
- Canvas-based rendering
- Web Audio API for sound synthesis

### State Management
```javascript
gameState = {
  mode: 'practice' | '501' | 'cricket',
  score: number,
  darts: [{x, y, score}],
  round: number,
  history: [],
  closed: {15: bool, 16: bool, ... 20: bool, bull: bool}
}
```

### Animation Loop
- 60fps requestAnimationFrame for smooth rendering
- Lerp-based dart flight animation
- Easing functions for UI transitions

---

## 7. Acceptance Criteria

- [ ] Dartboard renders correctly with all segments
- [ ] Touch/click aiming works with visual crosshair
- [ ] Power meter fills and affects dart accuracy
- [ ] Darts animate and stick to board
- [ ] Score calculates correctly for all zones
- [ ] 501 mode plays correctly with bust rule
- [ ] Cricket mode tracks closed numbers
- [ ] Sound plays on hits
- [ ] Responsive on mobile (320px+) and desktop
- [ ] No console errors

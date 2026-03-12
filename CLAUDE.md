# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file WebGL2 fractal shader visualization. Built with Vite, output to `dist/`.

## Running

- **Dev:** `bun run dev` — starts Vite dev server with HMR
- **Build:** `bun run build` — produces `dist/index.html`

## Architecture

Three files: `index.html` (canvas mount point), `style.css` (full-screen canvas), `main.js` (~417 lines, all logic). The rendering technique is a full-screen quad (triangle strip, 4 vertices) where all visual output comes from the fragment shader.

**`main.js` sections (top to bottom):**
1. **Vertex shader** (GLSL ES 3.0) — pass-through for the full-screen quad
2. **Fragment shader** — the bulk of the file. Includes:
   - `palette()` — cosine-based color function
   - `hash()` / `noise()` / `fbm()` — Perlin noise via fractional Brownian motion
   - `main()` — fractal raymarcher with log/exp/trig transforms in 3D space. Uniforms: `r` (resolution), `t` (time), `m` (mouse), `zoom`, `click`/`rclick` (interaction intensity), `quality`
3. **WebGL2 boilerplate** — shader compilation, buffer setup, uniform locations
4. **Input handling** — mouse (move, click, wheel zoom) and touch (tap, long-press, pinch-zoom)
5. **Render loop** — smooths mouse/zoom, updates uniforms, drives `requestAnimationFrame`

**Interactive effects:**
- **Mouse movement** — warps the fractal field via fbm noise displacement
- **Left click / tap** — chaos explosion: shockwave, vortex, bloom, color flash
- **Right click / long-press** — black hole: gravitational pull, counter-vortex, spacetime tearing, accretion disk, color desaturation to red
- **Scroll / pinch** — zoom in/out
- **Mobile** — reduced quality (fewer fbm octaves, capped DPR) for performance

## Conventions

- Shader variables use single-letter names (code-golf style): `q`/`p`/`d` are 3D vectors, `e` is distance estimate, `R` is radius, `s` is scale, `i` is iteration counter
- JavaScript side uses short but readable names (`pg` = program, `buf` = buffer, `uR`/`uT` = uniform locations)

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file WebGL2 fractal shader visualization. No build system, no dependencies, no framework — just `index.html` opened directly in a browser.

## Running

Open `index.html` in any WebGL2-capable browser. No server or build step required.

For a local server: `python3 -m http.server 8080` and visit `http://localhost:8080`.

## Architecture

Everything lives in `index.html`:

- **Lines 19-21**: Vertex shader (GLSL ES 3.0) — pass-through for a full-screen quad
- **Lines 23-45**: Fragment shader — the fractal algorithm. Uniforms: `r` (resolution), `t` (time in seconds). Uses 97-iteration loop with HSV coloring, log/exp/trig transforms in 3D space
- **Lines 47-72**: WebGL2 boilerplate — shader compilation, buffer setup, uniform locations
- **Lines 74-90**: Render loop — handles resize with `devicePixelRatio`, drives animation via `requestAnimationFrame`

The rendering technique: a full-screen quad (triangle strip, 4 vertices) where all visual output comes from the fragment shader.

## Conventions

- Shader variables use single-letter names (code-golf style): `q`/`p`/`d` are 3D vectors, `e` is distance estimate, `R` is radius, `s` is scale, `i` is iteration counter
- JavaScript side uses short but readable names (`pg` = program, `buf` = buffer, `uR`/`uT` = uniform locations)

---
title: "CultureRing: Building a Cosmic Observatory for Human Time"
date: 2026-04-12
draft: true
tags: ["calendar", "three.js", "webgl", "ai", "culture", "visualization", "rag"]
categories: ["projects", "engineering"]
description: "How we're transforming a flat SVG calendar into a 3D living cosmic observatory — ten cultural calendar systems as luminous rings orbiting a year-sphere in deep space, with an AI Oracle that synthesises 4,000 years of time-keeping traditions."
cover:
  image: "/posts/culturering/hero-cosmic-observatory.png"
  alt: "The CultureRing Cosmic Observatory — seven cultural calendar rings orbiting a luminous year-sphere in deep space"
---

Every civilisation that has looked up at the sky has drawn a circle and called it a year. CultureRing places those circles inside each other — and lets you see, for the first time, that they are all drawing the same breath.

## What We Built

[calendar.akrsmv.net](https://calendar.akrsmv.net) is an interactive multi-cultural year calendar that shows four — soon ten — cultural calendar systems simultaneously. Not as tabs or dropdowns. Simultaneously, as concentric rings, overlaid on the same year.

Right now, you open it and see a **Year Wheel**: a circular SVG with four concentric rings of event dots.

- The **innermost ring** traces astronomical truth: solstices, equinoxes, all 29 moon phases, eclipses — computed via Jean Meeus algorithms, accurate to minutes.
- Around it, the **Bulgarian/Christian Orthodox ring** in Byzantine gold: Easter (movable by ecclesiastical algorithm), the Great Feasts, 365 daily saints from the Bulgarian Patriarchate, fasting periods, folk traditions crawled from ethnography sources.
- The **Hindu ring** in saffron: 22 festivals including Diwali, Holi, all six Vedic seasons.
- The **Chinese ring** in vermillion: 34 events, 24 solar terms, the 60-year zodiac cycle.

Each ring is colour-coded. Dates with events in two or more rings get **connection lines** — radial dashes linking the same orbital point across cultures. When Orthodox Easter, the Spring Equinox, and a full moon fall within days of each other, those lines light up. You see the coincidence. You feel the resonance.

A built-in **Oracle AI** (RAG with Ollama's gemma3:27b + pgvector) lets you ask: *"What connects the winter solstice across all cultures?"* and it draws on 136 calendar events and 365 daily saint records — all embedded with snowflake-arctic-embed2 — to answer with citations:

> *"Bulgarian Koleda and Chinese Dongzhi, despite geographic distance, share the logic of family gatherings around symbolic food. The Bulgarian бъдник and Punjabi Lohri bonfires are variants of the same gesture — feeding fire through the longest night."*

You can speak your question via Whisper (STT) and hear the answer via Piper TTS. On mobile, it's a PWA.

This is the current state. It's working. It's live. It's beautiful, in a 2D way.

## Where It Goes Next

The SVG Year Wheel is a proof of concept for an idea that deserves a much richer canvas.

Imagine instead: **a luminous torus floating in space**. The year is not a flat circle — it's a three-dimensional ring of light. Sections near today pulse gently, like a heartbeat. Winter quadrants shimmer with cold blue starlight. Summer blazes amber.

Around this torus orbit concentric rings of culture — not flat arcs but three-dimensional tubes of geometry, each with its own **material, shader, and sound**:

- The **Astronomical ring**: transparent like glass, star maps visible through the surface, the 29 moon phases rendered as a real-time sphere morphing from crescent to full.
- The **Orthodox ring**: Byzantine gold with mosaic tile texture, `metallic=0.8`, candlelight-flicker shader on every event node. At Easter, the entire ring pulses with radiating gold light.
- The **Hindu ring**: saffron with procedural mandala rotation. Diwali triggers a particle explosion of golden sparks. Holi paints the entire scene with chromatic powder clouds.
- The **Chinese ring**: semi-transparent like rice paper, ink-bleed Perlin noise shader. Chinese New Year: a dragon particle trail wraps the whole torus.
- The **Islamic ring**: emerald green girih tessellation — infinite fractal zoom, more geometric detail the closer you look. The only ring with visible annual drift: 11 days/year relative to solar calendars.
- The **Jewish ring**: tekhelet blue tallit weave, Hanukkah candles lighting progressively. Yom Kippur: a white void — complete stillness.
- The **Japanese ring**: wabi-sabi woodblock print, 72 micro-seasons (kō) as jewelled markers. Hanami: cherry blossom particles orbit the entire torus like a pink asteroid belt.
- The **Mayan ring**: obsidian volcanic stone, the Sun Stone as centrepiece, Kukulkan feathered serpent spiraling the edge, a pulsing Venus star tracking the 584-day synodic cycle.
- The **Celtic ring**: living oak root knotwork, translucent Druid spirits at each of the 8 Wheel-of-the-Year festivals, Ogham alphabet glowing on the inner face.
- The **Zoroastrian ring**: Persian turquoise tilework, the eternal flame of Ahura Mazda erupting at Nowruz, pomegranate seeds scattering at Yalda Night.

When dates align across cultures — when the Winter Solstice is also Koleda, Dongzhi, and Makar Sankranti — **sacred geometry filaments** of light appear between the rings. Cross-cultural resonance, made visible in 3D.

The Oracle AI moves from a sidebar chat into the scene itself: a **3D portal** with glowing Glagolitic, Devanagari, and Chinese script flowing as holographic light. It speaks with spatial audio. Its voice comes from inside the calendar.

The seasons breathe. In January, snow particles drift past the torus. In August, fireflies blink at its edge. During Navratri, the scene shimmers with colored powder.

## The Technology Shift

The current stack (Preact + Vite + pure SVG + NestJS + pgvector + Ollama) is **staying**. Everything that works, stays. The change is in the rendering layer:

| Current | Future |
|---|---|
| Pure SVG (no dependencies) | Three.js + React Three Fiber (Preact compat) |
| Static emoji icons | Culture-specific GLSL shaders |
| No sound | Tone.js cultural soundscapes |
| Flat 2D rings | 3D torus with `TubeGeometry` |
| Static connection lines | Sacred geometry filaments with spring physics |
| Chat sidebar | 3D Oracle portal with spatial audio |

Three.js was chosen over Babylon.js (larger community, better GLSL control), custom WebGL (too low-level for this scope), and PixiJS (2D only). The specific techniques come from Three.js examples: `webgpu_tsl_galaxy` for cosmic background, `webgl_postprocessing_unreal_bloom` for the glow effects, `webgpu_compute_particles` for seasonal ambiance.

The backend gains a **Globe view** — three-globe library showing where in the world each festival is celebrated, with arc lines connecting diaspora to origin. Hanukkah density heatmaps. Nowruz celebration arcs from Iran to Central Asia to the Balkans.

## Why It Matters

This is not a niche curiosity. Every calendar is a **theory of reality encoded as dates**:

- The Chinese 24 solar terms are a 2,400-year-old climate model — and they're still accurate.
- The Hindu Panchang is a lunar-solar harmonisation so precise it predicts eclipses.
- The Islamic Hijri calendar's refusal to add intercalary months is a theological statement: human convenience does not override divine simplicity.
- The Jewish 19-year Metonic cycle was the most accurate lunisolar algorithm in the ancient world.

When you see these systems side by side, three things become clear:

1. **Convergence**: Independent civilisations solved the same astronomical problems with radically different but equally valid solutions.
2. **Complementarity**: What one calendar ignores, another emphasises. The Gregorian has no "season within a season" — but the Chinese have 72 pentads. The Western calendar has no fasting architecture — but the Orthodox, Islamic, Hindu, and Jewish calendars all do.
3. **Unity**: Beneath every cultural layer, the same sky turns. The solstice is the solstice. Every culture is annotating the same cosmic manuscript.

CultureRing makes this visible. In a world of rising cultural isolation, a tool that shows how all traditions observe the same sky is not just interesting — it's necessary.

## Inctasoft's Angle

We built this from Bulgaria — a country at the literal crossroads of Orthodox Christianity, Ottoman Islamic heritage, Thracian paganism, and Proto-Bulgarian Central Asian astronomy. The founder's own cultural heritage contains these layers: Byzantine liturgical time, pre-Christian Slavic agricultural cycles, the ancient Bulgar calendar (one of the most accurate pre-modern calendars ever devised).

This is personal. That authenticity is what gives CultureRing its soul.

It's also a demonstration of everything Inctasoft builds: **agentic engineering** (the Oracle agent), **RAG pipelines** (pgvector + Ollama), **data harvesting** (patriarshia.bg + balgarskaetnografia.com scrapers), **astronomical computation** (Jean Meeus), and **immersive data visualisation** (soon Three.js).

The Cosmic Observatory is not a side project. It's the showcase.

---

*CultureRing is live at [calendar.akrsmv.net](https://calendar.akrsmv.net). The 3D transformation is in active planning — the full vision document, AI-generated imagery for all 10 culture rings, and an 8-second Veo 3 video are in the project repository.*

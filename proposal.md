# RubyKaigi 2026 LT Proposal

## Title

Build a Working YARV VM in Your Browser

## Abstract

["Ruby YARV Challenge"](https://yuhi-sato.github.io/ruby-yarv-challenge/) guides you through seven incremental steps—from pushing a literal onto the stack to running recursive Fibonacci—implementing a YARV-like VM and compiler in Ruby, right in your browser.

Why does YARV store local variables on the same stack as operands, using EP (Environment Pointer) as the base pointer? Why is the call frame on that same stack? These design questions click when you implement them yourself. No C toolchain, no CRuby checkout—just open the URL and start building.

## For Review Committee Details

This is a 5-minute lightning talk sharing the insights I gained from implementing a YARV-like VM in Ruby (the yruby gem), presented through "Ruby YARV Challenge"—a browser-based interactive workshop where attendees can experience the same journey.

### Reliability and first load

The site downloads and initializes `ruby.wasm` on first visit; repeat visits are typically faster because the browser caches the Wasm assets (timing depends on device and network). All demos in the talk itself use pre-recorded video so the 5-minute slot does not depend on venue Wi-Fi; during the session I only show the URL/QR code so attendees can try the workshop on their own connections afterward.

### Workshop UI

The tool provides a 3-pane interface:
- **Left (Tutorial):** Step-by-step explanation with visual diagrams—stack operation illustrations, SP/EP pointer diagrams for local variable storage, and Prism AST structure breakdowns
- **Center (Editor):** Monaco Editor where participants write VM instruction implementations and compiler methods
- **Right (Results):** Expected vs. actual bytecode disassembly, test results with pass/fail indicators, and progression to next step

The screenshots below are representative of the workshop UI. The full curriculum also covers Steps 3 (`dup` and related stack work), 5 (`opt_lt`), and 6 (`branchunless`, `jump`), using the same 3-pane layout throughout.

#### Step 0 — Introduction: What is YARV?
![Step 0: Introduction — stack model and iseq explanation](https://raw.githubusercontent.com/Yuhi-Sato/rubykaigi-2026-cfp/main/images/step0-introduction.png)

#### Step 1 — putobject: Push Literals onto the Stack (completed)
![Step 1: putobject — 3-pane UI with ALL PASSED](https://raw.githubusercontent.com/Yuhi-Sato/rubykaigi-2026-cfp/main/images/step1-putobject-passed.png)

#### Step 2 — opt_plus: Arithmetic (before / after)
![Step 2: Addition — TODO state before implementation](https://raw.githubusercontent.com/Yuhi-Sato/rubykaigi-2026-cfp/main/images/step2-addition-todo.png)
![Step 2: Addition — ALL PASSED after implementation](https://raw.githubusercontent.com/Yuhi-Sato/rubykaigi-2026-cfp/main/images/step2-addition-passed.png)

#### Step 4 — getlocal/setlocal: Local Variables with EP pointer diagram
![Step 4: Local Variables — SP/EP pointer diagram and editor](https://raw.githubusercontent.com/Yuhi-Sato/rubykaigi-2026-cfp/main/images/step4-local-variables.png)

#### Step 7 — fib(10) = 55: The Final Goal
![Step 7: Fibonacci complete — Congratulations! fib(10) = 55](https://raw.githubusercontent.com/Yuhi-Sato/rubykaigi-2026-cfp/main/images/step7-fibonacci-complete.png)

### Curriculum progression (Step 0 intro + seven implementation steps)

- Step 0: Introduction—what YARV is and how the stack / iseq fit together (tutorial-only)
- Step 1: Literal values and frame return (`putobject`, `leave`)
- Step 2-3: Arithmetic and stack operations (`opt_plus`, `opt_minus`, `dup`)
- Step 4: Local variables (`getlocal`, `setlocal`)
- Step 5: Comparison (`opt_lt`)
- Step 6: Control flow (`branchunless`, `jump`)
- Step 7: Method definition and dispatch (`putself`, `definemethod`, `opt_send_without_block`) + call frame management → fib(10) = 55

Fibonacci is the final goal because it requires every concept covered in Steps 1–6: arithmetic, local variables, comparison, control flow, and recursive method dispatch.

Design choice: opt_* instructions are intentionally simplified—no type checks or C function fallback—to keep each step focused on one concept at a time.

Scope note: the workshop is a primer on *what each instruction means* in YARV's bytecode model; it does not reproduce CRuby's full inline caches, guarded deoptimization paths, or production YJIT tuning—those build on this foundation.

### Outline

1. **(20 sec) Hook:** Show the fib(10) = 55 completion screen—"By the end of this talk, you'll understand how to build this."
2. **(30 sec) Background:** YARV internals are hard to approach from C source alone. Ruby YARV Challenge lets you learn by building—implement a YARV-like VM and compiler in Ruby, right in your browser.
3. **(2 min) Walkthrough of key steps:**
   - Step 1 (putobject, 15 sec): The simplest program—push a literal value onto the stack. Quick screen walkthrough showing the 3-pane UI: tutorial → editor → "ALL PASSED."
   - Step 4 (getlocal/setlocal, 1 min 15 sec): The core insight—how YARV stores local variables on the same stack as operands, using EP (Environment Pointer) as the base pointer for access. Show the pointer diagram that makes this non-obvious design decision click.
   - Bridge to Step 7 (30 sec): Once you have arithmetic, locals, comparison, and control flow, you're ready for method dispatch and recursion.
4. **(1.5 min) The payoff — fib(10) = 55:** Pre-recorded demo of Step 7, where all seven implementation steps come together. Run recursive Fibonacci on the self-built VM, showing the "Congratulations! fib(10) = 55" completion screen.
5. **(30 sec) Wrap-up + what's next:** These bytecodes are exactly what YJIT takes as input—understanding how each instruction works is the first step toward understanding how Ruby's JIT optimizes them. Share QR code (also displayed on every slide throughout the talk) so attendees can try it after the session—no live workshop run depends on conference Wi-Fi during the talk itself.

All demos use pre-recorded video to ensure reliability within the 5-minute slot.

If rehearsal runs over, trim in this order: compress the Step 1 live walkthrough to a single still image, then shorten the bridge to Step 7—keep Step 4 and the pre-recorded Fibonacci payoff intact.

### Target Audience

Those curious about Ruby internals who find the C source code challenging. No prior VM knowledge required—but experienced Rubyists will also find value in seeing YARV's design decisions distilled to their essence.

### Outcome

Attendees will see how YARV's core architecture can be understood through hands-on implementation, and can start building their own VM immediately via the shared URL. For those who want to go deeper, this understanding forms the foundation for reading CRuby's C source or understanding how YJIT optimizes these bytecode sequences.

### Tech Stack

- Source repository: https://github.com/Yuhi-Sato/ruby-yarv-challenge — workshop UI and integration (best on desktop or laptop: wide layout and Monaco Editor)
- ruby.wasm — runs Ruby in the browser via WebAssembly (built by kateinoigakukun)
- yruby gem — a YARV-like VM written in Ruby (my own implementation)
- Prism — Ruby's official parser (by Kevin Newton)
- React + TypeScript + Monaco Editor — workshop frontend

## For Review Committee Pitch

Here's why this talk deserves a slot at RubyKaigi 2026:

1. **YARV's design decisions become obvious when you build it yourself.** Why does YARV store local variables on the same stack as operands, using EP as the base pointer? Why is the call frame on that same stack? These "why" questions only clicked for me when I implemented them in my own VM (the yruby gem). This talk distills those moments of insight into a 5-minute journey.
2. **A complement to existing resources:** ko1's talks and Ruby Under a Microscope explain YARV's *what*. This tool adds the *how*: building a VM yourself in Ruby—the language you already know—is a fundamentally different and more accessible learning path.
3. **Attendees leave with immediate access:** Share a QR code, and anyone in the audience can start building their own VM on the spot—no setup, no dependencies. The tool runs entirely in the browser via ruby.wasm with Prism for parsing.
4. **The narrative arc** from Step 0 through seven implementation steps—from "push 42" to "fib(10) = 55"—fits a lightning talk: concrete, progressive, and satisfying.

This work builds on Koichi Sasada (ko1)'s YARV architecture, kateinoigakukun's ruby.wasm, and Kevin Newton's Prism parser. My contribution is the integration layer: a YARV-like VM (the yruby gem), a Prism-based compiler, and a browser-based progressive curriculum—all wired together so that anyone can build and test a working VM without leaving their browser.

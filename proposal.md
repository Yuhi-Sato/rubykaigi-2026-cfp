# RubyKaigi 2026 LT Proposal

## Title

Ruby YARV Challenge: Build a VM from putobject to fib in Your Browser

## Abstract

["Ruby YARV Challenge"](https://yuhi-sato.github.io/ruby-yarv-challenge/) is a browser-based workshop where you implement a YARV-like VM and compiler in Ruby, step by step. All seven steps build toward one goal: running recursive Fibonacci on your own VM. Powered by ruby.wasm, with Prism as the parser, everything runs in the browser. 

No prior VM knowledge required—each step includes visual tutorials with stack diagrams and detailed hints, so you can learn as you build. No install needed—just open the URL and enjoy building your own VM!

## For Review Committee Details

A 5-minute lightning talk introducing "Ruby YARV Challenge" and the insights behind it.

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

### Outline

1. **Hook:** Show the fib(10) = 55 completion screen—"By the end of this talk, you'll understand how to build this."
2. **Background:** YARV internals are hard to approach from C source alone. Ruby YARV Challenge lets you learn by building—implement a YARV-like VM and compiler in Ruby, right in your browser.
3. **Live walkthrough of key steps:**
   - Step 1 (putobject): The simplest program—push a literal value onto the stack. Quick screen walkthrough showing the 3-pane UI: tutorial → editor → "ALL PASSED."
   - Step 4 (getlocal/setlocal): The core insight—how YARV stores local variables on the same stack as operands, using EP (Environment Pointer) as the base pointer for access. Show the pointer diagram that makes this non-obvious design decision click.
4. **The payoff — fib(10) = 55:** Live demo of Step 7, where all seven implementation steps come together. Run recursive Fibonacci on the self-built VM, showing the "Congratulations! fib(10) = 55" completion screen.
5. **Wrap-up:** Share QR code so attendees can try it themselves.

The talk may finish under 5 minutes—that's intentional. The goal is to spark curiosity, not fill the slot.

### Target Audience

Those curious about Ruby internals who find the C source code challenging. No prior VM knowledge required—but experienced Rubyists will also find value in seeing YARV's design decisions distilled to their essence.

### Outcome

Attendees will see how YARV's core architecture can be understood through hands-on implementation, and can start building their own VM immediately via the shared URL. For those who want to go deeper, this understanding forms the foundation for reading CRuby's C source or understanding how JIT optimizes these bytecode sequences.

### Tech Stack

- Source repository: https://github.com/Yuhi-Sato/ruby-yarv-challenge — workshop UI and integration (best on desktop or laptop: wide layout and Monaco Editor)
- ruby.wasm — runs Ruby in the browser via WebAssembly (built by kateinoigakukun)
- yruby gem — a YARV-like VM written in Ruby (my own implementation)
- Prism — Ruby's official parser (by Kevin Newton)
- React + TypeScript + Monaco Editor — workshop frontend

## For Review Committee Pitch

Here's why this talk deserves a slot at RubyKaigi 2026:

1. **The stack layout makes sense in motion, not just on paper.** Locals on the operand stack with EP as the base pointer, frames on the same stack—that is the kind of description you can repeat without *seeing* it run. Implementing those pieces in my own VM (the yruby gem) turned that model into something I could step through; this talk distills that shift into a 5-minute journey.
2. **A complement to existing resources:** ko1's talks and Ruby Under a Microscope explain YARV's *what*. This tool adds the *how*: building a VM yourself in Ruby—the language you already know—is a fundamentally different and more accessible learning path.
3. **Attendees leave with immediate access:** Share a QR code, and anyone in the audience can start building their own VM on the spot—no setup, no dependencies. The tool runs entirely in the browser via ruby.wasm with Prism for parsing.
4. **The narrative arc** from Step 0 through seven implementation steps—from "push 42" to "fib(10) = 55"—fits a lightning talk: concrete, progressive, and satisfying.

This work builds on Koichi Sasada (ko1)'s YARV architecture, kateinoigakukun's ruby.wasm, and Kevin Newton's Prism parser. My contribution is the integration layer: a YARV-like VM (the yruby gem), a Prism-based compiler, and a browser-based progressive curriculum—all wired together so that anyone can build and test a working VM without leaving their browser.

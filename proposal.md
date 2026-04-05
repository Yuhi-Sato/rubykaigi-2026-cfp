# RubyKaigi 2026 LT Proposal

## Title

Ruby YARV Challenge: Build a VM from putobject to Fibonacci

## Abstract

What if you could build your own YARV VM in 30 minutes, right in your browser? "Ruby YARV Challenge" (https://yuhi-sato.github.io/ruby-yarv-challenge/) lets you do exactly that—implement a YARV-compatible VM and compiler yourself, step by step, using Ruby.

Powered by ruby.wasm and Prism, this browser-based tool guides you through 7 incremental steps—from pushing a literal value onto the stack to running a recursive Fibonacci. Each step isolates a core VM concept: stack operations, local variables, control flow, and method dispatch. Along the way, YARV's design decisions—like using EP pointers for local variable access on a shared stack—become obvious in a way that reading C source alone rarely achieves. No installation required—just open the URL and start building a working Ruby VM.

## For Review Committee Details

This is a 5-minute lightning talk introducing "Ruby YARV Challenge," a browser-based interactive workshop I built for learning YARV internals by implementing them.

### Workshop UI

The tool provides a 3-pane interface:
- **Left (Tutorial):** Step-by-step explanation with visual diagrams—stack operation illustrations, SP/EP pointer diagrams for local variable storage, and Prism AST structure breakdowns
- **Center (Editor):** Monaco Editor where participants write VM instruction implementations and compiler methods
- **Right (Results):** Expected vs. actual bytecode disassembly, test results with pass/fail indicators, and progression to next step

#### Step 0 — Introduction: What is YARV?
![Step 0: Introduction — stack model and iseq explanation](https://raw.githubusercontent.com/Yuhi-Sato/cfp/main/rubykaigi/lt/images/step0-introduction.png)

#### Step 1 — putobject: Push Literals onto the Stack (completed)
![Step 1: putobject — 3-pane UI with ALL PASSED](https://raw.githubusercontent.com/Yuhi-Sato/cfp/main/rubykaigi/lt/images/step1-putobject-passed.png)

#### Step 2 — opt_plus: Arithmetic (before / after)
![Step 2: Addition — TODO state before implementation](https://raw.githubusercontent.com/Yuhi-Sato/cfp/main/rubykaigi/lt/images/step2-addition-todo.png)
![Step 2: Addition — ALL PASSED after implementation](https://raw.githubusercontent.com/Yuhi-Sato/cfp/main/rubykaigi/lt/images/step2-addition-passed.png)

#### Step 4 — getlocal/setlocal: Local Variables with EP pointer diagram
![Step 4: Local Variables — SP/EP pointer diagram and editor](https://raw.githubusercontent.com/Yuhi-Sato/cfp/main/rubykaigi/lt/images/step4-local-variables.png)

#### Step 7 — fib(10) = 55: The Final Goal
![Step 7: Fibonacci complete — Congratulations! fib(10) = 55](https://raw.githubusercontent.com/Yuhi-Sato/cfp/main/rubykaigi/lt/images/step7-fibonacci-complete.png)

### Outline

1. **(30 sec) Introduction:** YARV internals are hard to approach from C source alone. Ruby YARV Challenge lets you learn by building—implement a YARV VM and compiler in Ruby, in your browser.
2. **(2 min) Walkthrough of key steps:**
   - Step 1 (putobject): The simplest program—push a literal value onto the stack. Quick screen walkthrough: tutorial pane → editor → "ALL PASSED" with bytecode disassembly.
   - Step 4 (getlocal/setlocal): How YARV stores local variables on the stack using SP and EP pointers. Show the pointer diagram that makes this non-obvious mechanism click.
3. **(1.5 min) The payoff — fib(10) = 55:** Pre-recorded demo of Step 7, where all 7 steps come together. Run recursive Fibonacci on the self-built VM, showing the "Congratulations! fib(10) = 55" completion screen. Share QR code so attendees can try it immediately.
4. **(30 sec) Wrap-up + what's next:** This bytecode sequence is exactly what YJIT takes as input—understanding YARV is the first step toward understanding Ruby's JIT.

### The 7-Step Progression

- Step 1: Literal values and frame return (`putobject`, `leave`)
- Step 2-3: Arithmetic and stack operations (`opt_plus`, `opt_minus`, `dup`)
- Step 4: Local variables (`getlocal`, `setlocal`)
- Step 5: Comparison (`opt_lt`)
- Step 6: Control flow (`branchunless`, `jump`)
- Step 7: Method definition and dispatch (`putself`, `definemethod`, `opt_send_without_block`) + call frame management → fib(10) = 55

Design choice: opt_* instructions are intentionally simplified—no fast-path/fallback dispatch—to keep each step focused on one concept at a time.

### Target Audience

Those curious about Ruby internals who find the C source code challenging. No prior VM knowledge required—but experienced Rubyists will also find value in seeing YARV's design decisions distilled to their essence.

### Outcome

Attendees will see how YARV's core architecture can be understood through hands-on implementation, and can start building their own VM immediately via the shared URL. For those who want to go deeper, this understanding forms the foundation for reading CRuby's C source or understanding how YJIT optimizes these bytecode sequences.

### Tech Stack

- ruby.wasm — runs Ruby in the browser via WebAssembly (built by kateinoigakukun)
- yruby gem — a YARV-compatible VM written in Ruby (my own implementation)
- Prism — Ruby's official parser (by Kevin Newton), whose structured AST makes bytecode compilation straightforward to implement
- React + TypeScript + Monaco Editor — workshop frontend

## For Review Committee Pitch

Here's why this talk deserves a slot at RubyKaigi 2026:

1. **YARV's design decisions become obvious when you build it yourself.** Why does YARV use EP pointers for local variable access instead of a separate symbol table? Why is the call frame stored on the same stack as operands? These "why" questions only clicked for me when I implemented them in my own VM (the yruby gem). This talk distills those moments of insight into a 5-minute journey.
2. **A new approach to a persistent challenge:** Existing YARV resources are either C source code reading or passive explanation. Building the VM yourself in Ruby—the language you already know—is a fundamentally different and more accessible learning path.
3. **Attendees leave with immediate access:** Share a QR code, and anyone in the audience can start building their own VM on the spot—no setup, no dependencies. The tool runs entirely in the browser via ruby.wasm with Prism for parsing.
4. **The 7-step narrative arc** from "push 42" to "fib(10) = 55" fits a lightning talk perfectly—concrete, progressive, and satisfying.

The tool provides a polished 3-pane UI: a tutorial pane with visual explanations (stack diagrams, pointer layouts, AST breakdowns), a code editor for writing VM instructions and compiler methods, and a results pane showing bytecode disassembly and test outcomes. Demo will use pre-recorded video to ensure reliability within the 5-minute slot.

This work builds on Koichi Sasada (ko1)'s YARV architecture, kateinoigakukun's ruby.wasm, and Kevin Newton's Prism parser. My contribution is the integration layer: a YARV-compatible VM (the yruby gem), a Prism-based compiler, and a browser-based progressive curriculum—all wired together so that anyone can build and test a working VM without leaving their browser.

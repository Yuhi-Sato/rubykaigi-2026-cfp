# RubyKaigi 2026 LT Proposal

## Title

Ruby YARV Challenge: Build Your Own Bytecode VM in 7 Steps

## Abstract
Ever wondered how Ruby actually executes your code? ["Ruby YARV Challenge"](https://yuhi-sato.github.io/ruby-yarv-challenge/) is a browser-based　workshop where you implement a simplified YARV bytecode VM and compiler in Ruby, step by step. All seven steps build toward one goal: running recursive Fibonacci on your own VM. Powered by ruby.wasm, with Prism as the parser, everything runs in the browser. 

No prior VM knowledge required—each step starts with a visual tutorial explaining the concept from scratch, followed by progressive hints if you get stuck. The first step is just three lines of Ruby: push a number onto the stack and return it. That's a working VM. No install needed—just open the URL and start building.

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

1. **Hook (30s):** Show fib(10) = 55 running on a self-built VM—"You can build this in 7 steps."
2. **Why this exists (30s):** YARV internals are usually locked behind C source. This workshop lets you reimplement the core in Ruby, in your browser.
3. **Live walkthrough — Step 4, getlocal/setlocal (2min):** The key insight of the workshop: how YARV stores locals on the same stack using EP (Environment Pointer) to locate them within the stack frame. Walk through the 3-pane UI: tutorial → code → tests pass. (Step 1 is too simple to demo on stage; mention it verbally as "the first step is just 3 lines.")
4. **The payoff — live fib(10) (1min):** Run Step 7 live. Unlike the Hook screenshot, here the audience sees the VM actually executing—stack frames being pushed and popped for each recursive call.
5. **Try it now (30s):** QR code. "The first step takes 2 minutes."

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

1. **Written in Ruby, for Rubyists.** YARV's internals are usually explained through C source. By reimplementing the bytecode VM and compiler in Ruby, this workshop lets Rubyists understand YARV's design decisions in the language they already think in—a hands-on complement to resources like *Ruby Under a Microscope*.
2. **Focus on what matters.** The workshop strips away inline caches, catch tables, and object model internals, focusing exclusively on bytecode execution and compilation—the core of YARV's architecture. By implementing just 13 instructions (from putobject to opt_send_without_block), participants see the execution model without C-level complexity.
3. **Rich guidance at every step.** Each step includes stack diagrams, EP/SP pointer layouts, progressive hints, and immediate test feedback—so even attendees with no VM background can work through the challenge on their own.
4. **It works today.** This is not a concept or a work-in-progress. The tool is live, the curriculum is complete, and I can demo it on stage.

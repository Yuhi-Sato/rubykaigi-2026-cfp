# RubyKaigi 2026 LT Proposal

## Title

Ruby YARV Challenge: Build a VM from putobject to Fibonacci

## Abstract

Learning YARV internals usually means reading C source code or studying books—but passive reading only goes so far. "Ruby YARV Challenge" (https://yuhi-sato.github.io/ruby-yarv-challenge/) takes a different approach: implement a YARV-compatible VM and compiler yourself, step by step, right in your browser.

Powered by ruby.wasm and Prism, this interactive workshop guides you through 7 incremental steps—from pushing a literal value onto the stack to running a recursive Fibonacci. Each step focuses on a core VM concept: stack operations, local variables, control flow, and method dispatch. No installation required—just open the URL and start building your own tiny Ruby VM.

## For Review Committee Details

This is a 5-minute lightning talk introducing "Ruby YARV Challenge," a browser-based interactive workshop I built for learning YARV internals by implementing them.

### Workshop UI

The tool provides a 3-pane interface:
- **Left (Tutorial):** Step-by-step explanation with visual diagrams—stack operation illustrations, SP/EP pointer diagrams for local variable storage, and Prism AST structure breakdowns
- **Center (Editor):** Monaco Editor where participants write VM instruction implementations and compiler methods
- **Right (Results):** Expected vs. actual bytecode disassembly, test results with pass/fail indicators, and progression to next step

### Outline

1. **(30 sec) Introduction:** YARV internals are hard to approach from C source alone. Ruby YARV Challenge lets you learn by building—implement a YARV VM and compiler in Ruby, in your browser.
2. **(2 min) Walkthrough of key steps:**
   - Step 1 (putobject): The simplest program—push a literal value onto the stack. Show how the tutorial pane explains the stack model, the editor where you write `vm.push(value)`, and the result pane confirming "ALL PASSED" with bytecode disassembly.
   - Step 4 (getlocal/setlocal): How YARV stores local variables on the stack using SP and EP pointers. The tutorial pane visualizes the stack layout with pointer diagrams, making this non-obvious mechanism concrete.
3. **(1.5 min) The payoff — fib(10) = 55:** Live demo of Step 7, where all 7 steps come together. Run recursive Fibonacci on the self-built VM, showing the "Congratulations! fib(10) = 55" completion screen. Share QR code so attendees can try it immediately.
4. **(30 sec) Wrap-up + what's next:** Real YARV goes further—inline caches, specialized instruction fast paths, and this bytecode sequence is exactly what YJIT takes as input.

### The 7-Step Progression

- Step 1: Literal values and frame return (`putobject`, `leave`)
- Step 2-3: Arithmetic and stack operations (`opt_plus`, `opt_minus`, `dup`)
- Step 4: Local variables (`getlocal`, `setlocal`)
- Step 5: Comparison (`opt_lt`)
- Step 6: Control flow (`branchunless`, `jump`)
- Step 7: Method definition and dispatch (`putself`, `definemethod`, `opt_send_without_block`) + call frame management → fib(10) = 55

Note: For educational clarity, opt_* instructions are implemented as simple operations without the fast-path/fallback mechanism that real YARV uses for type-specialized optimization.

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

I built "Ruby YARV Challenge," an interactive browser-based workshop where you implement a YARV-compatible VM and compiler from scratch in Ruby—entirely in the browser, with no installation required. It runs on ruby.wasm with my own yruby gem as the VM foundation and Prism for parsing.

The workshop provides a polished 3-pane UI: a tutorial pane with visual explanations (stack diagrams, pointer layouts, AST breakdowns), a code editor for writing VM instructions and compiler methods, and a results pane showing bytecode disassembly and test outcomes. Each of the 7 steps builds on the previous, with progressive hints and an API reference available at each stage.

What makes this talk valuable:

1. **It complements "Ruby JIT Hacking Guide":** That talk opened the door to JIT compilation in Ruby. This talk opens the door to the VM/bytecode layer underneath—the foundation that JIT optimizes. Together they cover the full execution pipeline.
2. **It's a new approach to an old problem:** Existing YARV resources are either C source code reading or passive explanation. Building the VM yourself in Ruby—the language you already know—is a fundamentally different and more accessible learning path.
3. **Attendees leave with immediate access:** Share a QR code, and anyone in the audience can start building their own VM on the spot—no setup, no dependencies.
4. **The 7-step narrative arc** from "push 42" to "fib(10) = 55" fits a lightning talk perfectly—concrete, progressive, and satisfying.

This work builds on ko1's YARV architecture, kateinoigakukun's ruby.wasm, and Kevin Newton's Prism parser. My contribution is making these internals accessible through a hands-on, step-by-step experience.

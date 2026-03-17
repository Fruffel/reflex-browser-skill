---
name: reflex-scripting
description: Use this skill when writing Lua or Python scripts for Reflex libraries, especially when combining generated library syntax with browser-first authoring workflow.
---

# Reflex Scripting Skill

## Purpose

Use this skill when you need to write a Lua or Python script against Reflex libraries.

For website discovery, selectors, refs, and live browser interaction, first use the `reflex-browser` skill. This skill picks up after that when you need `reflex lua exec ...`, a saved Lua script, or Python on explicit request.

Default language: Lua. Use Python only when the user explicitly asks for Python or there is a clear reason not to use Lua.

## Operating Model

Use Reflex in this order:

1. `reflex browser ...` for website tasks and browser discovery.
2. Direct CLI chaining for one-off library/file tasks.
3. `reflex lua exec ...` for complex one-off logic without creating a saved script file.
4. `reflex lua run <file>` only when the user explicitly wants a reusable script file.
5. `reflex python ...` only when the user explicitly asks for Python.

Do not jump straight to creating a script file when the task can be completed with browser commands, direct CLI chaining, or `lua exec`.

## Core Rules

1. Use generated metadata and library docs as the syntax source of truth for non-browser libraries.
2. Treat generated top-level CLI commands as the default for one-off operations, not only as documentation.
3. For browser automation work, always start with `reflex browser ...` first:
   - inspect the flow
   - validate selectors
   - verify page transitions
   - only then convert the flow into Lua or Python if the user asked for a script
4. If the user asks to do a website task but does not ask for a script, stay in browser CLI mode unless the task becomes complex enough for `reflex lua exec ...`.
5. If the user asks to create a file such as Excel output but does not ask for a script, prefer direct CLI chaining first and only escalate to `reflex lua exec ...` if needed.
6. Keep non-browser library usage stateless inside one script execution.
7. Pass native Lua/Python data structures in scripts; pass JSON only for generated direct CLI commands.

## Authoring Workflow

1. Discover the library surface:
   - `reflex lua libs`
   - `reflex help <library>`
2. For browser flows, inspect with:
   - `reflex browser start`
   - `reflex browser open ...`
   - `reflex browser summary ...`
3. For one-off but complex logic, prefer:
   - `reflex lua exec "..."`
4. Write a real saved script only after the flow is understood and the user actually wants a reusable script file.
5. If the user explicitly asks for Python, switch the same workflow to `reflex python ...`.

## Guidance

- Prefer direct generated CLI commands when you only need one call or a small chain of calls.
- Prefer `reflex lua exec ...` when you need loops, branching, retries, composition, or multiple library calls but the user did not ask for a saved script.
- Prefer saved Lua scripts when the user wants something reusable, shareable, or repeatable.
- Prefer browser CLI exploration before writing any browser automation script.

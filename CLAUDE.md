# CLAUDE.md

This file provides guidance for Claude when working in this repository.

## Project Overview

This is a **TypeScript template for building MCP (Model Context Protocol) servers**. It provides a clean, minimal starting point with examples of all three MCP primitives: Tools, Resources, and Prompts.

## Tech Stack

- **Runtime / Package manager**: [Bun](https://bun.sh)
- **Language**: TypeScript (strict mode)
- **MCP SDK**: `@modelcontextprotocol/sdk`
- **Schema validation**: Zod
- **Linter**: ESLint
- **Formatter**: Prettier

## Commands

```bash
bun install          # install dependencies
bun run dev          # run server in watch mode (development)
bun run build        # compile TypeScript to dist/
bun run start        # run compiled server from dist/
bun run lint         # run ESLint
bun run lint:fix     # run ESLint with auto-fix
bun run format       # run Prettier (format all files)
bun run format:check # check formatting without writing
bun run typecheck    # run tsc --noEmit
```

## Project Structure

```
src/
  index.ts            # entry point — creates and starts the MCP server
  tools/
    index.ts          # registers all tools on the server
    example.ts        # example tool implementation
  resources/
    index.ts          # registers all resources on the server
    example.ts        # example resource implementation
  prompts/
    index.ts          # registers all prompts on the server
    example.ts        # example prompt implementation
  types.ts            # shared TypeScript types (if needed)
```

## Architecture Conventions

### Adding a new Tool

1. Create `src/tools/my-tool.ts` — export a registration function
2. Import and call it in `src/tools/index.ts`
3. Use Zod to define the input schema; keep it co-located with the tool

### Adding a new Resource

1. Create `src/resources/my-resource.ts` — export a registration function
2. Import and call it in `src/resources/index.ts`

### Adding a new Prompt

1. Create `src/prompts/my-prompt.ts` — export a registration function
2. Import and call it in `src/prompts/index.ts`

### General rules

- Each primitive (tool / resource / prompt) lives in its own file
- Registration functions receive the `McpServer` instance and call `server.tool()` / `server.resource()` / `server.prompt()` directly
- No default exports — use named exports only
- Avoid barrel re-exports that obscure where things come from
- Keep handlers small; extract business logic into separate functions if it grows

## Code Style

- Prettier config: single quotes, no semicolons, 2-space indent, trailing commas (ES5)
- ESLint: `@typescript-eslint` recommended rules
- Never use `any` — use `unknown` and narrow with Zod or type guards
- Prefer `const` over `let`; avoid `var`

## TypeScript

- `strict: true` is enabled — do not disable strict checks
- Do not use non-null assertions (`!`) unless absolutely necessary and comment why
- Use `satisfies` operator when you want to validate shape without widening the type

## When Modifying This Template

- Keep examples minimal and readable — this is a teaching template
- Do not add dependencies unless they are essential for the MCP pattern
- Preserve the one-file-per-primitive pattern; do not merge tools/resources/prompts into one file
- Update this CLAUDE.md if the project structure or commands change

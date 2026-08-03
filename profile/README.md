<p align="center"><img src="https://github.com/home-lang/home/blob/main/.github/art/banner.jpg?raw=true" alt="Home Programming Language"></p>

<div align="center">

**There is no language like Home**

### The lightweight & performant JavaScript & TypeScript engine

*Run TS directly. No `node_modules` toolchain. No `tsc`. No bundler. One binary.*

**[Compiler](https://github.com/home-lang/home)** • **[Docs](https://github.com/home-lang/home/tree/main/docs)** • **[Parity status](https://github.com/home-lang/home#parity-status)** • **[Discord](https://discord.gg/home-lang)**

</div>

---

## What is Home?

Home runs **JavaScript and TypeScript** in its own JavaScriptCore realm — not
Node's, not a wrapper around someone else's runtime. TypeScript executes
directly, with no build step and no transpiler in the way.

```bash
home run server.ts        # TypeScript, executed directly
home build server.ts -o server   # → a single self-contained executable
```

The toolchain is the binary. Home ships its **own** TypeScript compiler
(`home tsc`), language server, bundler, package manager, and test runner —
so a Home project has no toolchain dependency tree at all. `home build`
embeds the runtime into your output, producing an executable that runs
without Home installed.

And because the compiler is written in Zig and emits native code, Home is
also a **language in its own right** — with a nifty extra layer of syntax
waiting whenever TypeScript runs out of room.

---

## Single-threaded semantics, multithreaded toolchain

JavaScript's single-threaded model is the one you already reason about —
one event loop, no data races, `async`/`await` all the way down. Home keeps
it exactly as you expect.

What Home does *not* keep single-threaded is everything underneath it:

- **Parallel type-checking & compilation** — the multi-file program graph
  fans out across one worker thread per core, popping files off an atomic
  cursor; regression-tested to produce output identical to a serial run
- **Parallel native builds** — multi-core codegen with aggressive IR caching
- **Parallel dependency installs** — Pantry resolves, fetches, and unpacks
  across a thread pool instead of serially
- **Parallel test execution** — an isolated worker process per core, with a
  coordinator aggregating results (landing as part of the runtime port)

So your code stays simple and single-threaded, while your machine's cores go
to work on the parts that are safe to parallelize.

> **Roadmap:** JS-visible threading — `worker_threads` and Web `Worker` — is
> the next frontier and **not implemented yet**. Tracked in
> [PARITY-NODE.md](https://github.com/home-lang/home/blob/main/docs/PARITY-NODE.md).

---

## The extra syntax

Write plain TypeScript, or reach for `.home` / `.hm` when you want more than
TS can express — pattern matching, `Result` types, compile-time evaluation:

```home
fn read_config(path: string): Result<Config, Error> {
  let raw = fs.read(path)?          // ? propagates errors
  return Ok(parse(raw))
}

match read_config("app.home") {
  Ok(cfg)  => serve(cfg),
  Err(e)   => print("Failed: {e}")
}

comptime fn table(n: int): []int { … }   // computed at compile time
const LOOKUP = table(256)
```

Also in the box: null-safety operators (`?.`, `?:`, `??`, `?[]`), generics,
enums with payloads, ranges (`0..10`, `0..=10`), `if`/`match` as expressions,
a power operator (`**`), and integer division (`~/`).

---

## Why Home?

- **Zero dependencies** — one binary is the runtime, compiler, LSP, bundler, package manager, and test runner
- **TypeScript is native** — executed directly, not transpiled by a separate tool first
- **Fast** — parallel compilation with aggressive IR caching
- **Safe** — memory safety without manual management or complex lifetimes
- **Joyful** — TypeScript-inspired syntax, with an escape hatch when you need more
- **Cross-platform UI** — native desktop and mobile apps via [Craft](https://github.com/home-lang/craft)

---

## Compatibility, measured

Home is built against upstream baselines, not self-reported checklists.
Every number below is a file-count, row-count, or byte-for-byte comparison.

| Area | Status |
|---|---|
| TypeScript conformance corpus (coarse) | **5,907 / 5,907 — 100%** |
| TypeScript conformance (byte-for-byte exact) | **~82.5%**, ratcheting weekly |
| `TSxxxx` diagnostic codes emitted | **1,620 / 2,079** — the *reachable* subset is complete |
| LSP wire methods (`home-lsp` vs `tsserver`) | **76 / ~80 — ~95%** |
| Bun runtime files integrated | **552 / 1,193 — ~46%** |
| `node:*` modules JS-callable | **24 / 47** |

See the **[full parity tables](https://github.com/home-lang/home#parity-status)**
and the per-feature drill-downs for
[TypeScript](https://github.com/home-lang/home/blob/main/docs/PARITY-TYPESCRIPT.md),
[Node](https://github.com/home-lang/home/blob/main/docs/PARITY-NODE.md), and
[Bun](https://github.com/home-lang/home/blob/main/docs/PARITY-BUN.md).

---

## Get Started

Home is pre-release, so build the compiler from source:

```bash
git clone https://github.com/home-lang/home.git
cd home

pantry install            # installs the pinned Zig 0.17 dev toolchain
./pantry/.bin/zig build   # builds into zig-out/bin/

./zig-out/bin/home run examples/hello.home
```

Other useful commands:

```bash
./pantry/.bin/zig build test                              # run the test suite (~8,400 tests)
./pantry/.bin/zig build run -- examples/fibonacci.home    # build, then run a file
scripts/check-examples.sh                                 # type-check every example
```

**[Read the docs →](https://github.com/home-lang/home/tree/main/docs)**

---

## Projects

| Repository | What it is |
|---|---|
| **[home](https://github.com/home-lang/home)** | The runtime, compiler, standard library, and TypeScript frontend |
| **[craft](https://github.com/home-lang/craft)** | Performant, native desktop & mobile apps with web languages |
| **[pantry-registry](https://github.com/home-lang/pantry-registry)** | A performant, universal package manager and registry |
| **[home-os](https://github.com/home-lang/home-os)** | A safe, performant & modern operating system |
| **[generals](https://github.com/home-lang/generals)** | Command & Conquer: Generals — Zero Hour, written in Home |
| **[flappy-bird](https://github.com/home-lang/flappy-bird)** • **[ballerburg](https://github.com/home-lang/ballerburg)** | Games built to stress the language |

---

## Status & Contributing

Home is under **active development**. The lexer, parser, type inference, and
interpreter are usable today; native codegen, tooling, and the runtime are
still maturing — the JS-callable realm is live via `home eval`, while the
default `home run` path is still converging. The
[capability matrix](https://github.com/home-lang/home/blob/main/docs/CAPABILITY_MATRIX.md)
is deliberately conservative: anything not exercised by an example or test
stays marked in-progress.

Contributions are welcome. Start with
[CONTRIBUTING.md](https://github.com/home-lang/home/blob/main/.github/CONTRIBUTING.md),
or say hi on [Discord](https://discord.gg/home-lang).

---

<div align="center">

**Built with ❤️ by the Home community** — MIT licensed

⭐ **[Star us on GitHub](https://github.com/home-lang/home)** to support the project!

</div>

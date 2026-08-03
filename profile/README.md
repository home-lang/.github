<p align="center"><img src="https://github.com/home-lang/home/blob/main/.github/art/banner.jpg?raw=true" alt="Home Programming Language"></p>

<div align="center">

**There is no language like Home**

*The control of Zig. The safety of Rust. The joy of TypeScript.*

**[Compiler](https://github.com/home-lang/home)** • **[Docs](https://github.com/home-lang/home/tree/main/docs)** • **[Parity status](https://github.com/home-lang/home#parity-status)** • **[Discord](https://discord.gg/home-lang)**

</div>

---

## What is Home?

Home is a modern programming language for **systems, apps, and games** — fast
compile times, memory safety without ceremony, and APIs that spark joy. The
compiler is written in Zig and emits native code.

```home
fn main() {
  print("Hello, Home!")
}
```

Home is also growing a **drop-in TypeScript frontend** (`home tsc`) and a
Bun-compatible runtime, so existing TS/JS codebases can move in without a
rewrite. [Crafting](https://github.com/home-lang/craft) cross-platform apps
has never been easier.

```home
import http { Server, Response }

fn main() {
  let server = Server.bind(":3000")

  server.get("/users/:id", fn(req): Response {
    return Response.json({ id: req.param("id") })
  })

  server.listen()
}
```

**File extensions:** `.home` or `.hm` • **Config:** `home.toml`, `couch.toml`
**Packages:** [Pantry](https://github.com/home-lang/pantry-registry) with `.freezer` lockfiles

---

## Why Home?

- **Fast** — lightning-quick compile times with aggressive IR caching and parallel builds
- **Safe** — memory safety without manual management or complex lifetimes
- **Joyful** — TypeScript-inspired syntax that feels natural and familiar
- **Batteries included** — HTTP server, database access, queues, and async runtime in the stdlib
- **Modern tooling** — built-in package management, formatter, LSP, and test runner
- **Cross-platform UI** — native desktop and mobile apps alongside your web apps

Language highlights: pattern matching • generics • comptime evaluation •
async/await • `Result` types with `?` propagation • null-safety operators
(`?.`, `?:`, `??`, `?[]`) • power (`**`) and integer-division (`~/`) operators.

---

## TypeScript, Node & Bun compatibility

A large share of the work happens here: Home ships its own TypeScript
compiler, language server, and a Bun runtime port — measured against upstream
baselines rather than self-reported.

| Area | Status |
|---|---|
| TypeScript conformance corpus (coarse) | **5,907 / 5,907 — 100%** |
| TypeScript conformance (byte-for-byte exact) | **~82.5%**, ratcheting weekly |
| `TSxxxx` diagnostic codes emitted | **1,620 / 2,079** — the *reachable* subset is complete |
| LSP wire methods (`home-lsp` vs `tsserver`) | **76 / ~80 — ~95%** |
| Bun runtime files integrated | **552 / 1,193 — ~46%** |
| `node:*` modules JS-callable | **24 / 47** |

Every number is a file-count, row-count, or byte-for-byte measurement against
an external baseline — see the
**[full parity tables](https://github.com/home-lang/home#parity-status)** and the
per-feature drill-downs for
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

# Run your first program
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
| **[home](https://github.com/home-lang/home)** | The compiler, standard library, TypeScript frontend, and runtime |
| **[craft](https://github.com/home-lang/craft)** | Performant, native desktop & mobile apps with web languages |
| **[pantry-registry](https://github.com/home-lang/pantry-registry)** | A performant, universal package manager and registry |
| **[home-os](https://github.com/home-lang/home-os)** | A safe, performant & modern operating system |
| **[generals](https://github.com/home-lang/generals)** | Command & Conquer: Generals — Zero Hour, written in Home |
| **[flappy-bird](https://github.com/home-lang/flappy-bird)** • **[ballerburg](https://github.com/home-lang/ballerburg)** | Games built to stress the language |

---

## Status & Contributing

Home is under **active development**. The lexer, parser, type inference, and
interpreter are usable today; native codegen, tooling, the TypeScript frontend,
and the Bun-compatible runtime are still maturing. The
[capability matrix](https://github.com/home-lang/home/blob/main/docs/CAPABILITY_MATRIX.md)
is deliberately conservative — anything not exercised by an example or test
stays marked in-progress.

Contributions are welcome. Start with
[CONTRIBUTING.md](https://github.com/home-lang/home/blob/main/.github/CONTRIBUTING.md),
or say hi on [Discord](https://discord.gg/home-lang).

---

<div align="center">

**Built with ❤️ by the Home community** — MIT licensed

⭐ **[Star us on GitHub](https://github.com/home-lang/home)** to support the project!

</div>

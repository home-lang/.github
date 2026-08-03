<p align="center"><img src="https://github.com/home-lang/home/blob/main/.github/art/banner.jpg?raw=true" alt="Home Programming Language"></p>

<div align="center">

**There is no language like Home**

### The lightweight & performant JavaScript & TypeScript engine

*Run TS directly. Real threads over a shared heap. No GIL. One binary.*

**[Compiler](https://github.com/home-lang/home)** • **[Engine](https://github.com/zig-utils/zig-js)** • **[Docs](https://github.com/home-lang/home/tree/main/docs)** • **[Parity status](https://github.com/home-lang/home#parity-status)** • **[Discord](https://discord.gg/home-lang)**

</div>

---

## What is Home?

Home runs **JavaScript and TypeScript** on its own engine — written from
scratch in Zig, not a fork of V8 or JavaScriptCore. TypeScript executes
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

## Actually multithreaded JavaScript

Every mainstream JS runtime gives you *isolation* and calls it threading:
workers with separate heaps, talking through serialized messages. Home's
engine does that too — and then goes considerably further.

**Two thread models, both real:**

| Model | What's shared | What crosses the boundary |
|---|---|---|
| **Agent / worker isolation** | Nothing — each OS thread owns its context, globals, and heap | Structured-clone bytes, `SharedArrayBuffer` |
| **Shared-realm `Thread`** | One heap, one global object, one shape tree, **shared object identity** | Ordinary function arguments and return values |

That second row is the one nobody else offers. Spawned `Thread`s execute
JavaScript **concurrently on real OS threads over a single garbage-collected
heap — with no global interpreter lock**, in parallel by default:

```js
const box = { n: 0 }

const t = new Thread((shared) => {
  shared.n += 1        // the *same* object — no postMessage, no clone
  return shared
}, box)

t.join() === box       // true: object identity survives the thread boundary
```

The concurrency toolkit is first-class JavaScript: `Thread`, `Lock`,
`Condition`, `ThreadLocal`, `ConcurrentAccessError`, property-mode `Atomics.*`,
and proposal-aligned `Atomics.Mutex` / `Atomics.Condition`. A deterministic
GIL mode stays available for when you want serialized interleavings.

**What it measures** (eight lanes, versus JavaScriptCore):

| Mode | Throughput vs JSC | Scaling |
|---|---:|---:|
| Direct warmed context (1 lane) | **2.32×** | — |
| Independent steady contexts (8 lanes) | **2.53×** | **5.35×** (JSC: 4.96×) |
| **Shared realm, no GIL (8 lanes)** | *no public JSC equivalent* | **4.84×** |

WebAssembly threads come along too: complete atomic opcode execution, shared
memory, and `wait`/`notify` — **17.23 M/s** contended adds and **287,444**
wait/notify handoffs per second at eight workers.

Underneath sits a concurrent, generational GC with write barriers, per-structure
locks, and precise frame roots — the whole surface gated in CI by
ThreadSanitizer, fuzzers, and a parallel test262 corpus.

The toolchain is parallel too: type-checking and compilation fan out one worker
thread per core off an atomic cursor, regression-tested to produce output
identical to a serial run.

> **Engine migration in flight:** the parallel engine is
> [zig-js](https://github.com/zig-utils/zig-js), which powers Home's repository
> tooling today. The Bun-parity production runtime still links vendored
> JavaScriptCore while the private ABI surface is ported —
> [tracked here](https://github.com/zig-utils/zig-js/blob/main/docs/HOME_INTEGRATION.md).

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

- **Truly parallel JS** — real threads over a shared heap, no GIL, no `postMessage` tax
- **Zero dependencies** — one binary is the runtime, compiler, LSP, bundler, package manager, and test runner
- **TypeScript is native** — executed directly, not transpiled by a separate tool first
- **Fast** — 2.3–2.5× JavaScriptCore throughput, plus parallel compilation with aggressive IR caching
- **Safe** — memory safety without manual management or complex lifetimes
- **Joyful** — TypeScript-inspired syntax, with an escape hatch when you need more
- **Cross-platform UI** — native desktop and mobile apps via [Craft](https://github.com/home-lang/craft)

---

## Compatibility, measured

Home is built against upstream baselines, not self-reported checklists.
Every number below is a file-count, row-count, or byte-for-byte comparison.

| Area | Status |
|---|---|
| **test262** (engine conformance) | **53,175 / 53,175** |
| **WebAssembly** (ten-profile matrix) | **151,802 / 151,802** applicable |
| TypeScript conformance corpus (coarse) | **5,907 / 5,907 — 100%** |
| TypeScript conformance (byte-for-byte exact) | **~82.5%**, ratcheting weekly |
| `TSxxxx` diagnostic codes emitted | **1,620 / 2,079** — the *reachable* subset is complete |
| LSP wire methods (`home-lsp` vs `tsserver`) | **76 / ~80 — ~95%** |
| Bun runtime files integrated | **552 / 1,193 — ~46%** |
| `node:*` modules JS-callable | **24 / 47** |

See the **[full parity tables](https://github.com/home-lang/home#parity-status)**
and the per-feature drill-downs for
[TypeScript](https://github.com/home-lang/home/blob/main/docs/PARITY-TYPESCRIPT.md),
[Node](https://github.com/home-lang/home/blob/main/docs/PARITY-NODE.md),
[Bun](https://github.com/home-lang/home/blob/main/docs/PARITY-BUN.md), and the
[engine's thread model](https://github.com/zig-utils/zig-js/blob/main/docs/threads/index.md).

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
| **[zig-js](https://github.com/zig-utils/zig-js)** | The JavaScript engine — pure Zig, parallel, no GIL |
| **[craft](https://github.com/home-lang/craft)** | Performant, native desktop & mobile apps with web languages |
| **[pantry-registry](https://github.com/home-lang/pantry-registry)** | A performant, universal package manager and registry |
| **[home-os](https://github.com/home-lang/home-os)** | A safe, performant & modern operating system |
| **[generals](https://github.com/home-lang/generals)** | Command & Conquer: Generals — Zero Hour, written in Home |
| **[flappy-bird](https://github.com/home-lang/flappy-bird)** • **[ballerburg](https://github.com/home-lang/ballerburg)** | Games built to stress the language |

---

## Status & Contributing

Home is under **active development**. The lexer, parser, type inference, and
interpreter are usable today; native codegen, tooling, and the runtime are
still maturing, and the engine migration described above is in progress. The
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

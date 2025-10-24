<div align="center">

# The Home Programming Language

**A systems language that doesn't compromise**

*There is no language like Home. The speed of Zig. The safety of Rust. The joy of TypeScript.*

**[home-lang.org](https://home-lang.org)** • **[Docs](https://home-lang.org)** • **[GitHub Discussions](https://github.com/home-lang/home/discussions)** • **[Discord](https://discord.gg/home-lang)**

</div>

---

## What is Home?

Home is a modern programming language for systems and application development that delivers fast compile times, memory safety without ceremony, and APIs that spark joy. [Crafting](https://github.com/home-lang/craft) cross-platform apps has never been easier.

```home
import std/http { Server }

fn main() {
  let server = Server.bind(":3000")

  server.get("/", fn(req) -> async Response {
    return Response.json({ message: "Welcome Home! 🏡" })
  })

  server.listen()
}
```

**File Extensions:** `.home` or `.hm`

**Package Manager:** `pantry/` for dependencies • `.freezer` for lockfiles

**Configuration:** `home.toml` • `couch.toml` • `couch.json(c)`

---

## Why Home?

- **⚡ Fast** - Lightning-quick compile times with aggressive IR caching and parallel builds
- **🔒 Safe** - Memory safety without the ceremony of manual management or complex lifetimes
- **😊 Joyful** - TypeScript-inspired syntax that feels natural and familiar
- **🔋 Batteries Included** - HTTP server, database access, queues, and async runtime in stdlib
- **📦 Modern Tooling** - Built-in package management, formatter, and LSP
- **🎨 Cross-Platform UI** - Build native desktop and mobile apps alongside your web apps

---

## Get Started

```bash
# Clone the compiler
git clone https://github.com/home-lang/home.git
cd home

# Build
zig build

# Run your first program
home run examples/hello.home
```

**[Read the docs →](https://docs.home-lang.org)**

---

<div align="center">

**Built with ❤️ by the Home community**

⭐ **[Star us on GitHub](https://github.com/home-lang/home)** to support the project!

</div>

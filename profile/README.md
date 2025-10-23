<div align="center">

# 🏡 The Home Programming Language

**A systems language that doesn't compromise**

*The speed of Zig. The safety of Rust. The joy of TypeScript.*

**[home-lang.org](https://home-lang.org)** • **[Docs](https://home-lang.org)** • **[Discord](https://discord.gg/home-lang)**

</div>

---

## What is Home?

Home is a modern programming language for systems and application development. We deliver blazing compile times, memory safety without ceremony, and APIs that spark joy.

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

## 🎨 Craft: UI Engine for Home

Build beautiful, native desktop and mobile applications with Home.

**[Craft](https://github.com/home-lang/craft)** is our official UI engine - combining the power of native performance with the simplicity of modern web-like APIs.

```home
import craft { Window, Button, Text }

fn main() {
  let window = Window.create("My App", 800, 600)

  window.render(fn() {
    Text("Welcome to Craft")
    Button("Click me", fn() {
      print("Button clicked!")
    })
  })

  window.show()
}
```

**Learn more:** [github.com/home-lang/craft](https://github.com/home-lang/craft)

---

## ⚡ Core Repositories

- **[home](https://github.com/home-lang/home)** - The Home compiler and standard library
- **[craft](https://github.com/home-lang/craft)** - UI engine for desktop and mobile apps
- **[pantry](https://github.com/home-lang/pantry)** - Official package registry
- **[vscode-home](https://github.com/home-lang/home)** - VSCode extension

---

## 🚀 Why Home?

**⚡ Fast** - 30-50% faster compile times than Zig
**🔒 Safe** - Memory safety without the ceremony
**😊 Joyful** - TypeScript-inspired syntax that feels natural
**🔋 Batteries Included** - HTTP, database, queues in stdlib
**📦 Modern Tooling** - Built-in package manager, formatter, LSP

---

## 🌟 Get Started

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

## 💬 Community

- **[GitHub Discussions](https://github.com/home-lang/home/discussions)** - Questions & ideas
- **[Discord](https://discord.gg/home-lang)** - Real-time chat
- **[Twitter/X](https://twitter.com/homelang)** - Updates & announcements

---

<div align="center">

**Built with ❤️ by the Home community**

⭐ **[Star us on GitHub](https://github.com/home-lang/home)** to support the project!

</div>

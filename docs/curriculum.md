# Rust Learning Curriculum & Tutor Notes

> Persistent roadmap, progress tracker, and session notes for learning Rust from the ground up, building strong SWE practices (Git, testing, CI/CD), and working toward Rust game development / engine work.

## Learner Profile & Preferences
- **Prior experience**: C++ and C# (indie game dev, including Cocos2d-x), Python, Prolog, OCaml (degree).
- **Strengths to leverage**:
  - C++ -> stack vs. heap, RAII, move semantics, templates, zero-cost abstractions.
  - OCaml -> sum types (`enum`), exhaustive pattern matching, `Option`/`Result`, immutability by default, expression-oriented style, iterators/higher-order functions.
  - C# / game dev -> interfaces (`trait`s), game loops, architecture.
- **Key mental shifts to watch for**:
  - Moving from OOP inheritance trees (e.g. Cocos2d-x `Node` hierarchies) to composition, traits, and handle/index-based or ECS data layouts.
  - Letting the borrow checker guide data ownership rather than fighting it with `Rc<RefCell<T>>` everywhere.
- **Workflow preferences**:
  - **Pacing**: Go slow, **one concept / one small step at a time**. Always explain *what* each file, config field, and command is for and *why* it exists before asking the learner to run or write it. Do not batch multiple setup or coding steps into a single turn.
  - Learner runs installation/CLI commands and writes the code themselves.
  - Tutor explains concepts, designs exercises, reviews code (PR-style), and guides debugging.
  - Uses **GitHub** for CI/CD and multi-device syncing.
  - Wants **multiple small mini-projects** leading up to larger capstones, including non-game-dev projects that teach valuable SWE/Rust skills.

---

## Progress Tracker

- [ ] **Phase 0: Workspace, Cargo & CI Foundations**
  - [x] 0.1 — Cargo Workspace setup, `.gitignore`, initial GitHub push
  - [ ] 0.2 — GitHub Actions CI (`fmt`, `clippy`, `test`)
- [ ] **Phase 1: Core Rust Foundations**
  - [ ] 1.1 — Mini-Project 1: `dice-cli` (syntax, structs/enums, `match`, `Option`/`Result`, unit tests)
  - [ ] 1.2 — Mini-Project 2: `text-stats` (ownership, borrowing, slices, lifetimes, iterators, doc-tests)
  - [ ] 1.3 — Mini-Project 3: `cfg-check` (custom errors, `?`, `thiserror`/`anyhow`, `serde`, `clap`, `lib.rs` vs `main.rs`)
  - [ ] 1.4 — Mini-Project 4: `arena-sim` (traits, generics, static vs dynamic dispatch, deterministic RNG testing)
  - [ ] 1.5 — Phase 1 Capstone: `term-rogue` (terminal roguelite, handle-based entities, save/load, TUI)
- [ ] **Phase 2: Concurrency, Networking & Production SWE**
  - [ ] 2.1 — Mini-Project 5: `asset-packer` (threads, `Send`/`Sync`, channels, `rayon`, `criterion` benchmarks, CD release workflow)
  - [ ] 2.2 — Mini-Project 6: `score-api` (async `tokio`, `axum` HTTP API, SQLite, `tracing`, integration tests in CI)
- [ ] **Phase 3: Game Architecture & Graphics**
  - [ ] 3.1 — Mini-Project 7: `mini-ecs` (generational arenas / handles, sparse sets, library API design)
  - [ ] 3.2 — Phase 3 Capstone: 2D Graphics Prototype (`macroquad`/`bevy` or `winit` + `wgpu` custom renderer) -> Cocos2d-x port / engine planning

---

## Detailed Lesson Plans

### Phase 0: Workspace, Cargo & CI Foundations
- **0.1 Repo & Cargo Workspace Setup**
  - Structure the repo as a **Cargo Workspace** (`Cargo.toml` at root with `members = ["projects/*"]`) so all mini-projects live cleanly under `projects/`, share one `Cargo.lock` and `target/`, and are tested together in CI.
  - Clean up the empty `main.rs`, configure `.gitignore`, write a short `README.md`, and push to GitHub.
- **0.2 GitHub Actions CI**
  - Create `.github/workflows/ci.yml` running:
    1. `cargo fmt --all -- --check`
    2. `cargo clippy --workspace --all-targets -- -D warnings`
    3. `cargo test --workspace`
  - Practice feature branches + PRs (even solo) so CI gates every merge.

### Phase 1: Core Rust Foundations
- **Mini-Project 1 — Dice & Probability Roller (`projects/dice-cli`)**
  - Parse dice notation like `3d6+2` or `2d20kh1` from CLI args, roll them, and print results + basic stats.
  - *Skills*: bindings/mutability, expressions, `struct`, `enum`, `match`, `Option`/`Result`, `#[cfg(test)]` unit tests.
- **Mini-Project 2 — Zero-Copy Text Analyzer (`projects/text-stats`)**
  - Read a text/markdown file and compute word frequencies, top-N words, and line/section summaries without unnecessary cloning.
  - *Skills*: ownership, move vs `Copy`, `&T`/`&mut T`, `&str` vs `String`, explicit lifetimes `'a`, iterators & closures, `HashMap::entry`, doc-tests (`///`), integration tests (`tests/`).
- **Mini-Project 3 — Game Config / Save-File Linter (`projects/cfg-check`)**
  - Validate JSON/TOML config files against domain rules (e.g. item/level definitions) and report structured diagnostics.
  - *Skills*: `lib.rs` + `main.rs` split, custom error enums, `From`, `?`, `thiserror` (lib) vs `anyhow` (bin), `serde`, `clap`.
- **Mini-Project 4 — Turn-Based Arena Battler (`projects/arena-sim`)**
  - Two teams of fighters with different abilities/behaviors battle automatically in a deterministic simulation.
  - *Skills*: `trait`s, generics, `impl Trait` vs `Box<dyn Trait>`, composition over inheritance, seeded RNG (`rand`) for reproducible tests, module trees.
- **Phase 1 Capstone — Terminal Roguelite (`projects/term-rogue`)**
  - Grid dungeon, procedural rooms/corridors, player movement, enemies, combat, items, save/load, terminal UI (`crossterm` or `ratatui`).
  - *Skills*: combining all Phase 1 concepts + introducing **index/handle-based entity storage** to avoid borrow-checker tangles.

### Phase 2: Concurrency, Networking & Production SWE
- **Mini-Project 5 — Multi-Threaded Asset Packer (`projects/asset-packer`)**
  - Batch-process a directory of files/images (e.g., resizing, hashing, building a manifest/atlas) in parallel.
  - *Skills*: `std::thread`, `Send`/`Sync`, `mpsc` channels, `Arc<Mutex<T>>`, `rayon`, benchmarking with `criterion`, and **CD** via GitHub Actions (automated tagged release binaries).
- **Mini-Project 6 — Game Leaderboard HTTP API (`projects/score-api`)**
  - Small REST API for submitting and querying high scores per game/level, backed by SQLite.
  - *Skills*: `async`/`await`, `tokio`, `axum`, `sqlx` (or `rusqlite`), `tracing` structured logs, DB migrations, automated API integration tests in CI, and **multi-stage Dockerfiles** for Rust services.

### Phase 3: Game Architecture & Graphics
- **Mini-Project 7 — Generational Arena / Mini-ECS (`projects/mini-ecs`)**
  - Build a small reusable library crate implementing generational handles and component storage.
  - *Skills*: data-oriented design in Rust, interior mutability trade-offs, public API ergonomics, benchmarks.
- **Phase 3 Capstone — 2D Graphics & Engine Direction**
  - Hands-on comparison of building on an engine (`bevy` / `macroquad`) vs. writing a minimal 2D sprite renderer (`winit` + `wgpu`), setting up the real Cocos2d-x port or custom engine project.

---

## Session Log
- **2026-11-25**: Initialized curriculum. Learner installed `rustup` via `apt` and set `stable` toolchain. Next up: **Lesson 0.1 (Cargo Workspace & GitHub setup)**.

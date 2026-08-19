<p align="center">
  <img src="assets/banner.jpg" alt="Nandishwar Singh — compilers, databases, systems, AI tooling" width="100%">
</p>

<p align="center">
  <a href="https://github.com/NandishwarSingh?tab=repositories"><b>Projects</b></a>
  &nbsp;·&nbsp;
  <a href="https://www.linkedin.com/in/nandishwar-singh-00b7602b0"><b>LinkedIn</b></a>
  &nbsp;·&nbsp;
  <a href="https://x.com/SocialRadish"><b>X</b></a>
</p>

---

I build the layers most people treat as a black box — compilers, storage engines,
sync spines, video pipelines — and then use them to ship things I actually run.

MCA student at **VIT Vellore**. Right now that means a systems language with its own
embedded backend, an LSM-tree database in C, and a self-hosted personal cloud that
handles my notes, files, mail and voice.

<br>

## Building

| Project | What it is |
| :-- | :-- |
| **[Simple](https://github.com/NandishwarSingh/simple-lang)**<br><sub>`C++` `QBE` `arm64 · x86_64 · rv64`</sub> | A small systems language that compiles to real native executables through an **embedded QBE backend** — one binary, no VM, no GC. ARC frees heap values the instant their last user is done; `spawn` + channels make data races *unwritable* because threads never share memory. Always-on inliner, const-fold/DCE and auto-vectorizer. **Ties or beats C on 6 of 14 benchmarks** in its own perf lab, verified on macOS and Linux, clang and gcc, arm64 and x86_64. |
| **[nDB](https://github.com/NandishwarSingh/nDB)**<br><sub>`C`</sub> | A high-performance **LSM-tree storage engine**: memtable + SSTables, write-ahead log with selectable durability modes, bloom filters for fast negative lookups, non-blocking background compaction, a multi-threaded TCP server on a binary protocol, a SQL front end, chunked direct-to-disk blob storage, and p50/p95/p99/p999 latency metrics served from inside the engine. |
| **[badal](https://github.com/NandishwarSingh/badal)**<br><sub>`Go` `Svelte` `Podman`</sub> | My **personal cloud**, self-hosted across a VPS and a GPU node. Content-defined chunking over BLAKE3 blobs, a `change_log` sync spine with passkeys and device tokens, an offline-first PWA with inline conflict resolution, IMAP ingest that turns mail into calendar events *with verbatim evidence*, hybrid semantic search with RRF and reranking, and voice in/out (Whisper + Kokoro) that never leaves my own hardware. |
| **[Helm](https://github.com/NandishwarSingh/helm)**<br><sub>`TypeScript` `Next.js` `Postgres`</sub> | A **keyboard-first command center** for Gmail and Google Calendar, built on [Corsair](https://corsair.dev). Multi-tenant *and* multi-account, strict cache-read/live-write split, realtime push for both Gmail (Pub/Sub) and Calendar (`events.watch`), optimistic edits with a 7-second undo. [▶ demo](https://www.youtube.com/watch?v=RC5qz3lX104) |
| **[Chitra](https://github.com/NandishwarSingh/chitra-video-editor)**<br><sub>`TypeScript` `Rust` `WebGPU`</sub> | A **browser-native AI video editor**. WebGPU preview compositor, FFmpeg-in-a-worker export whose text math is shared with the preview so the two can't drift, chat-driven editing through a custom DSL (EAL), local speech-to-text, beat detection and SAM2/EfficientTAM rotoscoping. No clip ever has to leave the machine. |
| **[GeoPolitiq](https://github.com/NandishwarSingh/GeoPolitiq)**<br><sub>`JavaScript` `MongoDB`</sub> | A **geopolitics intelligence platform** — scheduled AI generation with authenticity verification before publish, region-targeted web push, an auto-linked tag graph with paginated archives, and a newspaper-style reading experience. |

<br>

## Merged upstream

| | Pull request | Where |
| :-- | :-- | :-- |
| `2026-07` | [feat(datadog): add Datadog integration plugin](https://github.com/corsairdev/corsair/pull/457) | **corsairdev/corsair** — the agent integration layer |
| `2026-07` | [fix(gmail): reliably sync messages from webhook pushes](https://github.com/corsairdev/corsair/pull/450) | **corsairdev/corsair** |
| `2026-06` | [feat(examples): add LangGraph multi-agent chat example](https://github.com/thesysdev/openui/pull/644) | **thesysdev/openui** — the open standard for generative UI |
| `2026-05` | [Fix `Too many open files` during proxy validation](https://github.com/X3r0Day/ProxyToolkit/pull/1) | **X3r0Day/ProxyToolkit** |
| `2026-04` | [Implement `Debug` for `PlainEditor` and its dependent types](https://github.com/linebender/parley/pull/615) | **linebender/parley** — Rust rich text layout |

<br>

## Stack

```
systems    C · C++ · Rust · Go · Zig · Assembly · QBE · LLVM-free codegen
web        TypeScript · React · Next.js · Svelte · Node · tRPC
data       Postgres · MongoDB · LSM-trees · S3 / Garage · BLAKE3
infra      Podman · Caddy · Tailscale · SELinux · Pub/Sub webhooks
media      WebGPU · FFmpeg · Whisper · SAM2 / EfficientTAM
```

<br>

---

<p align="center">
  <sub>Always building. Always shipping.</sub>
</p>

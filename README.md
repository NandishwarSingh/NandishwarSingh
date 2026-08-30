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

Your Markdown has broken escaping and duplicated link syntax. Here’s the cleaned-up version:

| Project                                                                                             | What it is                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[badal](https://github.com/NandishwarSingh/badal)**<br><sub>`Go` `Svelte` `Podman`</sub>          | My **personal cloud**, self-hosted across a VPS and a GPU node. Content-defined chunking over BLAKE3 blobs, a `change_log` sync spine with passkeys and device tokens, an offline-first PWA with inline conflict resolution, IMAP ingest that turns mail into calendar events *with verbatim evidence*, hybrid semantic search with RRF and reranking, and voice in/out (Whisper + Kokoro) that never leaves my own hardware.                                                                                                                                                                                                                                                                                  |
| **[Helm](https://github.com/NandishwarSingh/helm)**<br><sub>`TypeScript` `Next.js` `Postgres`</sub> | A **keyboard-first command center** for Gmail and Google Calendar, built on [Corsair](https://corsair.dev). Multi-tenant *and* multi-account, strict cache-read/live-write split, realtime push for both Gmail (Pub/Sub) and Calendar (`events.watch`), optimistic edits with a 7-second undo. [▶ Demo](https://www.youtube.com/watch?v=RC5qz3lX104)                                                                                                                                                                                                                                                                                                                                                           |
| **FPGA display-path accelerator**<br><sub>`Verilog` `cocotb` `Verilator` · design stage</sub>       | An FPGA that sits between a handheld's SoC and its panel: render at 360p, reconstruct on the way out. The real work is the accelerator's own memory subsystem — DDR3 front end with a row-hit-aware scheduler, QoS arbiter (scanout hard real-time, flow estimation best-effort), and line-buffer reuse. Verified the way RTL has to be: a golden model the RTL must match bit-exact, cocotb testbenches on Verilator running headless in CI, constrained-random testing with functional coverage, formal verification on the arbiter, and synthesis in the loop so timing and LUT budgets fail CI instead of surprising me. No RTL yet — the design log came first, and it killed most of my own assumptions. |

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
systems    C · C++ · Go · Assembly · QBE · LLVM-free codegen
web        TypeScript · React · Next.js · Svelte · Node · tRPC
data       Postgres · MongoDB · LSM-trees · S3 / Garage · BLAKE3
infra      Podman · Caddy · Tailscale · SELinux · Pub/Sub webhooks
media      WebGPU · FFmpeg · Whisper · SAM2 / EfficientTAM
silicon    Verilog · SystemVerilog · cocotb · Verilator · SVA + formal   ← learning
```

<br>

---

<p align="center">
  <sub>Always building. Always shipping.</sub>
</p>

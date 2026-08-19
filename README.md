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

## Going down a layer — FPGA & digital design

I got to compilers by refusing to treat codegen as magic, so the same instinct
pointed at the hardware underneath it. I'm working up the ladder deliberately:
combinational and sequential logic → a register file → a fetch-decode-execute
datapath → HDL → real silicon on an FPGA → pipelining, hazards and caches →
SIMT and the memory hierarchy a GPU actually needs.

**The design in progress** is an FPGA sitting in the *display path* of a handheld —
it takes low-resolution frames over HDMI and reconstructs them, so the SoC can
render at 360p and the panel still gets something worth looking at. Writing the
design log first killed most of my own assumptions: a display-path tap yields
post-composite RGB with no motion vectors or depth, an external FPGA can't sit in
an x86 page-table walk (~1–10 ns on-die vs ~500–2000 ns over PCIe), and frame
interpolation costs a full frame of latency *by construction*. What survived is
sharper than what I started with — the real work is the accelerator's own memory
subsystem: a DDR3 front end with a bank/row-hit-aware scheduler, a multi-master
arbiter with QoS (scanout is hard real-time, flow estimation is best-effort),
tiled vs raster access ordering, line-buffer reuse, DMA descriptor engines, and
tear-free double buffering. Which is, more or less, a GPU memory subsystem.

The numbers set the constraints: a streaming 4-tap vertical scaler costs
**0.185 ms** and ~7.7 KB of BRAM at zero DRAM bandwidth; 720p60 frame generation
wants **1.33 GB/s** against ~2.1 GB/s usable on 16-bit DDR3-1600; naive full-search
block matching is **60 Gop/s** and infeasible, but a 3-level pyramid brings it to
1.5–3 Gop/s. The thesis is falsifiable on purpose: not "beat native rendering,"
but *beat the panel driver's built-in bilinear scaler at ~zero added latency* —
scored with SSIM/LPIPS against a native-res reference.

### Verification, because RTL is where "it compiles" means nothing

You can't `printf` a timing violation, and a bug that reaches a bitstream costs
hours instead of seconds. So the testbench is the deliverable, not an afterthought:

- **A golden model first, RTL second.** Every block gets a C or Python reference
  implementation. The RTL isn't done when it runs — it's done when it matches the
  model bit-exact over the whole stimulus set.
- **[cocotb](https://www.cocotb.org) on [Verilator](https://www.veripool.org/verilator/)** so testbenches are Python and run headless in CI.
  Self-checking, no human staring at a waveform to decide whether it passed.
- **Constrained-random + functional coverage,** not a directed test per bug.
  Directed tests only find the bugs you already thought of; coverage tells you which
  corners of the state space were never reached.
- **Assertions on every interface** — stream handshakes hold, no beat is dropped,
  backpressure is honoured, FIFOs never overflow or read empty.
- **Formal property checking** on the arbiter and FIFO control paths, where random
  simulation is weakest and a deadlock hides behind an unlikely interleaving.
- **Synthesis in the loop.** Every push re-runs lint → sim → coverage → synth, so
  LUT/BRAM/DSP usage and timing closure are regressions that fail CI, not surprises
  discovered the week the board arrives.
- **Image quality as a numeric gate.** SSIM/LPIPS against the reference runs in the
  same pipeline — "looks better" becomes a number CI can fail on. Waveforms are
  dumped as artifacts only when something breaks.

<sub>Status: the design log and the analysis above are real; the RTL isn't written yet.
Hardware for the first track is a Tang Nano 20K and a TFP401 breakout — deliberately
the cheap board, because committing to a big one before you know your LUT and bandwidth
budget is just guessing with money.</sub>

<br>

## Stack

```
systems    C · C++ · Rust · Go · Zig · Assembly · QBE · LLVM-free codegen
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

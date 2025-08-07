# SpiderMonkey Benchmarking

This repository is focused on benchmarking and analyzing **SpiderMonkey’s JIT compilation** behavior (specifically IonMonkey), with tools, scripts, and configuration for fine-grained profiling, instrumentation, and visualization of JavaScript execution.

---

## 📁 Project Structure

- `benchmark/`: Contains the benchmarking scripts, typically performance micro-benchmarks like `prop_access.js`.
- `benchmarkers/`: Helper scripts/tools to run benchmarks and log results.
- `.gitignore`: Excludes large binaries and logs from Git tracking.
- `temp_large_files/`: (Ignored) For storing large binaries or artifacts locally without pushing to GitHub.

---

## 🧪 Benchmarking Setup

### Prerequisites

- Built version of SpiderMonkey from `mozilla-central`
- The `js` shell executable, located in:
```

mozilla-central/obj-x86\_64-pc-linux-gnu/dist/bin/js

````

### Enable JIT Compilation and Logging

Use the `--ion-eager` flag to force IonMonkey to compile functions eagerly.

```bash
cd mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin
export IONFLAGS=logs  # or use detailed flags like: aborts,mir,compilation,info
./js --ion-eager path/to/prop_access.js
````

---

## ⚙️ Usage Counters Instrumentation

Instrumentation was added using:

```cpp
cx->runtime()->setUseCounter(cx->global(), JSUseCounter::DATEPARSE_IMPL_DEF);
```

Relevant modified files:

* `js/xpconnect/src/XPCJSRuntime.cpp`
* `js/public/friend/UsageStatistics.h`
* `dom/base/UseCounters.conf`

This is helpful for gathering usage statistics on specific JavaScript features.

---

## 📈 Exploring Optimizations with `iongraph`

### Install `iongraph` (if not available):

IonGraph helps visualize IonMonkey's internal MIR/LIR graphs.

```bash
# From the root of mozilla-central
./mach bootstrap  # Ensure dependencies are installed
./mach build
```

### Example Usage:

```bash
iongraph js --format png -- --ion-eager ./Benchmarkers/prop_access.js
```

#### Additional Options

| Flag                             | Description                                 |
| -------------------------------- | ------------------------------------------- |
| `--format {gv,pdf,pdfs,png,svg}` | Output format for graphs                    |
| `-f FUNCNUM`                     | Dump graph for specific function number     |
| `-n FUNCNAME`                    | Dump graph for specific function name       |
| `-p PASSNUM`                     | Dump graph for a specific optimization pass |
| `--final`                        | Show final optimized graph                  |

### Useful `IONFLAGS` values

```bash
export IONFLAGS=caches,loadkeys,stubfolding,logs-sync
```

This will output data about:

* Inline caches
* Property key accesses
* CacheIR stub folding
* Graph logs synced after each function

---

## 🔬 Experiments to Try

* Run benchmarks with different object shapes.
* Observe how object key order affects optimization and caching.
* Compare initial vs final MIR graphs using `--final`.

---

## 📝 Notes

This repo excludes binaries (`js`, `libmozjs`, etc.) due to GitHub file size limits. Ensure these are built locally or stored in `temp_large_files/`.

For any issues with shallow clone limitations, consider working from a full clone or exporting only benchmarking-related files.

---

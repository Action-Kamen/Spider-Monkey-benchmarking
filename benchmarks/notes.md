cx->runtime()->setUseCounter(cx->global(), JSUseCounter::DATEPARSE_IMPL_DEF);

mozilla-central/js/xpconnect/src/XPCJSRuntime.cpp
mozilla-central/js/public/friend/UsageStatistics.h
mozilla-central/dom/base/UseCounters.conf

anirudh@anirudh-ROG-Zephyrus-G15-GA503RM-GA503RM:~/Desktop/CS485/mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin$ export IONFLAGS=aborts,mir,compilation,info,log
anirudh@anirudh-ROG-Zephyrus-G15-GA503RM-GA503RM:~/Desktop/CS485/mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin$ ./js --ion-eager ../../../../Benchmarkers/prop_access.js
found tag: aborts
found tag: mir
found tag: compilation
Unknown flag.

anirudh@anirudh-ROG-Zephyrus-G15-GA503RM-GA503RM:~/Desktop/CS485$ cd ./mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin
anirudh@anirudh-ROG-Zephyrus-G15-GA503RM-GA503RM:~/Desktop/CS485/mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin$ export IONFLAGS=logs
anirudh@anirudh-ROG-Zephyrus-G15-GA503RM-GA503RM:~/Desktop/CS485/mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin$ ./js --ion-eager ../../../../Benchmarkers/prop_access.js

usage: IONFLAGS=option,option,option,... where options can be:

  aborts        Compilation abort messages
  scripts       Compiled scripts
  mir           MIR information
  prune         Prune unused branches
  escape        Escape analysis
  alias         Alias analysis
  alias-sum     Alias analysis: shows summaries for every block
  gvn           Global Value Numbering
  licm          Loop invariant code motion
  flac          Fold linear arithmetic constants
  eaa           Effective address analysis
  sink          Sink transformation
  regalloc      Register allocation
  inline        Inlining
  snapshots     Snapshot information
  codegen       Native code generation
  bailouts      Bailouts
  caches        Inline caches
  osi           Invalidation
  safepoints    Safepoints
  pools         Literal Pools (ARM only for now)
  cacheflush    Instruction Cache flushes (ARM only for now)
  range         Range Analysis
  branch-hint   Wasm Branch Hinting
  wasmbce       Wasm Bounds Check Elimination
  shapeguards   Redundant shape guard elimination
  gcbarriers    Redundant GC barrier elimination
  loadkeys      Loads used as property keys
  stubfolding   CacheIR stub folding
  logs          JSON visualization logging
  logs-sync     Same as logs, but flushes between each pass (sync. compiled functions only).
  profiling     Profiling-related information
  dump-mir-expr Dump the MIR expressions
  warp-snapshots WarpSnapshots created by WarpOracle
  warp-transpiler Warp CacheIR transpiler
  warp-trial-inlining Trial inlining for Warp
  all           Everything

  bl-aborts     Baseline compiler abort messages
  bl-scripts    Baseline script-compilation
  bl-op         Baseline compiler detailed op-specific messages
  bl-ic         Baseline inline-cache messages
  bl-ic-fb      Baseline IC fallback stub messages
  bl-osr        Baseline IC OSR messages
  bl-bails      Baseline bailouts
  bl-dbg-osr    Baseline debug mode on stack recompile messages
  bl-all        All baseline spew

See also SPEW=help for information on the Structured Spewer.
anirudh@anirudh-ROG-Zephyrus-G15-GA503RM-GA503RM:~/Desktop/CS485/mozilla-central/obj-x86_64-pc-linux-gnu/dist/bin$

2. What Else Can You Explore with iongraph

Once your command is working and you can generate the visual graphs, here are additional ways to explore IonMonkey’s optimizations:
A. Explore Different Output Formats

    Output Format Option:
    You can specify the format of the generated graph with the --format option. For example:

    iongraph js --format png -- --ion-eager ./Benchmarkers/prop_access.js

    Valid formats include gv, pdf, pdfs, png, and svg.

B. Select Specific Functions or Passes

    Function Number:
    Use -f FUNCNUM to dump the graph for a specific function number.

    Function Name:
    Use -n FUNCNAME to filter by function name.

    Pass Number:
    Use -p PASSNUM if you want the graph for a particular pass during optimization.

C. Examine Final Optimized Graphs

    Final MIR/LIR Graphs:
    Add the --final flag to dump the final optimized graphs:

    iongraph js --final -- --ion-eager ./Benchmarkers/prop_access.js

    This helps you compare the initial versus final state of optimizations.

D. Investigate Different IONFLAGS

    Alter IONFLAGS for More Information:
    You can enable various debugging flags via the environment variable IONFLAGS. For example:

    export IONFLAGS=caches,loadkeys,stubfolding,logs-sync

    This can give you additional insight into inline caches, how property keys are loaded, and whether CacheIR stub folding is taking place.

E. Compare Graphs for Different Object Shapes

    Benchmark Variants:
    Run your property access benchmark with different object literal orders (e.g., one with a "0" key and one without) and compare the resulting MIR graphs. Look specifically for differences in how property key conversion or inline caching is represented in the graphs.
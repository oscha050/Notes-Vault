## Plan: Fix Recursor PI Lifetime Race

Eliminate the remaining use-after-free (`0xcccccccc`) in process exit/wait synchronization. Current panic stack (`sema_up` -> `list_pop_front`) indicates `wait_sema` inside `struct pi` is being touched after `pi` is freed. The fix is to serialize `process_wait` and child-exit handoff so parent cannot free child `pi` before child finishes signaling.

**Steps**
1. In `process_wait`, hold `children_lock` while deciding whether to wait and while transitioning ownership of the matched child record; do not leave an unlocked window where `info` can be freed before `sema_down`/status read completes.
2. Redesign the matched-child path in `process_wait` so list unlink + `free(info)` happen only after synchronization with child exit is complete and still under safe lock discipline.
3. In `process_cleanup`, make child-to-parent notification atomic with respect to parent wait logic (`exited` flag + wakeup), using the same lock that protects child records in parent list.
4. In `process_cleanup`, keep orphaning logic (`info->parent = NULL`) and child-list cleanup under lock, but ensure no path can free a `struct pi` that another running thread still uses.
5. Add brief lock-order rules in code comments for this subsystem (parent `children_lock` interaction vs child `pi` fields) to prevent reintroducing deadlocks/UAF.
6. Re-run recursor repeatedly and verify kernel no longer faults in `sema_up`/`list_pop_front`; then run a quick regression set of basic `wait/exec` tests.

**Relevant files**
- `/home/oscar/Code/pintos/userprog/process.c` — primary race fix in `process_wait` and `process_cleanup` around `struct pi`, `wait_sema`, `exited`, and parent-child list ownership.
- `/home/oscar/Code/pintos/threads/thread.h` — optional small struct/comment update only if extra state is needed for safe ownership semantics.
- `/home/oscar/Code/pintos/threads/synch.c` — reference only for understanding semaphore behavior while validating race elimination (no functional change expected).

**Verification**
1. Reproduce previous failure workload (deep `recursor-c`) multiple times; confirm no `0xcccccccc` page fault and no panic recursion.
2. Confirm backtrace no longer touches `sema_up` on poisoned memory.
3. Run parent/child lifecycle tests (`exec`, `wait`, double-wait invalid cases) to ensure no deadlock/hang or changed return values.
4. Re-run a small set of basic userprog tests to ensure no regressions in normal syscall paths.

**Decisions**
- Included: process lifecycle synchronization bugfix (`process_wait`/`process_cleanup`) as immediate next fix.
- Excluded for now: broader filesystem syscall global lock and large refactor of process model unless this race fix is insufficient.
- Assumption: current inode-lock changes reduced one race, but remaining crash is in `struct pi` lifetime handoff.

**Further Considerations**
1. If this still flakes, fallback is explicit refcounting of `struct pi` to decouple parent-list ownership from child-thread lifetime.
2. If lock-hold time around `sema_down` is concerning, split lock scope with an intermediate ownership flag rather than raw pointer lifetime assumptions.

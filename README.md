# zed-diags-readme

## Repro steps:

  * Open this folder in Zed Preview (tested it 0.153.1)
  * Open `src/lib.rs`, this should kick r-a initialization
  * Open `README.md` in zed and do something to this rust code block, then save

```rust
fn main() {
    let x = 1;
}

// you could uncomment the next line and save:
// asodfijois
// or do something else
```

Eventually, we see this in `LSP Logs`:

```
stderr: Error: file not found: /Users/amos/zed-diags-readme/README.md
stderr: sending on a disconnected channel
stderr:
stderr: Stack backtrace:
stderr: 0: std::backtrace::Backtrace::create
stderr: 1: anyhow::error::<impl anyhow::Error>::msg
stderr: 2: anyhow::__private::format_err
stderr: 3: rust_analyzer::run_server
stderr: 4: std::sys::backtrace::__rust_begin_short_backtrace
stderr: 5: core::ops::function::FnOnce::call_once{{vtable.shim}}
stderr: 6: std::sys::pal::unix::thread::Thread::new::thread_start
stderr: 7: __pthread_deallocate
```

And r-a is inactive until restarted.

Restarting it _only works_ when `lib.rs` is the active tab:
otherwise, the `editor: restart language server` action
is still present in the command palette, but doesn't
appear do anything.

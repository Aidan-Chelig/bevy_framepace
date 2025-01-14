# bevy_framepace⏱️

[![crates.io](https://img.shields.io/crates/v/bevy_framepace)](https://crates.io/crates/bevy_framepace)
[![docs.rs](https://docs.rs/bevy_framepace/badge.svg)](https://docs.rs/bevy_framepace)
[![CI](https://github.com/aevyrie/bevy_framepace/workflows/CI/badge.svg?branch=main)](https://github.com/aevyrie/bevy_framepace/actions?query=workflow%3A%22CI%22+branch%3Amain)
[![Bevy tracking](https://img.shields.io/badge/Bevy%20tracking-main-lightblue)](https://github.com/bevyengine/bevy/blob/main/docs/plugins_guidelines.md#main-branch-tracking)

### Framepacing and framelimiting for bevy

As simple as adding the plugin to your app:

```rs
app.add_plugin(bevy_framepace::FramepacePlugin::default())
```

You can adjust the framerate limit and framepacing forward estimation safety margin when adding the
plugin, or at runtime by modifying the `FramepacePlugin` resource.

See `demo.rs` in the examples folder, or run with:
```console
cargo run --release --example demo
```

## How it works

![image](https://user-images.githubusercontent.com/2632925/148489293-180b28e2-de49-4450-a1db-221d50b29a00.png)

The plugin works by recording how long it takes to render each frame, it then uses this to estimate how long the next frame will take, and sleeps at the end of the frame until just before it needs to start rendering the next one (red annotation above). Because this system has to estimate how long to sleep for, there is a small safety margin that is subtracted from the sleep time to prevent frame drops, in case the frame takes longer to render than expected. 

A second system then runs right before the frame is presented to the gpu, and sleeps until the desired frametime limit has been reached (blue annotation above). This makes up for any error in forward estimation, and ensures frame time is exactly correct.

The `spin_sleep` dependency is needed for precise sleep times. The sleep function in the standard library is not accurate enough for this application, especially on Windows.

## License

Bevy_framepace is free and open source! All code in this repository is dual-licensed under either:

* MIT License (LICENSE-MIT or http://opensource.org/licenses/MIT)
* Apache License, Version 2.0 (LICENSE-APACHE or http://www.apache.org/licenses/LICENSE-2.0)

at your option. This means you can select the license you prefer! This dual-licensing approach is the de-facto standard in the Rust ecosystem and there are very good reasons to include both.

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.

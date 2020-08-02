# pinephone-cairo-game-starter

This repo is a starter for creating a cairo based game in Rust for PinePhone

If you like this project check out [`mochi`](https://github.com/richardanaya/mochi/), a game engine inspired by this work 

![](screenshots/screenshot1.jpg)
![](screenshots/screenshot0.png)

# Example

```rust
engine::init();

let img_non_touched = engine::image_from_resource("/app/non_touched.png");
let img_touched = engine::image_from_resource("/app/touched.png");

engine::run_game(move |window, ctx, pointer, delta_time| {
    if pointer.is_down() {
      engine::draw_image_centered(ctx,window.width/2.0,window.height/2.0,img_touched);
    } else {
      engine::draw_image_centered(ctx,window.width/2.0,window.height/2.0,img_non_touched);
    }
});
```

## Compilation

Check out commands in my `Makefile` or simply run `make`.

Notice how I compile glade and images into a single `.gresource` file, this let's me embed 
the bytes of all the things my app needs into my Rust app. We rely on the tool `glib-compile-resources` 
putting our game resources in a single binary.

Currently I compile this on my desktop for quick iteration and my phone to test out.

* in debug mode, the app will be about the size of a pinephone
* in release mode, the app will maximize to take up available space (assumed to be running on phone)

When i compile on the phone, I have to increase my amount of ram by uzing zram

```
sudo swapoff /dev/zram0 
sudo zramctl --reset /dev/zram0 
sudo zramctl --find --size 2048M
sudo mkswap /dev/zram0 
sudo swapon /dev/zram0
```

## Setup Rust for cross compilation

I haven't figured this stuff out yet, I could use some help on these details.

1. get gcc setup for aarch64

```
sudo apt-get install gcc-aarch64-linux-gnu
```
```
sudo dnf install gcc-aarch64-linux-gnu
```

2. get rust setup for aarch64

```
rustup install stable-aarch64-unknown-linux-gnu
```

add to `~/.cargo/config`

```
[target.aarch64-unknown-linux-gnu]
linker = "aarch64-linux-gnu-gcc"
```

3. Make sure you have aarch64 dependencies for Gtk

???

4. Start the compile!

```
PKG_CONFIG_ALLOW_CROSS=1 cargo build --target aarch64-unknown-linux-gnu --release
```

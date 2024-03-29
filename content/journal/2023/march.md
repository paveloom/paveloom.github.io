# March 2023

### Thursday, 30 {#30}

#### `obs-teleport`

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/223953) to add [`obs-teleport`](https://github.com/fzwoch/obs-teleport) to Nixpkgs.

### Wednesday, 29 {#29}

#### [PMG](../../git.md#pmg) {#28#pmg}

Made plotting multi-threaded. While I don't have much experience with multi-threading in [Julia](https://julialang.org), spawning [`Task`](https://docs.julialang.org/en/v1/base/parallel/#Core.Task)s turned out to be trivial via the [`@spawn`](https://docs.julialang.org/en/v1/base/multi-threading/#Base.Threads.@spawn) macro. I also learned about [`ReentrantLock`](https://docs.julialang.org/en/v1/base/parallel/#Base.ReentrantLock) and used that to share the standard output between the tasks in a thread-safe manner.

Reduced trait coupling in the [Rust](https://www.rust-lang.org) code. This is a quirk of the trait system that bothers me.

Here's an example:

```rust
impl<F> Object<F>
where
    T: A + B
{
    fn a(&mut self) -> Result<()> {
        // <...>
    }
    fn b(&mut self) -> Result<()> {
        // <...>
    }
}
```

Now everything that uses either of these methods should implement both traits. This scales really badly and leads to weird trait bounds (I had [`SampleUniform`](https://docs.rs/rand/latest/rand/distributions/uniform/trait.SampleUniform.html) being a requirement for using an iterator of a wrapped vector...).

However, you can separate the trait bounds, like so:

```rust
impl<T> Object<T>
{
    fn a(&mut self) -> Result<()>
    where
        T: A,
    {
        // <...>
    }
    fn b(&mut self) -> Result<()>
    where
        T: B,
    {
        // <...>
    }
}
```

And now the problem is solved by excessive copying/pasting.

Also, if you specify the trait bounds in the declaration of an item, you will have to carry around them everywhere. I found it not being worth it for binary crates.

### Monday, 27 {#27}

#### [PMG](../../git.md#pmg) {#27#pmg}

Found the culprit of the errors on the rotation curve: the uncertainties related to the distances should have been inherited from parallax's errors only. However, after fixing it, the uncertainties related to the velocities are still mostly bigger than before. Taking into account that operations done by [automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation) are implicit, it's hard to tell whether it's a regression or an improvement.

Finished with the old requests and now ready to work towards improving the fitting algorithm.

### Sunday, 26 {#26}

#### [PMG](../../git.md#pmg) {#26#pmg}

Finished refactoring. Seems like that fixed weird errors I had on the rotation curve. However, there is no easy way to check the correctness (except, well, by going through the math again).

Implemented some requests, too (e.g., outputting more computed values).

### Saturday, 25 {#25}

#### Identity

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/223048) to add [Identity](https://gitlab.gnome.org/YaLTeR/identity) to Nixpkgs.

The author of the program has implemented the long-awaited [side-by-side comparison mode](https://thisweek.gnome.org/posts/2023/03/twig-88/#identity), which is very useful. You can zoom in/out and drag the image, too!

### Tuesday, 22 {#22}

#### [PMG](../../git.md#pmg) {#22#pmg}

Started refactoring code to (hopefully) make it simpler. The galactocentric distance is stored in one place now instead of two.

Passing so many arguments around is bothering me (same thing I had with Zig). Unfortunately, Rust doesn't have anonymous structs, so I will probably have to create a general struct with optional fields. Perhaps, simplifying the `Object` struct itself to this state would be possible.

#### [Site](../../git.md#site) {#22#site}

Yeah, Codeberg's Woodpecker instance updates the image cache infrequently. I believe I solved the issue with

```
fatal: unable to access 'https://codeberg.org/paveloom/pages.git/': OpenSSL/3.0.8: error:16000069:STORE routines::unregistered scheme
```

by setting the `SSL_CERT_FILE` environment variable to `${pkgs.cacert}/etc/ssl/certs/ca-bundle.crt` in the image. Still can't test it, though!

### Tuesday, 21 {#21}

#### OCI {#21#oci}

Learned how to use [`umoci`](https://umo.ci). Ditched the [`buildContainer`](https://nixos.org/manual/nixpkgs/stable/#ssec-pkgs-ociTools-buildContainer) function in favor of using `umoci` for building an OCI image from scratch. It simplifies stuff since there is no need for a `config.json` file, you just need to put what you want in your file system in a `rootfs` directory, and then `umoci insert` it.

[`dive`](https://github.com/wagoodman/dive) works fine for OCI images, surprisingly. The last update for it was in 2021.

#### [PMG](../../git.md#pmg) {#21#pmg}

Did a couple of stylistic changes in the code.

#### [Site](../../git.md#site) {#21#site}

I switched to using an OCI image for CI/CD. The image is built via Nix.

This way the runners don't require Nix anymore to run the build, and the image contains only required packages.

[Woodpecker](https://ci.codeberg.org/paveloom/pages/pipeline/632/3) doesn't see `/bin/sh`, though, for some reason. Image cache, perhaps?

### Thursday, 16 {#16}

#### OCI {#16#oci}

Tried to build an [OCI](https://opencontainers.org) image for my website today using [`ociTools`](https://nixos.org/manual/nixpkgs/stable/#sec-pkgs-ociTools) in [Nixpkgs](https://github.com/NixOS/nixpkgs). Turns out the [`buildContainer`](https://nixos.org/manual/nixpkgs/stable/#ssec-pkgs-ociTools-buildContainer) function actually creates a [filesystem bundle](https://github.com/opencontainers/runtime-spec/blob/main/bundle.md) instead of an image. Tried to use [`runc`](https://github.com/opencontainers/runc) to run a container from the bundle, but found out that the generated config is [incorrect](https://github.com/NixOS/nixpkgs/pull/130061). I fixed the `realatime` typo locally, but got stuck on trying to implement a working config for a [rootless container](https://github.com/opencontainers/runc/blob/main/README.md#rootless-containers). Might pick up the PR later.

### Tuesday, 14 {#14}

#### Zigmod {#14#zigmod}

Did a bit more work on the [PR](https://github.com/NixOS/nixpkgs/pull/217229) to Nixpkgs.

### Sunday, 12 {#12}

#### `tmux`

Started learning [`tmux`](https://github.com/tmux/tmux) today. I wonder if it's better to outsource some functionality of my Neovim config (e.g., tree-like file system manager plugins) to related external programs (e.g., [`xplr`](https://xplr.dev)). I do that with [Lazygit](https://github.com/jesseduffield/lazygit) already. It's especially useful if I'm ever to switch to [Helix](https://helix-editor.com) which doesn't have a plugin system yet.

### Saturday, 11 {#11}

#### Zig

Turns out you can provide a [custom format function for any user type](https://ziglang.org/documentation/0.10.1/std/#root;fmt.format). I did not know that!

### Friday, 10 {#10}

#### Divolt

[Divolt](https://divolt.xyz) is [down](https://reddit.com/r/Piracy/comments/11mver1/slav_art_dead_again). Was able to download from [Slav Art Web](https://slavart.gamesdrive.net). Tried [DoubleDouble](https://doubledouble.top), too, but it failed a lot.

### Thursday, 9 {#9}

#### Livestreaming {#9#livestreaming}

No stream today, busy with important personal matters.

### Wednesday, 8 {#8}

#### Livestreaming {#8#livestreaming}

Some notes after [today's stream](https://odysee.com/@paveloom-streams:3/pmg-1:b):

- Overall bit rate for streaming the screen turned out to be way lower than streaming Noita yesterday: around 1Mbps. VBR is supposed to be capping the maximum bit rate, but I was missing some frames until I switched from 6Mbps to 5Mbps (I show stats on the recording). So, I wonder if this rate control mode might use more bits when necessary. I'm downloading the replay right now, and it is 1.75 times bigger than the recorded one. Also, since the recording's bit rate is pretty small, as I pointed out yesterday, there might be no reason to use VBR in my use case. However, I would like to try 60 FPS tomorrow.
- Colors seem to be a bit more pale. I wonder if I can fiddle with the color space.
- I might try a slower preset for the recording.
- If I do stick with VBR, I might also try to lower the bit rate and make the buffer size the same or twice as that.
- Might increase the audio bit rate, too.
- I would like to take a look on how it looks on mobile while I'm streaming.

Here are the best resources on rate control modes:

- [CRF Guide (Constant Rate Factor in x264, x265 and libvpx)](https://slhck.info/video/2017/02/24/crf-guide.html)
- [Understanding Rate Control Modes (x264, x265, vpx)](https://slhck.info/video/2017/03/01/rate-control.html)

I've been thinking more about the "public accountability" idea. Here's a simple recap in 3 key points:

1. [Timeboxing](https://en.wikipedia.org/wiki/Timeboxing) (you schedule your work)
2. Public schedule (e.g., announcing livestreams)
3. Public work (e.g., livestreaming)

The main pro, of course, is that you get things done. You might also get fun along the way if you look into creating a community (not me, ironically, but check out [ThePrimeagen](https://www.twitch.tv/ThePrimeagen)). I was also thinking about it being an analogy to traditional work, except, you know... it's on your terms.

#### OBS {#8#obs}

Had problems with adding the [Dropped Frame Alarm](https://obsproject.com/forum/resources/dropped-frame-alarm.675) Lua script for OBS. Looks like Lua's `CPATH` is broken, the correct path to `obslua.so` is appended twice in a single entry.

#### Odysee

I said yesterday that Odysee's front-end is closed-source, but apparently it *is* [open-source](https://github.com/OdyseeTeam/odysee-frontend). And the mobile apps are, too!

A YouTube / Twitch alternative that goes brrr.

#### [PMG](../../git.md#pmg) {#8#pmg}

Updated everything, added a Nix flake. Somehow that took 4 hours... Done on a [stream](https://odysee.com/@paveloom-streams:3/pmg-1:b)!

### Tuesday, 7 {#7}

#### Livestreaming {#7#livestreaming}

I did a [test stream](https://odysee.com/@paveloom-streams:3/test-stream-playing-noita:e) today, playing [Noita](https://noitagame.com). It ended with the game crashing my laptop. That's the first time on the GOG version of the game, while both streaming and recording, but happens all the time on the Steam version, for some reason.

I decided to stick with [Odysee](https://odysee.com) (as opposed to YouTube and Twitch) mostly because I'm planning to play copyrighted music on streams. Never understood why this is considered illegal. It literally harms no one, but helps every artist with exposure. Also, Odysee is based on the [LBRY](https://lbry.com)'s open-source stack underneath the closed-source front-end, the network is peer-to-peer and uncensorable (if someone keeps seeding the content, that is).

Some notes:

- I need something notifying me that I lose frames (I have flaky internet)
- I can put a chat pop-up on top of all windows, but it's somewhat inconvenient for full screen applications (like games) on a single display

Streaming settings:

- Encoder: `x264`
- Rate Control: `VBR`
- Bit rate: 7Mbps, then 6Mbps, then 5Mbps
- CRF: 17
- Key frame interval: 2s
- Preset: `veryfast`
- Profile: High
- Tune: `zerolatency`

Recording settings:

- Encoder: `x264`
- Rate Control: `CRF`
- CRF: 17
- Key frame interval: 0s
- Profile: None
- Tune: None

Forgot to change the profile for the recording settings. Hard to tell about the bit rate for streaming until I download the replay (the live version was lagging for me, but I don't think it's that bad in reality). From what I can see now, 7Mbps was a bit too much, but 6Mbps and 5Mbps were fine (network-wise). The overall bit rate of the recording is way lower, though, almost 4Mbps, so not sure what happened there. This also might be a case for just using `CRF` for screen sharing, and using the same settings for the recording to lower CPU usage.

Also, someone really made sure to leave a slime right after I started streaming...

### Monday, 6 {#6}

#### Livestreaming {#6#livestreaming}

Looked into streaming today. I might actually do it for the sake of the "public accountability" idea. That is, the idea to accompany [timeboxing](https://en.wikipedia.org/wiki/Timeboxing) with the schedule and the work done being public. I fiddled with OBS today, preparing scenes and learning basic shortcuts. That said, it's probably gonna be a simple screen share.

Also looked into [VTubing](https://en.wikipedia.org/wiki/VTuber), but that seems to be somewhat expensive to run and hard to make pretty, and I don't feel like it's gonna add much (I'm not gonna be speaking, after all).

#### OBS {#6#obs}

Created a [PR](https://github.com/NixOS/nixpkgs/pull/219872) to [Nixpkgs](https://github.com/NixOS/nixpkgs) for the [Advanced Scene Switcher](https://obsproject.com/forum/resources/advanced-scene-switcher.395) plugin for [OBS](https://obsproject.com). I wanted to check whether it will allow me to automatically switch scenes depending on which window is focused. Alas, you can't do that on Wayland.

### Sunday, 5 {#5}

#### Counter-Strike: Source

So, my procrastination suggested me to play CSS ZE again. I always thought of this game as of sort of a time capsule, and I saw this again today: it's the same players playing the same maps and joking the same jokes. I still enjoy it, though.

#### Livestreaming {#5#livestreaming}

I've read the `noopkat`'s (Suz Hinton) old article "[Lessons from my first year of live coding on Twitch](https://www.freecodecamp.org/news/lessons-from-my-first-year-of-live-coding-on-twitch-41a32e2f41c1)" today. Livestreaming coding sessions is something I thought of for a while now. In the few articles that I saw, people often give advice on how to make them more entertaining (as far as that's possible for this niche). But what if I would do it just for the sake of keeping myself accountable? Sort of like this journal, but, you know, the next level.

And become a silent coding [VTuber](https://en.wikipedia.org/wiki/VTuber), maybe?!

### Saturday, 4 {#4}

#### [`gnome-shell-memento-mori`](../../git.md#gnome-shell-memento-mori)

I've tested my extension on [GNOME OS Nightly](https://os.gnome.org), [added support](https://extensions.gnome.org/review/39009) for GNOME 44.

I've also switched to pure [NPM](https://www.npmjs.com) from [Bun](https://bun.sh) and added a [Nix flake](https://github.com/paveloom-t/gnome-shell-memento-mori/blob/4ac49ec193dca04509982e4433c9ef0ba3a43b12/flake.nix).

#### Virt-Manager

Learned how to set up [Virtual Machine Manager](https://virt-manager.org) with file sharing between the host and the guest systems on NixOS:

1. Follow the instructions on the [NixOS Wiki](https://nixos.wiki/wiki/Virt-manager).
2. Add the [`virtiofsd`](https://gitlab.com/virtio-fs/virtiofsd) package to user packages.
3. From the virtual hardware details of a machine: `Add Hardware` → `Filesystem`.

    Set driver to `virtiofs`, source path -- to the path on your host machine. Target path is an arbitrary string used as a mount tag (e.g., `host`).

4. Add

    ```xml
    <binary path="/etc/profiles/per-user/$USER/bin/virtiofsd" xattr="on"/>
    ```

    to the XML config of the new Filesystem virtual hardware. Replace `$USER` with your username.

5. In the Guest system: run `sudo mkdir /media/host` and put

    ```
    host /media/host virtiofs rw,users,_netdev 0 0
    ```

    in `/etc/fstab`. Should mount automatically, but you might need to log out and log in again.

### Friday, 3 {#3}

#### Nix

I've been trying to rewrite the Nix expression of this very site to take advantage of fixed-output derivations. I've encountered some issues

- The `.git` directory is pruned from the local source (e.g., `./.`)
- Seems like there is something non-deterministic in [PDM](https://pdm.fming.dev/latest)'s cache
- Timestamps of all files in the store are reset (this breaks latest revision dates)

The first one can be bypassed by fetching the repo from a Git forge. Proper investigation would be required for the second one. And the third one is making this whole idea a no-go.

Gotta stick with the development shell on this one.

### Thursday, 2 {#2}

#### Zigmod {#2#zigmod}

[Finished](https://github.com/NixOS/nixpkgs/pull/217229#issuecomment-1451479971) the yesterday's work. Learned how to write [package tests](https://nixos.org/manual/nixpkgs/stable/#sec-package-tests) along the way.

### Wednesday, 1 {#1}

#### Objectivism

There seems to be a couple of issues with [Objectivism](https://en.wikipedia.org/wiki/Objectivism):

- [Glaring lack of priority of virtue acquisition in ethics](https://forum.objectivismonline.com/index.php?/topic/1892-aristotelianism-vs-objectivism/)
- [Rather aggressive focus on extremes](https://www.youtube.com/watch?v=TrmT6t0XWOk&lc=UgxEMuhawljZYRDbAeR4AaABAg.9mgbYFdyIM49mgjsq2_-0b)

The latter is something that struck me, too, while reading "[Philosophy: Who Needs It](https://en.wikipedia.org/wiki/Philosophy:_Who_Needs_It)".

Interestingly, both linked resources mention these issues, and both refer to [Aristotelianism](https://en.wikipedia.org/wiki/Aristotelianism) as a more practical solution. This might be a reason to explore the latter in parallel to objectivism.

#### Zigmod {#1#zigmod}

I learned how to use [fixed-output derivations](https://nixos.org/manual/nix/stable/language/advanced-attributes.html#adv-attr-outputHash) in [Nix](https://nixos.org) packages. This is big since it allows me to package pretty much anything now (you can bypass the network restrictions with them).

I'm almost done with the rewrite of the [Zigmod](https://github.com/nektro/zigmod) package, but deterministic builds with Git repositories are [apparently pretty complex](https://github.com/NixOS/nixpkgs/issues/8567). Thankfully, I only need the code, so I fixed my issue last minute by nuking all `.git` directories.

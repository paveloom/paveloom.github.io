# August 2023

### Thursday, 31 {#31}

#### [`gnome-shell-memento-mori`](../../git.md#gnome-shell-memento-mori) {#31#gnome-shell-memento-mori}

Finished the rewrite. Found out that [`Gio.Settings.get_string()`](https://gjs-docs.gnome.org/gio20~2.0/gio.settings#method-get_string) can be used to get the `nick` of the currently active state of an `enum`. It is mentioned in the documentation of the [`Gio.Settings.set_enum()`](https://gjs-docs.gnome.org/gio20~2.0/gio.settings#method-set_enum) method.

Also, found out about the [rules for XML internationalization](https://www.gnu.org/software/gettext/manual/html_node/Preparing-ITS-Rules.html). Modified the [`GSchema`'s ITS file](https://gitlab.gnome.org/GNOME/glib/-/blob/55e5e3bcd8dc72807ba70a557eaf73e2e7bfe251/gio/gschema.its) so that [`xgettext`](https://www.gnu.org/software/gettext/manual/html_node/xgettext-Invocation.html) doesn't pick up `summary` and `description` strings. Made the same workflow for managing translations work with a simple [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) script.

I found the [Learn X in Y minutes](https://learnxinyminutes.com)'s resources for [JavaScript](https://learnxinyminutes.com/docs/javascript) (including the mentioned [MDN's language overview](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Language_overview)) and [TypeScript](https://learnxinyminutes.com/docs/typescript) to be a great way to refresh my knowledge of these languages.

Also, I was able to make the emitted [JavaScript](https://en.wikipedia.org/wiki/JavaScript) code look pretty much the same as the [TypeScript](https://en.wikipedia.org/wiki/TypeScript) code via a separate [ESLint](https://eslint.org) config and a couple of [`sed`](https://en.wikipedia.org/wiki/Sed) calls.

### Wednesday, 30 {#30}

#### [`gnome-shell-memento-mori`](../../git.md#gnome-shell-memento-mori) {#30#gnome-shell-memento-mori}

Almost done with the rewrite. I dropped the [`date-fns`](https://date-fns.org) library in favor of computing total number of *average* months and years, just so that I don't have to bundle dependencies.

### Monday, 28 {#28}

#### [`gnome-shell-memento-mori`](../../git.md#gnome-shell-memento-mori) {#28#gnome-shell-memento-mori}

I learned that [`typescript-language-server`](https://github.com/typescript-language-server/typescript-language-server) can provide limited type checking in pure JavaScript via [JSDoc annotations](https://www.typescriptlang.org/docs/handbook/intro-to-js-ts.html#providing-type-hints-in-js-via-jsdoc). Naturally, I tried it, and then quickly decided to switch back to TypeScript. I still drop [Rollup](https://rollupjs.org), though. Since GNOME Shell has transitioned to [ESM](https://tc39.es/ecma262/#sec-modules), calling [`tsc`](https://www.typescriptlang.org/docs/handbook/compiler-options.html) works quite well (I will have to implement a workaround for [preserving blank lines](https://github.com/microsoft/TypeScript/issues/843), though).

Spent most of the day figuring out how to make [`ts-for-gir`](https://github.com/gjsify/ts-for-gir/blob/3.x/packages/cli/README.md) work. It eventually did!

### Sunday, 27 {#27}

#### [`gnome-shell-memento-mori`](../../git.md#gnome-shell-memento-mori) {#27#gnome-shell-memento-mori}

I've started preparing my [GNOME Shell](https://gitlab.gnome.org/GNOME/gnome-shell) extension for [GNOME](https://www.gnome.org) 45. I decided I will rewrite everything in pure [Javascript](https://en.wikipedia.org/wiki/Javascript), nuking [Typescript](https://en.wikipedia.org/wiki/TypeScript), [Rollup](https://rollupjs.org) and use of a pseudo-build system (`package.json` scripts). There is really no need of all this bloat in this case.

Also, I've changed my mind on [`flake-utils`](https://github.com/numtide/flake-utils). Rewriting parts of it myself quickly became a hurdle and increased the amount of noise in the code, so I would rather stick with this extra dependency.

### Saturday, 26 {#26}

#### Maintenance {#26#maintenance}

I've solved a couple of issues with the [Codon](https://docs.exaloop.io/codon) [Nix](https://nixos.org) package in the currently open [pull request](https://github.com/NixOS/nixpkgs/pull/241723). Specifically, making the compiling to executable feature work out of the box took me the whole day to figure out. Had to write a tiny bit of [C++](https://en.wikipedia.org/wiki/C++) code (yikes!), so that I can override the [runtime search paths](https://en.wikipedia.org/wiki/Rpath) from Nix and make sure the [Zlib](https://zlib.net) library is visible to the C++ compiler (Codon calls it under the hood).

### Friday, 25 {#25}

#### Japanese {#25#japanese}

Damn, found out I missed pre-lesson exercises in other places of the textbook and workbook (4 places to look in total, plus 2 in the Answer Key). That, and I found even more online resources today. Plus, I will be using [Anki](https://apps.ankiweb.net) for expanding vocabulary, eventually.

Also, I found a [tier list](https://www.reddit.com/r/Hololive/comments/iqlzwv/recommendations_for_learning_japanese/g4t3wpq) of the [Hololive](https://en.wikipedia.org/wiki/Hololive_Production) members ranked by how easy it is to comprehend what they're saying. I do have [Okayu's Minecraft stream](https://www.youtube.com/watch?v=ntgtxLmF2r4) open in a tab now...

#### Maintenance {#25#maintenance}

I removed [`flake-utils`](https://github.com/numtide/flake-utils) from my [`dotfiles`](../../git.md#dotfiles) Nix flake today (a [bad practice](https://ayats.org/blog/no-flake-utils)). Also, I don't pass `pkgs` to the `nixosSystem` anymore (also a [bad practice](https://discourse.nixos.org/t/allowunfree-doesnt-work-with-flake-managed-system/21798/14)). Now I provide as an output the `nixosConfigurations` attribute set instead of the `packages.${system}.nixosConfigurations` attribute set.

### Thursday, 24 {#24}

#### Japanese {#24#japanese}

Well, now I can thank for the food.

よろしくおねがいします！

### Wednesday, 23 {#23}

#### Japanese {#23#japanese}

Memorized [Katakana](https://en.wikipedia.org/wiki/Katakana), too. Although, admittedly, it's much easier to read the syllables than to write them by hand (for both syllabaries). There's still some effort required for deciphering, too. Especially when diacritics and digraphs are involved. Some reading is necessary.

#### Maintenance {#23#maintenance}

Updated my site to use the [new built-in plugin](https://github.com/squidfunk/mkdocs-material/releases/tag/9.2.0). Created a [pull request](https://github.com/NixOS/nixpkgs/pull/250955) to update [Quod Libet](https://quodlibet.readthedocs.io) in [Nixpkgs](https://github.com/NixOS/nixpkgs).

### Tuesday, 22 {#22}

#### Japanese {#22#japanese}

Aight, I memorized [Hiragana](https://en.wikipedia.org/wiki/Hiragana) (basic syllables + syllables with diacritical marks + the contracted sounds). Will need a bit more practice, of course. Speaking of which, I found the [Real Kana](https://realkana.com) website to be immensely useful for learning and practicing the syllables.

### Monday, 21 {#21}

#### Japanese {#21#japanese}

Started learning Japanese today. Kind of because of [Hololive](https://en.wikipedia.org/wiki/Hololive_Production), but also because of a vastly different culture. I have [Nyarons](https://nyarons.bandcamp.com) in my music collection and can't understand their songs, but they sound so cool! Anyway, I've started with [GEIKI](https://genki3.japantimes.co.jp/en) as a complete beginner. This is my second attempt to approach the language, though.

Learned a bunch about [IMFs](https://wiki.archlinux.org/title/Localization/Japanese#Input_Method_Framework_(IMF)) and [IMEs](https://wiki.archlinux.org/title/Localization/Japanese#Input_Method_Editor_(IME)). They are pretty easy to set up on [NixOS](https://nixos.org/manual/nixos/stable/index.html#module-services-input-methods): I chose to use the [Mozc](https://github.com/google/mozc) IME. I've set it up to use the US layout in the Direct Input mode. This allows me to have English and Japanese in one layout! I've also installed the [10ten Japanese Reader](https://github.com/birchill/10ten-ja-reader) extension in [Librewolf](https://librewolf.net), so that I can get explanations for how words are constructed by hovering over them.

### Saturday, 19 {#19}

#### HoloCure {#19#holocure}

Aight, the [IRyS strat](https://www.youtube.com/watch?v=LD_67QBTNtE) works, although I was able to get maximum 250K HoloCoins on S1 (Hard). Didn't have everything upgraded yet, though. YAGOOs kill me rather quickly after the 30 minute mark.

### Friday, 18 {#18}

#### HoloCure {#18#holocure}

Today I played [HoloCure](https://kay-yu.itch.io/holocure) and found out about [Hololive](https://en.wikipedia.org/wiki/Hololive_Production). The game is cool, but I'm not sure about the Hololive's content yet.

### Thursday, 17 {#17}

#### [Kirk](../../git.md#kirk) {#17#kirk}

Read a bit more on [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol). Found out that I could just use the [`libsoup`](https://libsoup.org/libsoup-3.0/index.html) library to make HTTP conversations. I would also like to switch to [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2) over [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) if [Qobuz](https://www.qobuz.com/us-en/shop) supports it. I don't even know how to quickly test it, since it seems to work completely differently compared to [HTTP/1.1](https://en.wikipedia.org/wiki/HTTP) (tried with [Netcat](https://en.wikipedia.org/wiki/Netcat)). Gotta learn a lot of networking stuff!

Also, I think I found a bug in [GIO](https://docs.gtk.org/gio): the [`GTask`](https://docs.gtk.org/gio/class.Task.html) in the [`g_tcp_connection_close_async`](https://gitlab.gnome.org/GNOME/glib/-/blob/95baa8dcc5a08ff04188de0a7902aa0fcec2699e/gio/gtcpconnection.c#L250) function (which is allocated when graceful disconnects are enabled) is never freed if there are no errors. Usually, one would put a `g_object_unref(task)` after any of the `g_task_return_*` functions.

### Wednesday, 16 {#16}

#### [Kirk](../../git.md#kirk) {#16#kirk}

Used [Wireshark](https://www.wireshark.org) to find out that my login conversation was closing abruptly (the [connection termination phase](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Protocol_operation) was just skipped each time). Turns out that one has to explicitly enable it when using GIO: via the [`g_tcp_connection_set_graceful_disconnect`](https://docs.gtk.org/gio/method.TcpConnection.set_graceful_disconnect.html) function.

#### Time tracking {#16#time-tracking}

I will stop tracking my time. Putting the stats at the end of the day became mechanical and doesn't seem like something that affects me in any way anymore. If I have something scheduled, I will do it anyway, if possible. It also doesn't represent some of my commitments that I find somewhat valuable. For example, today I've reorganized [my NixOS config](https://github.com/paveloom/dotfiles), which took quite a while. Then I created a couple of pull requests to update [KeePassXC](https://github.com/NixOS/nixpkgs/pull/249505) and [Picard](https://github.com/NixOS/nixpkgs/pull/249507) in Nixpkgs. I would call this type of activity *maintenance*, and I usually don't log maintenance tasks, because it always feels like it won't take that much time (and then it does).

### Tuesday, 15 {#15}

#### Wireshark

Finally, finished learning [Wireshark](https://www.wireshark.org).

#### Time tracking {#15#time-tracking}

- Today [03:29:26]
    - Consuming [03:29:26]
        - Research [01:58:07]
            - Learning [Wireshark](https://www.wireshark.org) [01:58:07]
        - Reading [01:31:19]
            - [1991] [Objectivism: The Philosophy of Ayn Rand](https://en.wikipedia.org/wiki/Objectivism:_The_Philosophy_of_Ayn_Rand) [01:31:19]

### Sunday, 13 {#13}

#### Wi-Fi power management

I think I found the reason my network speed was getting lower on idle: presumably, it's a Linux module that enables Wi-Fi power management ([dynamic power-saving](https://www.ctrl.blog/entry/linux-wifi-dpm-latency.html)). Here's a quick fix via [NetworkManager](https://www.networkmanager.dev) on [NixOS](https://nixos.org):

```nix
networking.networkmanager.wifi.powersave = false;
```

#### Time tracking {#13#time-tracking}

- Today [03:04:43]
    - Consuming [03:04:43]
        - Research [02:44:02]
            - Learning [Wireshark](https://www.wireshark.org) [02:44:02]
        - Reading [00:20:41]
            - [1991] [Objectivism: The Philosophy of Ayn Rand](https://en.wikipedia.org/wiki/Objectivism:_The_Philosophy_of_Ayn_Rand) [00:20:41]

### Saturday, 12 {#12}

#### Time tracking {#12#time-tracking}

- Today [02:10:09]
    - Consuming [02:10:09]
        - Research [02:10:09]
            - Learning [Wireshark](https://www.wireshark.org) [02:10:09]

### Friday, 11 {#11}

#### Time tracking {#11#time-tracking}

- Today [04:28:43]
    - Consuming [04:28:43]
        - Research [02:27:49]
            - Learning [Wireshark](https://www.wireshark.org) [02:27:49]
        - Reading [01:18:59]
            - [1991] [Objectivism: The Philosophy of Ayn Rand](https://en.wikipedia.org/wiki/Objectivism:_The_Philosophy_of_Ayn_Rand) [01:18:59]
        - Watching [00:41:55]
            - [2019] [The Rubin Report: Objectivism on Happiness](https://www.youtube.com/playlist?list=PLqsoWxJ-qmMtazmdDur38Fb9of1wpW2Sb) [00:41:55]

### Thursday, 10 {#10}

#### Time tracking {#10#time-tracking}

- Today [03:28:15]
    - Producing [00:39:48]
        - Software engineering [00:39:48]
            - [Updating the Zigmod Nix package](https://github.com/NixOS/nixpkgs/pull/217229) [00:39:48]
    - Consuming [02:48:27]
        - Research [02:32:06]
            - Learning [Wireshark](https://www.wireshark.org) [02:32:06]
        - Watching [00:16:21]
            - [2019] [The Rubin Report: Objectivism on Happiness](https://www.youtube.com/playlist?list=PLqsoWxJ-qmMtazmdDur38Fb9of1wpW2Sb) [00:16:21]

### Monday, 7 {#7}

#### [Kirk](../../git.md#kirk) {#7#kirk}

I scrapped the idea of reducing the closure size of the output of the Nix package. My overlays affected not only my package's and its derivation's closures, but also the `devShell`'s closures, which I completely didn't think about. At some point I started compiling [WebKit](https://webkit.org) from scratch (because it has [Cairo](https://cairographics.org) as a requisite, which I had an overlay for). As you can imagine, this isn't good (too long to compile). This made me realize that the idea of a generic runtime is much better for distributing an application (which is what [Flatpak](https://www.flatpak.org) is known for). Nix flakes allow me to lock [Nixpkgs](https://github.com/NixOS/nixpkgs) to a specific version, and then consumers can override it to their version if they'd like. Using the locked version would mean getting a guaranteed build at the price of downloading and storing (highly likely) duplicated dependencies. Overriding the input would mean using Nixpkgs as a runtime, with shared libraries updated at your will (which can potentially break the application). I can still support both of these choices, but by favoring the latter option (which would mean not overlaying anything) I don't have to compile the world every time I update the lock file.

Also, Nix makes it easy to fiddle with the derivation if necessary (which is what Flatpak can't provide), and there are no annoyances because of sandboxing.

#### Time tracking {#7#time-tracking}

- Today [03:17:36]
    - Producing [02:42:22]
        - Software engineering [02:42:22]
            - [Kirk](../../git.md#kirk) [02:42:22]
    - Consuming [00:35:14]
        - Research [00:35:14]
            - [The Monthly method](https://monthlymethod.com) and their resources [00:35:14]

### Sunday, 6 {#6}

#### [Kirk](../../git.md#kirk) {#6#kirk}

I was able to reduce the closure size of the *output* of the Nix package from 875 MiB to approximately 200 MiB by just removing the [GStreamer media backend](https://gitlab.gnome.org/GNOME/gtk/-/blob/4.10.4/meson_options.txt#L37-40). I mean, that's just a single flag! Ironically, as I edit the derivations of more and more packages, the closure size of the Kirk's *derivation* gets bigger (e.g., [`librsvg`](https://gitlab.gnome.org/GNOME/librsvg) requires Rust, so it's part of the closure now).

Found another neat tool: [`nix-output-monitor`](https://github.com/maralorn/nix-output-monitor). It shows you an active part of the graph of the derivation's closure while building.

#### Time tracking {#6#time-tracking}

- Today [02:00:51]
    - Producing [02:00:51]
        - Software engineering [02:00:51]
            - [Kirk](../../git.md#kirk) [02:00:51]

### Saturday, 5 {#5}

#### [Kirk](../../git.md#kirk) {#5#kirk}

Was working on reducing the size of the closure of the Nix package. Turned out to be quite an involved process! Found out the [`nix-tree`](https://github.com/utdemir/nix-tree) tool to be very useful to quickly navigate the tree of requisites (it supports derivations, too!). Also, related to this, found out that the latest version of [`nix-direnv`](https://github.com/nix-community/nix-direnv) supports manual reload of the Nix environment.

#### Time tracking {#5#time-tracking}

- Today [03:07:09]
    - Producing [03:07:09]
        - Software engineering [03:07:09]
            - [Kirk](../../git.md#kirk) [02:29:20]
            - [Updating Picard in Nixpkgs](https://github.com/NixOS/nixpkgs/pull/247321) [00:37:49]

### Friday, 4 {#4}

#### [Kirk](../../git.md#kirk) {#4#kirk}

Committed the code that makes the authorization button work. Switched to using [`Adw.Toast`](https://gnome.pages.gitlab.gnome.org/libadwaita/doc/1-latest/class.Toast.html)s for notifying the user about the results. Also, made it so that the request can be cancelled using the same button.

#### Time tracking {#4#time-tracking}

- Today [06:23:10]
    - Producing [03:57:02]
        - Software engineering [03:57:02]
            - [Kirk](../../git.md#kirk) [03:57:02]
    - Consuming [02:26:08]
        - Reading [01:24:05]
            - [1991] [Objectivism: The Philosophy of Ayn Rand](https://en.wikipedia.org/wiki/Objectivism:_The_Philosophy_of_Ayn_Rand) [01:24:05]
        - Research [01:02:03]
            - [The Monthly method](https://monthlymethod.com) and their resources [01:02:03]

### Thursday, 3 {#3}

#### [Kirk](../../git.md#kirk) {#3#kirk}

Found out what was causing the issue: I had to switch from the [`g_input_stream_read_all_async`](https://docs.gtk.org/gio/method.InputStream.read_all_async.html) function to the [`g_input_stream_read_async`](https://docs.gtk.org/gio/method.InputStream.read_async.html) function. Seems like it was trying to read it all and failing for some reason until the timeout.

Got the authentication working! Added a simple light bulb that changes color to indicate the success of the verification.

#### Time tracking {#3#time-tracking}

- Today [06:55:15]
    - Producing [04:23:48]
        - Software engineering [04:23:48]
            - [Kirk](../../git.md#kirk) [04:23:48]
    - Consuming [02:31:27]
        - Reading [01:44:43]
            - [1991] [Objectivism: The Philosophy of Ayn Rand](https://en.wikipedia.org/wiki/Objectivism:_The_Philosophy_of_Ayn_Rand) [01:44:43]
        - Research [00:46:44]
            - [The Monthly method](https://monthlymethod.com) and their resources [00:46:44]

### Wednesday, 2 {#2}

#### [Kirk](../../git.md#kirk) {#2#kirk}

Figured out how to wrap a chain of asynchronous operations into a new asynchronous operation and pass data in between using [`GTask`](https://docs.gtk.org/gio/class.Task.html). Wrote a lot of asynchronous functions for logging in [Qobuz](https://www.qobuz.com). By the end of the day I was able to get a response! Albeit, sometimes I get it, and sometimes I don't. I think this might be related to a socket not being closed, I once had a similar issue with [`mini-database-server-in-zig`](../../git.md#mini-database-server-in-zig), but never found out why Linux be like that.

#### Time tracking {#2#time-tracking}

- Today [06:06:43]
    - Producing [05:01:39]
        - Software engineering [05:01:39]
            - [Kirk](../../git.md#kirk) [05:01:39]
    - Consuming [01:05:04]
        - Research [01:05:04]
            - Quantifying time investment [01:05:04]
                - [2021] [My year in data](https://samplesize.one/blog/posts/my_year_in_data)
                - [2017] [Time Well Spent](https://putanumonit.com/2017/05/09/time-well-spent)

### Tuesday, 1 {#1}

#### [Kirk](../../git.md#kirk) {#1#kirk}

Was working on making the "Verify credentials" button work. Really struggling with figuring out how to do a proper composition of asynchronous tasks. I don't quite understand the examples in the [`GTask`](https://docs.gtk.org/gio/class.Task.html)'s documentation.

#### Time tracking {#1#time-tracking}

- Today [07:31:58]
    - Producing [05:05:27]
        - Software engineering [05:05:27]
            - [Kirk](../../git.md#kirk) [05:05:27]
    - Consuming [02:26:31]
        - Reading [01:05:30]
            - [1991] [Objectivism: The Philosophy of Ayn Rand](https://en.wikipedia.org/wiki/Objectivism:_The_Philosophy_of_Ayn_Rand) [01:05:30]
        - Research [01:21:01]
            - Debugging [Nix](https://nixos.org) closure sizes [01:21:01]
                - [2022] [NixCon 2022: Debugging Closure Sizes Graphically with nix-closure-graph](https://jade.fyi/nixcon2022)

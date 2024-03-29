# October 2023

### Tuesday, 31 {#31}

#### [Kirk](../../git.md#kirk) {#31#kirk}

Spent quite a lot of time figuring out how to connect to signals from the [Secret Service API](https://specifications.freedesktop.org/secret-service/latest) via D-Bus to set up my callback when the password changes in the secret store (spoiler: you apparently can't monitor that, only public parameters), just to find a simpler, one line solution afterward.

Also, started wrapping my blocking regex searching functions in asynchronous wrappers, as described [here](https://developer.gnome.org/documentation/tutorials/threading.html), so that I can use the `g_task_run_in_thread` function and make them run in a worker thread. Simply using `std::thread` from the C++ standard library turned out to be not enough to avoid blocks.

I get somewhat annoyed by C++'s need to run destructors all the time. For example, I can't really steal the pointer from an `std::string` to bypass the destructor. This causes workarounds in asynchronous code, such as copying the string to `GTask`'s data. I might as well just use GLib... and switch back to C?...

### Monday, 30 {#30}

#### [Kirk](../../git.md#kirk) {#30#kirk}

Implemented the retrieval of the application ID from [Qobuz](https://www.qobuz.com). Used the support for regular expressions and multi-threading in the standard C++ library. Currently, in the process of implementing UI nuances (like updating the password entry on changes in the secret store), so that the process of fetching the ID is more intuitive.

### Friday, 27 {#27}

#### [Kirk](../../git.md#kirk) {#27#kirk}

I started rewriting the code using [`gtkmm`](https://gtkmm.org), but eventually encountered that there is no pretty way to derive from the [`libadwaita`](https://gitlab.gnome.org/GNOME/libadwaita)'s widgets. There are hardly any examples of someone doing it ([GNOME System Monitor](https://gitlab.gnome.org/GNOME/gnome-system-monitor/) is one, but they just use the GTK Builder to avoid defining new classes). Also, I found out that you can't use templates in XML descriptions of user interfaces, only concrete objects. Furthermore, the API seems slightly different from the upstream (and their [reference](https://gnome.pages.gitlab.gnome.org/gtkmm) doesn't have a search!). I decided to stick with C-like C++ for the time being.

Started writing support for fetching application ID from [Qobuz](https://www.qobuz.com).

### Thursday, 26 {#26}

#### [Kirk](../../git.md#kirk) {#26#kirk}

Converted the code from C to the C-like C++. Turned out to be a pretty straight forward thing to do. Started rewriting into more C++-esque style using [`gtkmm`](https://gtkmm.org). Hopefully type-safe callbacks are worth it!

### Wednesday, 25 {#25}

#### [Kirk](../../git.md#kirk) {#25#kirk}

Started converting the code from C to C++.

#### Maintenance {#25#maintenance}

Worked on polishing the [pull request](https://github.com/NixOS/nixpkgs/pull/263111) to update [qBittorrent](https://www.qbittorrent.org) in [Nixpkgs](https://github.com/NixOS/nixpkgs).

### Tuesday, 24 {#24}

#### C++ {#24#c++}

Finished reading [A Tour of C++](https://www.stroustrup.com/tour3.html). It changed my opinion of C++ (I think it's pretty good).

#### Maintenance {#24#maintenance}

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/263111) to update [qBittorrent](https://www.qbittorrent.org) in [Nixpkgs](https://github.com/NixOS/nixpkgs).

### Monday, 23 {#23}

#### C++ {#23#c++}

Read more of [A Tour of C++](https://www.stroustrup.com/tour3.html).

### Friday, 20 {#20}

#### C++ {#20#c++}

Read more of [A Tour of C++](https://www.stroustrup.com/tour3.html).

### Thursday, 19 {#19}

#### C++ {#19#c++}

Read more of [A Tour of C++](https://www.stroustrup.com/tour3.html).

#### Maintenance {#19#maintenance}

Spent a lot of time figuring out what went wrong with using [`mold`](https://github.com/rui314/mold) on Darwin (checks failed [here](https://github.com/NixOS/nixpkgs/pull/261856)). Turns out support for macOS exists as a separate commercial project by the same authors, called [`sold`](https://github.com/bluewhalesystems/sold) (that's pretty funny). Learned about `otool` and `objdump` as alternatives to `patchelf` and `readelf`.

### Wednesday, 18 {#18}

#### C++ {#18#c++}

Read more of [A Tour of C++](https://www.stroustrup.com/tour3.html). Turns out the support for generic programming in C++ is very powerful!

#### Maintenance {#18#maintenance}

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/261856) to update [`mold`](https://github.com/rui314/mold) in [Nixpkgs](https://github.com/NixOS/nixpkgs). Also, I've set `allowedRequisites` to `null` for `useMoldLinker` to avoid issues downstream (see [#261825](https://github.com/NixOS/nixpkgs/issues/261825)).

### Tuesday, 17 {#17}

#### C++ {#17#c++}

Read more of [A Tour of C++](https://www.stroustrup.com/tour3.html).

#### Maintenance {#17#maintenance}

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/261589) to update [Tracy](https://github.com/wolfpld/tracy) in [Nixpkgs](https://github.com/NixOS/nixpkgs).

#### [`mpv`](https://mpv.io) {#17#mpv}

Turns out you can create per-directory configuration files (`mpv.conf`) and make them loaded automatically if you enable the [`use-filedir-conf`](https://mpv.io/manual/stable/#options-use-filedir-conf) option. So, for example, you can select an audio stream, a subtitles stream, a size and a position of the subtitles, for all files in a directory. Really useful when watching series!

### Monday, 16 {#16}

#### C++ {#16#c++}

Read more of [A Tour of C++](https://www.stroustrup.com/tour3.html). Already diving in complexities like [the rule of three/five/zero](https://en.cppreference.com/w/cpp/language/rule_of_three). Added locally a `pkg-config` file to the [Nixpkgs](https://github.com/NixOS/nixpkgs) package for [Microsoft's Guidelines Support Library](https://github.com/Microsoft/GSL) to make it work with Meson.

### Friday, 13 {#13}

#### C++ {#13#c++}

Started reading [A Tour of C++](https://www.stroustrup.com/tour3.html).

### Thursday, 12 {#12}

#### [Kirk](../../git.md#kirk) {#12#kirk}

Finished reading the [GDB manual](https://sourceware.org/gdb/current/onlinedocs/gdb).

### Wednesday, 11 {#11}

#### [Kirk](../../git.md#kirk) {#11#kirk}

Finished setting up additional linters ([`clang-tidy`](https://clang.llvm.org/extra/clang-tidy), [`scan-build`](https://clang-analyzer.llvm.org/scan-build.html), and [PVS-Studio](https://pvs-studio.com)). Surprisingly, there was only one new warning that I found useful: I didn't initialize a variable in one place, and `clang-tidy` notified me about that.

Read a part of the [GDB manual](https://sourceware.org/gdb/current/onlinedocs/gdb).

### Monday, 9 {#9}

#### Algorithms & Data Structures {#9#algorithms-and-data-structures}

Started reading [Cracking the Coding Interview](https://www.amazon.com/dp/0984782850).

#### Japanese {#9#japanese}

Dropped learning the language since I don't see the pragmatic reasoning of its usage anymore.

### Friday, 6 {#5}

#### Counter-Strike: Source {#6#counter-strike-source}

Wrote the blog post after finally figuring out how to fix the issues with the fonts.

### Thursday, 5 {#5}

#### Counter-Strike: Source {#5#counter-strike-source}

Worked on fixing some of the issues with the [game](https://en.wikipedia.org/wiki/Counter-Strike:_Source) (almost finished with a blog post on fixing issues with fonts on Linux). Found my old `custom` folder and configs on an old laptop!

### Wednesday, 4 {#4}

#### [Kirk](../../git.md#kirk) {#4#kirk}

Set up [PVS-Studio](https://pvs-studio.com/en/pvs-studio), started going through the warnings.

### Tuesday, 3 {#3}

#### Japanese {#3#japanese}

Switched from the [Tae Kim's Guide to Learning Japanese](https://guidetojapanese.org/learn/complete) to [IMABI](https://imabi.org) for proper explanations.

#### [Kirk](../../git.md#kirk) {#3#kirk}

Started learning how to use [PVS-Studio](https://pvs-studio.com/en/pvs-studio). Since I got a trial key yesterday and packaged the CLI tools to [Nixpkgs](https://github.com/NixOS/nixpkgs) today, I will look into it first before returning to [`rr`](https://github.com/rr-debugger/rr) and [GDB](https://www.sourceware.org/gdb).

Excited to find out what their implementation of [MISRA C](https://en.wikipedia.org/wiki/MISRA_C) will find in my code!

#### Maintenance {#3#maintenance}

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/258817) to add [PVS-Studio](https://pvs-studio.com/en/pvs-studio) to [Nixpkgs](https://github.com/NixOS/nixpkgs).

### Monday, 2 {#2}

#### Maintenance {#2#maintenance}

Created a [pull request](https://github.com/NixOS/nixpkgs/pull/258646) to add [VTFEdit](https://developer.valvesoftware.com/wiki/VTFEdit) to [Nixpkgs](https://github.com/NixOS/nixpkgs). That's a rather old Windows application (circa 2005--2011), but it's rather important for people who'd like to have high quality sprays in [Source](https://en.wikipedia.org/wiki/Source) games. So, I used [Wine](https://en.wikipedia.org/wiki/Wine_(software)) and [`winetricks`](https://github.com/Winetricks/winetricks) to make it work!

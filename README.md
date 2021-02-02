<!-- Please be careful editing the below HTML, as GitHub is quite finicky with anything that looks like an HTML tag in GitHub Flavored Markdown. -->
<p align="center">
  <img width="75%" src="assets/banner/banner.svg" alt="Banner">
</p>
<p align="center">
  <b>Tiling window management for the Mac.</b>
</p>
<p align="center">
  <a href="https://travis-ci.org/koekeishiya/yabai">
    <img src="https://travis-ci.org/koekeishiya/yabai.svg?branch=master" alt="CI Status Badge">
  </a>
  <a href="https://github.com/koekeishiya/yabai/blob/master/LICENSE.txt">
    <img src="https://img.shields.io/github/license/koekeishiya/yabai.svg?color=green" alt="License Badge">
  </a>
  <a href="https://github.com/koekeishiya/yabai/blob/master/CHANGELOG.md">
    <img src="https://img.shields.io/badge/view-changelog-green.svg" alt="Changelog Badge">
  </a>
  <a href="https://github.com/koekeishiya/yabai/releases">
    <img src="https://img.shields.io/github/commits-since/koekeishiya/yabai/latest.svg?color=green" alt="Version Badge">
  </a>
</p>

## About

<img align="right" width="40%" src="assets/screenshot.png" alt="Screenshot">

yabai is a window management utility that is designed to work as an extension to the built-in window manager of macOS.
yabai allows you to control your windows, spaces and displays freely using an intuitive command line interface and optionally set user-defined keyboard shortcuts using [&nearr;&nbsp;skhd][gh-skhd] and other third-party software.

The primary function of yabai is tiling window management; automatically modifying your window layout using a binary space partitioning algorithm to allow you to focus on the content of your windows without distractions.
Additional features of yabai include focus-follows-mouse, disabling animations for switching spaces, creating spaces past the limit of 16 spaces, and much more.

## Installation and Configuration

- The [&nearr;&nbsp;yabai&nbsp;wiki][yabai-wiki] has both brief and detailed installation instructions for multiple installation methods, and also explains how to uninstall yabai completely.
- Sample configuration files can be found in the [&nearr;&nbsp;examples][yabai-examples] directory. Refer to the [&nearr;&nbsp;documentation][yabai-docs] or the wiki for further information.
- Keyboard shortcuts can be defined with [&nearr;&nbsp;skhd][gh-skhd] or any other suitable software you may prefer.

## Requirements and Caveats

Please read the below requirements carefully.
Make sure you fulfil all of them before filing an issue.

|Requirement|Note|
|-:|:-|
|Operating&nbsp;System|macOS&nbsp;High&nbsp;Sierra&nbsp;10.13.6, Mojave&nbsp;10.14.4+ and Catalina 10.15.0+ is supported.|
|Accessibility&nbsp;API|yabai must be given permission to utilize the Accessibility API and will request access upon launch. The application must be restarted after access has been granted.|
|Mission&nbsp;Control|In the Mission Control preferences pane in System Preferences, the setting "Displays have separate Spaces" must be enabled.|

Please also take note of the following caveats.

|Caveat|Note|
|-:|:-|
|System&nbsp;Integrity&nbsp;Protection|System Integrity Protection needs to be (partially) disabled for yabai to inject a scripting addition into Dock.app for controlling windows with functions that require elevated privileges. This enables control of the window server, which is the sole owner of all window connections, and enables additional features of yabai. If you are running on macOS High Sierra 10.13.6, you can reenable SIP after the scripting addition has been installed.|
|Code&nbsp;Signing|When building from source (or installing from HEAD), it is recommended to codesign the binary so it retains its accessibility and automation privileges when updated or rebuilt.|
|Mission&nbsp;Control|In the Mission Control preferences pane in System Preferences, the setting "Automatically rearrange Spaces based on most recent use" should be disabled for commands that rely on the ordering of spaces to work reliably.|

## License and Attribution

yabai is licensed under the [&nearr;&nbsp;MIT&nbsp;License][yabai-license], a short and simple permissive license with conditions only requiring preservation of copyright and license notices.
Licensed works, modifications, and larger works may be distributed under different terms and without source code.

Thanks to [@fools-mate][gh-fools-mate] for creating a logo and banner for this project and making them available for free.

Thanks to [@dominiklohmann][gh-dominiklohmann] for contributing great documentation, support, and more, for free.

## Disclaimer

Use at your own discretion.
I take no responsibility if anything should happen to your machine while trying to install, test or otherwise use this software in any form.
You acknowledge that you understand the potential risk that may come from disabling [&nearr;&nbsp;System&nbsp;Integrity&nbsp;Protection][external-about-sip] on your system, and I make no recommendation as to whether you should or should not disable System Integrity Protection.

## New stuff

##### Random active border color

- Instead of using the default active border color, or the color defined in the `.yabairc` with `yabai -m config active_window_border_color`, the windows active border color will cycle randomly through the eight currently available colors on every new window focus.
- At some point I want the option of `yabai -m config active_window_border_colors_list` to cycle through a list of custom colors, but it's currently not there.

```
# Currently cycled default colors:

# Red:    0xffff0000
# Orange: 0xffff6e00
# Yellow: 0xfffedd43
# Green:  0xff37f007
# Blue:   0xff00a9ff
# Pink:   0xfff000ff
# Purple: 0xff9e00ff
# White:  0xffffffff
```

```
# .yabairc
yabai -m config window_border_randomize     on                 # on or off (default: off)

# .skhdrc
ctrl + shift - r : yabai -m window --toggle border_randomize   # toggle on or off
```

##### Show window state

- Instead of the active border color being used for all focused windows, this option shows a specific color for the extra states of a focused window (float, sticky or zoomed).

- Normal active focused windows use the default `active_border_color` (0xff775759), or the color defined with `yabai -m config active_window_border_color` in the `.yabairc`.

- If this and the 'Random active border color' options are both enabled, this option will override the random border color and just show the appropriate window state border color.

- [This issue][window-state-issue]

```
# .yabairc
yabai -m config window_border_show_state     on                  # on or off (default: off)

yabai -m config float_window_border_color    0xfffedd43          # float window state color (default: 0xff597757)
yabai -m config sticky_window_border_color   0xffff6e00          # sticky window state color (default: 0xff575977)
yabai -m config zoom_window_border_color     0xffff0000          # zoom window state color (default: 0xff737757)

# .skhdrc
ctrl + shift - s : yabai -m window --toggle  border_show_state   # toggle on or off
```

##### Floating/tiled border only

- This option styles windows borders based on the set config options `yabai -m config window_border_tiled_only` or `yabai -m config window_border_float_only`. The windows not being affected by border styling will look like OS default.

- If both `window_border_tiled_only` and `window_border_float_only` are set to `on`, both will be negated and borders will appear on all windows as normal.

- [This comment](https://github.com/koekeishiya/yabai/issues/663#issuecomment-749329315)

```
# .yabairc
yabai -m config window_border_tiled_only     on                  # on or off (default: off)
yabai -m config window_border_float_only     off                 # on or off (default: off)

# .skhdrc
ctrl + shift - t : yabai -m window --toggle  border_tiled_only   # toggle on or off
ctrl + shift - f : yabai -m window --toggle  border_float_only   # toggle on or off
```


<!-- Project internal links -->
[yabai-license]: LICENSE.txt
[yabai-examples]: https://github.com/koekeishiya/yabai/tree/master/examples
[yabai-wiki]: https://github.com/koekeishiya/yabai/wiki
[yabai-docs]: https://github.com/koekeishiya/yabai/blob/master/doc/yabai.asciidoc

<!-- Links to other GitHub projects/users -->
[gh-skhd]: https://github.com/koekeishiya/skhd
[gh-chunkwm]: https://github.com/koekeishiya/chunkwm
[gh-fools-mate]: https://github.com/fools-mate
[gh-dominiklohmann]: https://github.com/dominiklohmann

<!-- External links -->
[external-about-sip]: https://support.apple.com/en-us/HT204899
[window-state-issue]: https://github.com/koekeishiya/yabai/issues/663
[yabai-repo]: https://github.com/koekeishiya/yabai
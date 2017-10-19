---
title: 程序员使用Mac笔记
categories: 编程那点事儿
tags:
  - Mac
  - mpv
  - G点
  - Spotlight
  - 配置
---
<blockquote class="blockquote-center">写笔记，可以增加生命的厚度。</blockquote>

<!--more-->

**如果能过目不忘且可温故知新，那就可以不记笔记了，否则就是轻视世界加轻视自己的一条咸鱼 —— 相信我，你会丢掉好多时光。**

## Mac 系统设置

### Spotlight - 搜索文件内容设置

system preferences - spotlight 的 “search results” 和 “privacy” 选项配置。

### Finder - 文件管理器搜不到的文件设置

user（你的名字）下其实有个看不见的 Library 文件夹，系统默认不可见。

访问方法：
``` bash
打开Finder，按 shift+command+g，输入“~/Library”（引号里面的），回车，进入Library下。
```
这样就可以找一些难找的文件（比如优酷下载的视频文件，在 /Library/Containers/com.youku.mac/Data 下）。

### 设定可以在指定文件夹内打开 iTerm 命令行

``` bash
1. 打开 “Automator” ➡️ 选 “服务” ➡️ 确定 “选取”
2. 选定 “文件夹” ，位于 “Finder.app”
3. 左侧搜索找到 “Open Finder Iterms” ，拖拽到右侧
4. CMD+S 存储为 “...”（你想叫的功能名字）
5. 验证：Finder中选定一个文件夹，右键 ➡️ 服务 ➡️ 找到你第4步命名的功能
```

比如我把这个功能叫做 `open-iterm-in-folder`：

![](http://ogudt6aal.bkt.clouddn.com/image/20170811021704_gav3Cv_open-iterm-in-folder.jpeg)
![](http://ogudt6aal.bkt.clouddn.com/image/20170811021902_NLRYOw_open-iterm-in-folder2.jpeg)

### 开机启动项

1. 最一般的：system preferences - users&groups 的 “login items”
2. Macintosh HD 下：
  - Library 下 Launch... 内的 .plist文件
  - System/Library 下 Launch... 内的 .plist文件

第2种情况如下处理：
  1. 删除 .plist文件（不建议，万一以后要用呢）
  2. 执行命令
``` bash
launchctl unload -w /Library/Launch.../... .plist
```
[命令参考链接](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/launchctl.1.html "命令参考链接")

## Mac 常用软件

### 备受好评的 mpv 播放器使用指南(默认配置)

#### 播放速度

``` bash
space - 暂停/播放

快：
⬆️ - 快进60s
➡️ - 快进5s   
] - 1.1倍快速播放
} - 2倍快速播放

慢：
⬇️ - 快退60s
⬅️ - 快退5s
[ - 0.9倍慢速播放
{ - 0.5倍慢速播放
Backspace - 重置为正常播放速度
```

#### 字幕 音轨

``` bash
j(J) - 循环（反向）选择字幕文件
# - 循环切换音轨
9 - 音量减 2
0 - 音量加 2
```

#### 屏幕相关

``` bash
f - 切换全屏
T - 切换视频窗口在最前
s（S） - 视频截图，包含（不包含）字幕
Alt+s - 自动逐帧视频截图，再按一次停止截图
. - 进到下一帧
, - 退到前一帧
```

#### 高级配置（这里才是玩Mac的程序员的G点）

/Users/yourname/.config/mpv 下新建如下文件实现不同配置。

``` bash
~/.config/mpv
```

##### 解码 字幕配置：mpv.conf

``` bash
vo=opengl:gamma-auto:icc-profile-auto

hwdec=auto

autofit-larger=92%

playcache=8192

lang=zh,chi

video-sync=display-resample

sub-codepage=enca:zh:utf8

sub-auto=fuzzy

sub-text-font-size=40

sub-text-shadow-offset=0

sub-text-color="#ffffffff"

sub-text-font="STZhongsong"

sub-codepage=utf8:gb18030

screenshot-template=mpv-screenshot-%f-%p

screenshot-format=png

osd-font="STZhongsong"

osd-font-size=36

--script=/Users/yourname/.config/mpv/autoload.lua

```

上面最后一行代码加载lua脚本用来配置播放列表。这样本文件夹下视频文件可自动连续播放, < 和 > 实现上一曲下一曲切换。新建 autoload.lua：

``` bash
-- This script automatically loads playlist entries before and after the
-- the currently played file. It does so by scanning the directory a file is
-- located in when starting playback. It sorts the directory entries
-- alphabetically, and adds entries before and after the current file to
-- the internal playlist. (It stops if the it would add an already existing
-- playlist entry at the same position - this makes it "stable".)
-- Add at most 5000 * 2 files when starting a file (before + after).
MAXENTRIES = 5000

function Set (t)
    local set = {}
    for _, v in pairs(t) do set[v] = true end
    return set
end

EXTENSIONS = Set {
    'mkv', 'avi', 'mp4', 'ogv', 'webm', 'rmvb', 'flv', 'wmv', 'mpeg', 'mpg', 'm4v', '3gp',
    'mp3', 'wav', 'ogv', 'flac', 'm4a', 'wma',
}

mputils = require 'mp.utils'

function add_files_at(index, files)
    index = index - 1
    local oldcount = mp.get_property_number("playlist-count", 1)
    for i = 1, #files do
        mp.commandv("loadfile", files[i], "append")
        mp.commandv("playlist-move", oldcount + i - 1, index + i - 1)
    end
end

function get_extension(path)
    match = string.match(path, "%.([^%.]+)$" )
    if match == nil then
        return "nomatch"
    else
        return match
    end
end

table.filter = function(t, iter)
    for i = #t, 1, -1 do
        if not iter(t[i]) then
            table.remove(t, i)
        end
    end
end

function find_and_add_entries()
    local path = mp.get_property("path", "")
    local dir, filename = mputils.split_path(path)
    if #dir == 0 then
        return
    end
    local pl_count = mp.get_property_number("playlist-count", 1)
    if (pl_count > 1 and autoload == nil) or
       (pl_count == 1 and EXTENSIONS[string.lower(get_extension(filename))] == nil) then
        return
    else
        autoload = true
    end

    local files = mputils.readdir(dir, "files")
    if files == nil then
        return
    end
    table.filter(files, function (v, k)
        local ext = get_extension(v)
        if ext == nil then
            return false
        end
        return EXTENSIONS[string.lower(ext)]
    end)
    table.sort(files, function (a, b)
        local len = string.len(a) - string.len(b)
        if len ~= 0 then -- case for ordering filename ending with such as X.Y.Z
            local ext = string.len(get_extension(a)) + 1
            return string.sub(a, 1, -ext) < string.sub(b, 1, -ext)
        end
        return string.lower(a) < string.lower(b)
    end)

    if dir == "." then
        dir = ""
    end

    local pl = mp.get_property_native("playlist", {})
    local pl_current = mp.get_property_number("playlist-pos", 0) + 1
    -- Find the current pl entry (dir+"/"+filename) in the sorted dir list
    local current
    for i = 1, #files do
        if files[i] == filename then
            current = i
            break
        end
    end
    if current == nil then
        return
    end

    local append = {[-1] = {}, [1] = {}}
    for direction = -1, 1, 2 do -- 2 iterations, with direction = -1 and +1
        for i = 1, MAXENTRIES do
            local file = files[current + i * direction]
            local pl_e = pl[pl_current + i * direction]
            if file == nil or file[1] == "." then
                break
            end

            local filepath = dir .. file
            if pl_e then
                -- If there's a playlist entry, and it's the same file, stop.
                if pl_e.filename == filepath then
                    break
                end
            end

            if direction == -1 then
                if pl_current == 1 then -- never add additional entries in the middle
                    mp.msg.info("Prepending " .. file)
                    table.insert(append[-1], 1, filepath)
                end
            else
                mp.msg.info("Adding " .. file)
                table.insert(append[1], filepath)
            end
        end
    end

    add_files_at(pl_current + 1, append[1])
    add_files_at(pl_current, append[-1])
end

mp.register_event("start-file", find_and_add_entries)

```

##### 快捷键配置：input.conf

``` bash
# mpv keybindings
# Location of user-defined bindings: ~/.config/mpv/input.conf
# Lines starting with # are comments. Use SHARP to assign the # key.
# Copy this file and uncomment and edit the bindings you want to change.
# List of commands and further details: DOCS/man/input.rst
# List of special keys: --input-keylist
# Keybindings testing mode: mpv --input-test --force-window --idle
# Use 'ignore' to unbind a key fully (e.g. 'ctrl+a ignore').
# Strings need to be quoted and escaped:
# KEY show-text "This is a single backslash: \\ and a quote: \" !"
# You can use modifier-key combinations like Shift+Left or Ctrl+Alt+x with
# the modifiers Shift, Ctrl, Alt and Meta (may not work on the terminal).
# The default keybindings are hardcoded into the mpv binary.
# You can disable them completely with: --no-input-default-bindings
# Developer note:
# On compilation, this file is baked into the mpv binary, and all lines are
# uncommented (unless '#' is followed by a space) - thus this file defines the
# default key bindings.
# If this is enabled, treat all the following bindings as default.
#------------------------------------------------------------------------------------------------
#default-bindings start
#------------------------------------------------------------------------------------------------
#MOUSE_BTN0 ignore                      # don't do anything
#MOUSE_BTN0_DBL cycle fullscreen        # toggle fullscreen on/off
#MOUSE_BTN2 cycle pause                 # toggle pause on/off
#MOUSE_BTN3 seek 10
#MOUSE_BTN4 seek -10
#MOUSE_BTN5 add volume -2
#MOUSE_BTN6 add volume 2

MOUSE_BTN0 show-progress
MOUSE_BTN1 cycle fullscreen
MOUSE_BTN3 add volume 5
MOUSE_BTN4 add volume -5

#------------------------------------------------------------------------------------------------
# Mouse wheels, touchpad or other input devices that have axes
# if the input devices supports precise scrolling it will also scale the
# numeric value accordingly
#------------------------------------------------------------------------------------------------
#AXIS_UP    seek 10
#AXIS_DOWN  seek -10
#AXIS_LEFT  seek 5
#AXIS_RIGHT seek -5
#------------------------------------------------------------------------------------------------
# Seek units are in seconds, but note that these are limited by keyframes
#------------------------------------------------------------------------------------------------
#RIGHT seek  5
#LEFT  seek -5
#UP    seek  60
#DOWN  seek -60

RIGHT seek 5
LEFT seek -5
UP add volume 5
DOWN add volume -5

#------------------------------------------------------------------------------------------------
# Do smaller, always exact (non-keyframe-limited), seeks with shift.
# Don't show them on the OSD (no-osd).
#------------------------------------------------------------------------------------------------
#Shift+RIGHT no-osd seek  1 exact
#Shift+LEFT  no-osd seek -1 exact
#Shift+UP    no-osd seek  5 exact
#Shift+DOWN  no-osd seek -5 exact
#------------------------------------------------------------------------------------------------
# Skip to previous/next subtitle (subject to some restrictions; see manpage)
#------------------------------------------------------------------------------------------------
#Ctrl+LEFT   no-osd sub-seek -1
#Ctrl+RIGHT  no-osd sub-seek  1
#PGUP add chapter 1                     # skip to next chapter
#PGDWN add chapter -1                   # skip to previous chapter
#Shift+PGUP seek 600
#Shift+PGDWN seek -600
#[ multiply speed 0.9091                # scale playback speed
#] multiply speed 1.1
#{ multiply speed 0.5
#} multiply speed 2.0
#BS set speed 1.0                       # reset speed to normal,BS=BackSpace
#q quit
#Q quit-watch-later
#q {encode} quit 4
#ESC set fullscreen no
#ESC {encode} quit 4
#p cycle pause                          # toggle pause/playback mode
#. frame-step                           # advance one frame and pause
#, frame-back-step                      # go back by one frame and pause
#SPACE cycle pause
#> playlist-next                        # skip to next file
#ENTER playlist-next                    # skip to next file
#< playlist-prev                        # skip to previous file
#O no-osd cycle-values osd-level 3 1    # cycle through OSD mode
#o show-progress
#P show-progress
#I show-text "${filename}"              # display filename in osd
#z add sub-delay -0.1                   # subtract 100 ms delay from subs
#x add sub-delay +0.1                   # add
#ctrl++ add audio-delay 0.100           # this changes audio/video sync
#ctrl+- add audio-delay -0.100
#9 add volume -2
#/ add volume -2
#0 add volume 2
#* add volume 2
#m cycle mute
#1 add contrast -1
#2 add contrast 1
#3 add brightness -1
#4 add brightness 1
#5 add gamma -1
#6 add gamma 1
#7 add saturation -1
#8 add saturation 1
#Alt+0 set window-scale 0.5
#Alt+1 set window-scale 1.0
#Alt+2 set window-scale 2.0
#------------------------------------------------------------------------------------------------
# toggle deinterlacer (automatically inserts or removes required filter)
#------------------------------------------------------------------------------------------------
#d cycle deinterlace
#r add sub-pos -1                       # move subtitles up
#t add sub-pos +1                       #                down
#v cycle sub-visibility
#------------------------------------------------------------------------------------------------
# stretch SSA/ASS subtitles with anamorphic videos to match historical
#V cycle sub-ass-vsfilter-aspect-compat
#------------------------------------------------------------------------------------------------
# switch between applying no style overrides to SSA/ASS subtitles, and
# overriding them almost completely with the normal subtitle style
#------------------------------------------------------------------------------------------------
#u cycle-values sub-ass-style-override "force" "no"
#j cycle sub                            # cycle through subtitles
#J cycle sub down                       # ...backwards
#SHARP cycle audio                      # switch audio streams
#_ cycle video
#T cycle ontop                          # toggle video window ontop of other windows
#f cycle fullscreen                     # toggle fullscreen
ENTER cycle fullscreen
#s screenshot                           # take a screenshot
#S screenshot video                     # ...without subtitles
#Ctrl+s screenshot window               # ...with subtitles and OSD, and scaled
#Alt+s screenshot each-frame            # automatically screenshot every frame
#w add panscan -0.1                     # zoom out with -panscan 0 -fs
#e add panscan +0.1                     #      in
#------------------------------------------------------------------------------------------------
# cycle video aspect ratios; "-1" is the container aspect
#------------------------------------------------------------------------------------------------
#A cycle-values video-aspect "16:9" "4:3" "2.35:1" "-1"
#POWER quit
#PLAY cycle pause
#PAUSE cycle pause
#PLAYPAUSE cycle pause
#STOP quit
#FORWARD seek 60
#REWIND seek -60
#NEXT playlist-next
#PREV playlist-prev
#VOLUME_UP add volume 2
#VOLUME_DOWN add volume -2
#MUTE cycle mute
#CLOSE_WIN quit
#CLOSE_WIN {encode} quit 4
#E cycle edition                        # next edition
#l ab-loop                              # Set/clear A-B loop points
#L cycle-values loop "inf" "no"         # toggle infinite looping
#ctrl+c quit 4
#------------------------------------------------------------------------------------------------
# Apple Remote section
#------------------------------------------------------------------------------------------------
#AR_PLAY cycle pause
#AR_PLAY_HOLD quit
#AR_CENTER cycle pause
#AR_CENTER_HOLD quit
#AR_NEXT seek 10
#AR_NEXT_HOLD seek 120
#AR_PREV seek -10
#AR_PREV_HOLD seek -120
#AR_MENU show-progress
#AR_MENU_HOLD cycle mute
#AR_VUP add volume 2
#AR_VUP_HOLD add chapter 1
#AR_VDOWN add volume -2
#AR_VDOWN_HOLD add chapter -1
#------------------------------------------------------------------------------------------------
# For tv://
#------------------------------------------------------------------------------------------------
#h cycle tv-channel -1                  # previous channel
#k cycle tv-channel +1                  # next channel
#------------------------------------------------------------------------------------------------
# For dvb://
#------------------------------------------------------------------------------------------------
#H cycle dvb-channel-name -1            # previous channel
#K cycle dvb-channel-name +1            # next channel
#------------------------------------------------------------------------------------------------
# Legacy bindings (may or may not be removed in the future)
#------------------------------------------------------------------------------------------------
#! add chapter -1                       # skip to previous chapter
#@ add chapter 1                        #         next
#------------------------------------------------------------------------------------------------
# Not assigned by default
# (not an exhaustive list of unbound commands)
#------------------------------------------------------------------------------------------------
# ? add sub-scale +0.1                  # increase subtitle font size
# ? add sub-scale -0.1                  # decrease subtitle font size

Alt+= add sub-scale 0.1
Alt+- add sub-scale -0.1

# ? sub-step -1                         # immediately display next subtitle
# ? sub-step +1                         #                     previous
# ? cycle angle                         # switch DVD/Bluray angle
# ? add balance -0.1                    # adjust audio balance in favor of left
# ? add balance 0.1                     #                                  right
# ? cycle sub-forced-only               # toggle DVD forced subs
# ? cycle program                       # cycle transport stream programs
# ? stop                                # stop playback (quit or enter idle mode)

```

---

## 拓展链接

### mpv 官方 git

- [戳我看 mpv 官方 git](https://github.com/mpv-player/mpv/blob/master/DOCS/man/options.rst "mpv 官方 git")

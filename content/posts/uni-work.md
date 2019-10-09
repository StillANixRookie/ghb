---
title: "Uni Work"
date: 2019-09-03T23:52:15+01:00
draft: false
menu:
  main:
    weight: 1
---

+ {{%attachments title="" pattern=".*(pdf|mp4)"/%}}
+ [Master's Group Project](http://uosdesign.org/designshow2019/project/seabug/)

# themer
- A script to change theme of configs using a theme file.
Inspired by budlabs' mondo-generator from the AUR.

## Usage

```
themer [--dark|-D|--auto-dark|-t] [--change|-c|--new|-n] <name>
themer [--getvar|-v] <variable>
themer [--list|-l] themes|vars|vars-rofi|defaults
themer [--force|-f] [--apply-single|-a] <directory>
themer [--apply-all|-A|--updatelist|-u]
themer --help|-h
themer
```

## Options

+ Run with no options to create defaults in:
	+ $THEMER_DIR = $HOME/.config/themer

+ `--change|-c <theme>`
	+ Change current theme to `<theme>`.
	If `<theme>` is empty, a rofi list of themes will be displayed.
	Requires my i3menu rofi wrapper.

+ `--apply-single|-a <directory>`
	+ Apply current theme to `<directory>`.

+ `--apply-all|-A`
	+ Apply current theme to directories listed in $THEMER_DIR/.dirs.

+ `--force|-f`
	+ Use with --apply-(single|all) to force apply a theme, since themes
	are not automatically generated from 'template_' files to speed up
	applying the theme

+ `--getvar|-v <variable>`
	+ Get `<variable>` from current theme.

+ `--new|-n <name>`
	+ Create theme `<name>`.

+ `--list|-l themes|vars|vars-rofi|defaults`
	+ themes = list themes in $THEMER_DIR.
	+ defaults = list default variables
	+ vars = list vars of current theme.
	+ vars-rofi = rofi list of variables in current theme.

+ `--updatelist|-u`
	+ Update rofi theme list for -c.

+ `--dark|-D`
	+ Dark mode (switches fgo and bg0).

+ `--auto-dark|-t <am_time>:<pm_time>`
	+ Dark/Light mode based on time of day.
	+ `<am_time>` hr to apply 'light' mode.
	+ `<pm_time>` hr to apply 'dark' mode.
	+ Uses 24-hr time format

+ `--help|-h`
	+ Display this help.


## Making Themes

### My Default theme

```console
$ themer --list defaults
name           default
guifont        $romanfont
font           CMUTypewriterText
sansfont       Lucida Sans
monofont       CMU Typewriter Text
romanfont      Luxi Mono
ltimex         1
ltimey         90
size           11
rxvtsize       12
rofifsize      12
gtk            Standard-light
gtk-icon       Tela-red-dark
cursor         Bibata_Dark_Red
conky_loc1     top_left
conky_loc2     bottom_right
acc            #285577
bg0            #CDCBCD
fg0            #302f30
red            #cc241d
ylw            #904800
blu            #004890
grn            #009048
cyn            #009090
gnt            #900048
rng            #d65d0e
```

### Create New Theme

Using themer -n will prompt to create the required
fg0 [foreground], bg0 [background] and acc [accent]
colour variables.

The rest of the variables will be taken from the
$THEMER_DIR/.default theme, created automatically,
when the theme is generated. The .default theme
can be changed as you wish.

You can also reference variables using a '$' in front
of the variable name in the theme file and you can
also mix colours.

```console
$ cat ~/.config/themer/dosnerd
  name      dosnerd
  bg0       #DFBF9F
  acc       #d5715e
  fg0       #1F1F1F
  blu       $acc
  mix       colmix 33:$bg0 66:$acc
```

## Generating Themes

```
themer --change <theme>
```

to change the current theme and generate all variables,
including fg0, bg0, and acc variations.

Add `--dark` or `--auto-dark <am_time>:<pm_time>` to
convert <theme> to a dark theme (just swaps `fg0` and `bg0`
and renames the theme to `<name>-dark`),
or uses light/dark version based on time of day, respectively.

Use `themer --list vars` or `themer --list vars-rofi` to see
all the variables of the current theme.

## Making Theme Template

For example, an `.Xresources` theme:

```console
$ cd $HOME
$ mkdir .themer
$ cp .Xresources .themer/template_.Xresources
$ tree $HOME
  /home/user
  └── .themer
      └── template_.Xresources
```

Replace all colours or other things with the variables of
the current theme to get the `.Xresources` template below.

```console
$ cat ~/.themer/template_.Xresources
!! vim:ft=xdefaults

#define fontname  %%font%%
#define fontsize  %%rxvtsize%%
#define fontsize2 %%rxvtsize%%

!! >> default colours

#define cwhtl #302f30
#define cwhtd #7a90a2
#define credd #900000
#define credl #601718
#define cgrnd #009048
#define cgrnl #185f3c
#define cylwd #904800
#define cylwl #603b18
#define cblud #285577
#define cblul #2c4253
#define cgntd #900048
#define cgntl #60173c
#define ccynd #009090
#define ccynl #185f60
#define cblkd #CDCBCD
#define cblkl #b9b7b9

!! >> themer colours: %%name%%

#define cwhtl %%fg0%%
#define cwhtd %%acb%%
#define credd %%red%%
#define credl %%red%%
#define cgrnd %%grn%%
#define cgrnl %%grn%%
#define cylwd %%ylw%%
#define cylwl %%ylw%%
#define cblud %%blu%%
#define cblul %%blu%%
#define cgntd %%gnt%%
#define cgntl %%gnt%%
#define ccynd %%cyn%%
#define ccynl %%cyn%%
#define cblkd %%bg0%%
#define cblkl %%bg1%%

*background:             cblkd
*foreground:             cwhtl
*fading:                 30
*fadeColor:              black
*cursorColor:            credd
*pointerColorBackground: cblkd
*pointerColorForeground: ccynd
*border:                 cblkd

*color0:                 cblkd
*color1:                 credd
*color2:                 cgrnd
*color3:                 cylwd
*color4:                 cblud
*color5:                 cgntd
*color6:                 ccynd
*color7:                 cwhtd
*color8:                 cblkl
*color9:                 credl
*color10:                cgrnl
*color11:                cylwl
*color12:                cblul
*color13:                cgntl
*color14:                ccynl
*color15:                cwhtl
```

The variables from the current theme can be listed using
`themer --list vars`, and are to be referenced with:
`%%<variable>%%`.

Add RGB after a colour name to get its RGB value
in "R, G, B" form eg. `%%bg0RGB%%`.

Use `@@color@@` instead of `%%color%%` to get hex color
without '#'. Useful for `#AARRGGBB` colours.

Create .themer_pre and .themer_post bash scripts in
the `<directory>` to eg. reload the .Xresources file etc.
before/after applying a theme. These scripts have the
current theme name, eg. `dosnerd`, as their first argument.
If they are not present, then the file created by applying
the theme is just copied into the specified `<directory>`.

```
themer -a $HOME
```

to apply the theme to each template in `$HOME/.themer`.

If the theme is dosnerd, this will result in the following
tree structure in `$HOME`:

```console
$ cd $HOME
$ tree $HOME
  /home/user
  ├── .themer
  │   ├── dosnerd
  │   │   └── .Xresources
  │   ├── dosnerd-dark
  │   │   └── .Xresources
  │   └── template_.Xresources
  ├── .themer_pre
  └── .themer_post
```

```
themer --apply-all
```

to apply theme to all directories listed in
$THEMER_DIR/.dirs


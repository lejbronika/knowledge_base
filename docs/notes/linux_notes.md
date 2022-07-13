---
tags:
  - linux
---

# Linux Notes

## Commands usage and examples

### `rsync`

`rsync [OPTION...] SRC... [DST...]`

#### Flags

- `a`: archive mode, or faster way to specify recursion and necessity of preserving everything. 
- `z`: compress file data during transfer.
- `h`: human readable.
- `v`: verbose.

#### Examples

- find by date and sync:

```
sudo find SRC -type f -newermt DATE_BEGIN ! -newermt DATE_END > files_to_sync.txt

sudo rsync -avP `cat files_to_sync.txt` DST

sudo rsync -avP USER@HOST:SRC DST
```



## VA

### Make file executable

To make file executable use this command:

```
chmod u+x <file>
```

### Add opacity to Lxterminal

- Install [Compton](https://github.com/chjj/compton)
```
sudo apt-get install compton
```
- Use `xprop` command to get necessary app window properties.
- Search for `WM_CLASS(STRING) = APP_NAME` property.
- Use found name in `class_g` argument.
- Add opacity settings for  `~/.config/compton.conf`:
```	
sudo 'opacity-rule = ["75:class_g *= 'APP_NAME'"]' > ~/.config/compton.conf
```
- Add `compton` to `~/.config/lxsession/Lubuntu/autostart`
```
sudo 'compton' > ~/.config/lxsession/Lubuntu/autostart
```

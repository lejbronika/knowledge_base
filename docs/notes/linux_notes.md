# Linux notes

## Add opacity to Lxterminal

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

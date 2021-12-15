### Gnome 3+

To change a Gnome background one can <br>
simply utilize the `gsettings` command.

```shell
gsettings <Action> <Schema/Id> <Arguments>
```

For settings properties:

```shell
gsettings set <Id> <Key> <Value>
```

###### Gnome Properties:

| Property | Value | Description |
|:--------:|:-----:|:------------|
| `picture-uri` | `URI` | The **URI** of the background. |
| `picture-options` | `none` `wallpaper` <br> `centered` `scaled` <br> `stretched` `zoom` <br> `spanned` | Rendering behaviour |
| `color-shading-type` | `horizontal` <br> `vertical` `solid` | Shading direction |
| `primary-color` | `#023c88` | **Top** / **Left** gradient shading color |
| `secondary-color` | `#5789ca` | **Bottom** / **Right** gradient shading color |

###### Setting Keys:

| **Background** | `org.gnome.desktop.background` |
|:-:|:-:|
| **Screensaver** | `org.gnome.desktop.screensaver` |

###### Example

```shell
Wallpaper="..."

gsettings set org.gnome.desktop.background picture-uri "file://$Wallpaper"
gsettings set org.gnome.desktop.background picture-options 'wallpaper'
gsettings set org.gnome.desktop.background color-shading-type 'solid'
```

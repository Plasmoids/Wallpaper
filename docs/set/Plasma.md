### Plasma 5.7+

Using the **DBus** one can execute **Javascript** <br>
in the **PlasmaShell** process.

```shell
<DBus>              \
    <Service>       \
    <Path>          \
    <Method>        \
    <Arguments>
```

Depending on your system you may need a <br>
different dbus command, here **QDBus** is used:

```shell
qdbus                                   \
    org.kde.plasmashell                 \
    /PlasmaShell                        \
    org.kde.PlasmaShell.evaluateScript  \
    "<Javascript Code>"
```

With the help of the `desktops` function one can <br>
retrieve **All Desktop Configurations**, that includes <br>
configurations for currently disconnected devices <br>
and thus modify their properties.

```js
desktops() // Returns an array of desktop objects
```

Now one can utilize a **Plasma Wallpaper Plugin** <br>
like the **default** `org.kde.image` plugin to actually <br>
set the wallpaper.

<br>

*In this example the first* ***connected*** *desktop object is used*

First, set the **Wallpaper Plugin** used for the desktop.

```js
desktop.wallpaperPlugin = 'org.kde.image';
```

<br>

Second, select the storage group of the desktops configuration.

```js
desktop.currentConfigGroup = [ 'Wallpaper' , 'org.kde.image' , 'General' ];
```

<br>

`org.kde.image` uses this containment group to save <br>
the image path used for your desktops wallpaper.

*Other plugins may use different groups.*

Third, write the path into the data group.

```js
desktop.writeConfig('Image',`file://${ path }`);
```

<br>

*A possible location for the data file can be:* <br>
*`~/.config/plasma-org.kde.plasma.desktop-appletsrc`*

<br>

**If the command was successful you** <br>
**should see an immediate change.**

<br>

Now for a combined version that selects all<br>
actively used desktop configurations and <br>
sets their background to the given path.

```js
const path = '...';

for(const desktop of desktops().filter(onlyActive))
    changeWallpaper(desktop,path);

function onlyActive(desktop){
    return desktop.screen != -1;
}

function changeWallpaper(desktop,path){
    desktop.wallpaperPlugin = 'org.kde.image';
    desktop.currentConfigGroup = [
        'Wallpaper' , 'org.kde.image' , 'General' ];
    desktop.writeConfig('Image',`file://${ path }`);
}
```

Lastly written as a shell script with a shell <br>
variable as direct insertion into the JS code.

```shell
wallpaper="..."

qdbus                                   \
    org.kde.plasmashell                 \
    /PlasmaShell                        \
    org.kde.PlasmaShell.evaluateScript  \
    "
        for(const desktop of desktops().filter(onlyActive))
            changeWallpaper(desktop);

        function onlyActive(desktop){
            return desktop.screen != -1;
        }

        function changeWallpaper(desktop){
            desktop.wallpaperPlugin = 'org.kde.image';
            desktop.currentConfigGroup = [
                'Wallpaper' , 'org.kde.image' , 'General' ];
            desktop.writeConfig('Image','file://$wallpaper');
        }
    "
```

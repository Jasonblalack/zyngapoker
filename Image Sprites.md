# Image Sprites

Image sprites can be configured using the file `jasysprite.json` inside the asset folder of the application. For better structure this data could be split up into multiple `jasysprite.json` files (e.g. place them in sub folders).

## Manually defining sprite sheets

A typical `jasysprite.json` looks like:

```js
{
  "sprite1.png" : {
    "icon/copy.png" : {
      "left" : 0,
      "top" : 0
    },
    
    "icon/cut.png" : {
      "left" : 40,
      "top" : 0
    },

    "icon/paste.png" : {
      "left" : 20,
      "top" : 0
    },
    
    "splash.png" : {
      "left" : 0,
      "top": 20,
      "width" : 200,
      "height" : 200
    }
  }
}
```

This is the corresponding file system layout:

```
- asset/
  - jasysprite.json
  - splash.png
  - sprite1.png
  - icon/
    - copy.png
    - cut.png
    - paste.png
```

Normally you place the single images e.g. `copy.png` inside your repository as well. If that's not the case you should define the dimensions of the images as well:

```js
{
  "sprite1.png" : {
    "icon/copy.png" : {
      "left" : 0,
      "top" : 0,
      "width" : 20,
      "height" : 20
    },
    
    "icon/cut.png" : {
      "left" : 40,
      "top" : 0,
      "width" : 20,
      "height" : 20
    },
    
    "icon/paste.png" : {
      "left" : 20,
      "top" : 0,
      "width" : 20,
      "height" : 20
    },
    
    "splash.png" : {
      "left" : 0,
      "top": 20,
      "width" : 200,
      "height" : 200
    }
  }
}
```

As soon as the `jasysprite.json` in placed inside the asset folder these sprite images are used. There is no further configuration needed.

## Auto Generating Sprite Sheets

There is an API in Jasy to automatically generate sprite sheets. You can define a specific tasks which packs single images into sprite images completely automatically. This is pretty useful because automatically packaging these files is typically more efficient than doing it by hand.

Note: The sprite packer currently supports PNG files only.

```python
@task
def sprites():
  packer = SpritePacker("source/asset")
  packer.packDir("icon")
  packer.packDir("settings")
  packer.packDir("help")
```

These calls generate three sprite sheets (`jasysprite.json` with any number if `jasysprite_xx.png` files) from the images found in the folders `source/asset/icon`, `source/asset/settings` and `source/asset/help`. 

During the project phases layouts might change dramatically. It's worth adding a call to `clear` first to delete all existing image sprite files. Keep in mind that this deletes custom added files as well!

```python
@task
def sprites():
  packer = SpritePacker("source/asset")
  packer.clear()
  packer.packDir("icon")
  packer.packDir("settings")
  packer.packDir("help")
```


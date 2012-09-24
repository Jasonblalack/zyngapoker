Jasy allows to store information about image based animation directly with your assets. This information is automatically collected by the asset system and deployed to the exported asset data so that client side code has access to this valuable information. Placing this information directly with the assets has the benefit to keep related information together instead of in two different places.

Animations can be configured using the file `jasyanimation.yaml(-.json)` inside the asset folder of the application. 

## Manually defining animation sheets

A typical `jasyanimation.json` looks like:

```json
{
	"brickbreak.png" : {
		"columns": 4,
		"rows": 4,
		"frames": 13
	},

	"explosion.png" : {
		"columns": 25
	},

	"rocks/rockexplosion.png" : {
		"columns": 10
	}
}

```
Animations are usually stored in sprite-sheets. Use one sheet for one animation. With "columns" and "rows" you're able to define the grid settings of your sheet, "frames" defines how much sprites your animation sheet includes.


This is the corresponding file system layout:

```
- asset/
  - jasyanimation.json
  - brickbreak.png
  - explosion.png
  - rocks/
    - rockexplosion.png
```

As soon as the `jasyanimation.yaml(-.json)` in placed inside the asset folder these animation is useable. There is no further configuration needed.

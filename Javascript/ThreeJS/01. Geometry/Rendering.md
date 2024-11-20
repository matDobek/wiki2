### What is `depth buffer`?

* `dept buffer` also called `z-buffer` 
* is a `2D` array that matches the resolution (for example: `1920 x 1080` pixels)
* each pixel stores `normalized` ( 0 - 1 ) `distance` between `camera` and closes `object`
* `0` closes, `1` furthest

### What is `depth test`?
* `z-buffer` is initialized with `1` ( furthest value)
* we iterate over each `object` in the `scene`
	* calculate `distance` between every `pixel` covered by the `triangle` and the `camera`
	* compare the `distance` with corresponding value in `z-buffer`
		* if `distance` of the `pixel` of the `object` is closer to `camera` than the one in `z-buffer`
			* update `z-buffer` with new value
			* paint the pixel
		* discard ( do not render ) otherwise

### Challenges of `depth test`?
* precision issues - for large `range` covered by the `camera`, there may be some `z-fighting` for `objects` close to one another
	* tweak `camera.far` and `camera.near` distance to fix this issue

* transparency issues:
	* displaying `transparent sphere` inside a `transparent cube`
	* `sphere` is closer to the `camera` ( `cube` should be rendered first )
	- [ ] TODO - feels like writing to `z-buffer` is enabled for `transparency` objects

```js
// 1. set renderOrder for cube 
// 2. position sphere further away from the camera
// 3. depthWrite: false, for bigger sphere
// 4. depthTest: false, for smaller sphere
```

### How objects are rendered?

* `frustum culling` - discard all objects not in camera view
*  sort `opaque` / `transparent`  `objects` by `centroid` distance to `camera`
	* `centroid` - average position of all `vertices`
	* `opaque` - front-to-back
	* `transparent` - back-to-front
* sort by `renderOrder` (from lowest to highest)
* paint `opaque` objects by writing to `z-buffer` and performing  `depth test`
* paint `transparent` objects performing `depth test` (no writing to `z-buffer`)
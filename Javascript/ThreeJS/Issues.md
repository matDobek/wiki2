
### GPU memory leaks

```js
const box
// ...

box.geomentry = new BoxGeometry(...)
box.geometry.dispose()
box.geomentry = new BoxGeometry(...)
```

### `camera.LookAt` doesn't work
Make sure `OrbitControls` are not interfering - they will reset `camera` settings.
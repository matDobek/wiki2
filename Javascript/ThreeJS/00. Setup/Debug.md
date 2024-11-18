
```bash
npm install lil-gui
```

```js
import GUI from "lil-gui"

const gui = new GUI({
  width: 300,
  title: "Debug UI",
  closeFolders: false
})

debugObject = {
	positionX: 0,
	positionY: 0,
	color: 0xff0000
}

gui.add(debugObject, "positionX")
gui.add(debugObject, "positionY")
	.min(-10)
	.max(10)
	.step(1)
	.name("Position Y")
	.onChange(         (value) => {} )
	.onFinishedChange( (value) => {} )
gui.addColor(debugObject, "color")

gui.add(camera.position, "x").listen().disable()

folder = gui.addFolder("foobar")
folder.add(...)
```

```js
gui.hide()
window.addEventListener("keydown", (event) => {
  if(event.key == 'h') {
    gui.show(gui._hidden)
  }
})
```
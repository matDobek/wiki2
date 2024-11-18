
```js
const image = new Image()
const texture = new THREE.Texture(image)
texture.colorSpace = THREE.SRGBColorSpace

image.onload = () => {
  texture.needsUpdate = true
}
image.src = "/textures/door/color.jpg"

const geometry = new THREE.BoxGeometry(1, 1, 1)
const material = new THREE.MeshBasicMaterial({ map: texture })
const box = new THREE.Mesh(geometry, material)
box.position.set(0, 0, 0);
scene.add(box)
```

```js
const textureLoader = new THREE.TextureLoader()
const colorTexture = textureLoader.load(
  // "src", load, progress, error
  "/textures/door/color.jpg",
  () => { console.log("loaaded") },
  () => { console.log("progress") },
  () => { console.log("error") },
)
colorTexture.colorSpace = THREE.SRGBColorSpace
```

```js
const loadingManager = new THREE.LoadingManager()

loadingManager.onStart = () => {
  console.log("LoadingManager: loading started")
}
loadingManager.onLoad = () => {
  console.log("LoadingManager: loading finished")
}
loadingManager.onProgress = () => {
  console.log("LoadingManager: progress")
}
loadingManager.onError = () => {
  console.log("LoadingManager: loading error")
}

const textureLoader = new THREE.TextureLoader(loadingManager)
const colorTexture = textureLoader.load("/textures/door/color.jpg")
colorTexture.colorSpace = THREE.SRGBColorSpace
```

```js
colorTexture.repeat.x = 2
colorTexture.repeat.y = 3

colorTexture.wrapS = THREE.RepeatWrapping
colorTexture.wrapT = THREE.RepeatWrapping

colorTexture.wrapS = THREE.MirroredRepeatWrapping
colorTexture.wrapT = THREE.MirroredRepeatWrapping

colorTexture.offset.x = 0.5
colorTexture.offset.y = 0.5

colorTexture.rotation = Math.PI * 0.25
colorTexture.center.x += 0.5
colorTexture.center.y += 0.5
```


```js
checkerboard8.minFilter = THREE.NearestFilter
checkerboard8.magFilter = THREE.LinearFilter
```

![[Pasted image 20241116050010.png]]
### What is `vertex`, `edge` and `face`?
`vertex` - corner / points where 3 edges meet 
`edge` - straight line between `vertices`
`face` - flat surface on the shape

![[Pasted image 20241118134814.png]]
![[Pasted image 20241118134627.png]]
![[Pasted image 20241118134822.png]]

In `ThreeJS` faces are usually triangles.

### What is `uv mapping`?

`uv mapping` is a process of unwrapping 3d model into 2d space, so textures could be applied properly.
Each vertex in 3d model is assigned to `uv coordinate`.

`UV` stands for:
* `u` horizontal axis
* `v` vertical axis
* avoid confusion with 3D coordinates: `xyz`

Output of a `uv mapping` are `uv coordinates`.
Which in turn are used for `renderer` to place 2d `textures` into 3d object properly.

![[Pasted image 20241118141454.png]]

### What is `bounding` `box`/`sphere`?
Simplified geometrical representation.
It's an area wrapped around the object, to simplify:
* visibility checks
* collision detection
* lighting computation

![[Pasted image 20241118143130.png]]

In `ThreeJS`:

```js
// https://threejs.org/docs/index.html?q=geom#api/en/core/BufferGeometry.boundingBox

BufferGeometry.computeBoundingBox()
BufferGeometry.boundingBox

// Bounding box for the bufferGeometry, which can be calculated with .computeBoundingBox(). Default is `null`.

BufferGeometry.computeBoundingSphere()
BufferGeometry.boundingSphere

// Bounding sphere for the bufferGeometry, which can be calculated with .computeBoundingSphere(). Default is null.
```


 Example:

```js
//
// bounding box helper
//
const boundingBox = new THREE.BoxHelper(sphere, 0xff0000)
// boundingBox.update()
scene.add(boundingBox)

//
// bounding sphere
//
box.geometry.computeBoundingSphere()
const boundingSphere = new THREE.Mesh(
  new THREE.SphereGeometry(box.geometry.boundingSphere.radius, 10, 10),
  new THREE.MeshBasicMaterial({color: 0xff0000, wireframe: true})
)

boundingSphere.position.copy(box.position)
boundingSphere.position.add(box.geometry.boundingSphere.center)

scene.add(boundingSphere)
```

### What is `normal vector`?

Defines which direction `face` is pointing to.
`normal vector` is perpendicular to the surface.

![[Pasted image 20241118140023.png]]
> A normal to a surface at a point is the same as a normal to the tangent plane to the surface at the same point.

`normals` are use to compute lighting effects.
For example:
* compute the angle between face surface and the light source
	* bright light when facing directly the light
	* dimmer if under angle

### How to construct custom shape?

```js
THREE.BufferGeometry()
THREE.BufferAttribute()

const vertices
const indices
```

```js
const geometry = new THREE.BufferGeometry()
const vertices = new Float32Array([
  // base verticies ( square )
  -1, 0, -1, // 0 top left
  -1, 0, 1,  // 1 bottom left
  1, 0, 1,   // 2 bottom right
  1, 0, -1,  // 3 top right

  0, 1, 0,   // 4 apex
])

const indices = [
  2, 4, 1, // front face
  3, 4, 2, // right face
  0, 4, 3, // back face
  1, 4, 0, // left face

  // base face
  1, 0, 2,
  0, 3, 2,
]

geometry.setAttribute("position", new THREE.BufferAttribute(verices, 3))
geometry.setIndex(indices)
geometry.computeVertexNormals() // compute normals for lighting
```

* `BufferAttribute` as second argument takes number of elements that should be associated with single `vertex`
* Notice that there are different `type` requirements for `vertices` and `indices`.

- [*] Why lighting does not interact with my shape?

![[Pasted image 20241118193632.png]]
![[Pasted image 20241118193652.png]]
We do need to define `normals` for our `faces`. 
We can try compute them automatically:
```js
geometry.computeVertexNormals()
```


- [*] Why not all faces are displayed?

![[Pasted image 20241118194128.png]]
![[Pasted image 20241118194153.png]]
By default only one side will be visible. 
For debugging it may also be worth to enable:
```js
const material = new THREE.MeshStandardMaterial({ color: 0xffffff, side: THREE.DoubleSide // Disables backface culling (renders both sides) });
```

Order of `indices` in `face` definition determines which side is visible. In order to define visible side put `indices` in `counter-clockwise` order:

```js
const vertices = new Float32Array([
	// left vertex
	// right vertex
	// top vertex
])

const indices = new [
    2, 1, 0
]
```

![[Pasted image 20241118195304.png]]


### Mesh vs Geometry

```js
object3d

mesh.position
mesh.rotation
mesh.scale
mesh.lookAt
mesh.visible
mesh.receiveShadow
mesh.castShadow

geometry.translate
geometry.lookAt / rotateX / Y / Z
geometry.scale
geometry.boundingBox
geometry.boundingSphere
geometry.center
```
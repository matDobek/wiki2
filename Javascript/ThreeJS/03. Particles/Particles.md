### tl;dr
```js
const material = new THREE.PointsMaterial({
  size: 0.2,
  sizeAttenuation: true,
  transparent: true,
  alphaMap: texture,
  alphaTest: 0.001, // do not render opacity is lower than this value
  // depthTest: false,
  // depthWrite: false
  // blending: THREE.AdditiveBlending
})

const particles = new THREE.Points(geometry, material)
```
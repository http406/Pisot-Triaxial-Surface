# Pisot-Triaxial-Surface

### **What is the Pisot Triaxial Surface?**

The **Pisot triaxial surface** is a type of **parametric surface** that appears in mathematical visualization. It is generated using trigonometric functions based on Pisot numbers (a class of algebraic numbers with special properties). The triaxial nature refers to the three coordinate equations that define the surface in terms of two parameters \( u \) and \( v \). 

These surfaces can create **beautiful and complex 3D shapes** that are useful in **mathematical modeling, graphics, and visualizations**.

---

## **Understanding the Equation Used in the Code**

In the given code, the coordinates \( (x, y, z) \) of the surface are defined by these parametric equations:

![Image](https://github.com/user-attachments/assets/9bf340d8-177b-4ec1-b842-01f96f4d34f2)

### **Breakdown of Each Equation:**
1. **Cosine-based Deformation**:  
   - The use of **cosine functions** creates a smooth, wave-like shape.
   - The phase shifts (**e.g., 1.03002, 1.40772, 2.43773**) adjust the orientation and curvature.
   - Multiplying by constants (e.g., **0.655866, 0.71878, 0.868837**) scales the surface.

2. **Triaxial Structure**:  
   - Each equation depends on both **u and v**, meaning that changes in either parameter affect all three coordinates.
   - The trigonometric dependencies make it **non-trivial** and **asymmetrical**, producing a unique shape.

3. **Embedded 3D Form**:  
   - The structure depends on **nested cosine functions**, causing a **layered, intricate appearance**.

---

## **Explanation of the Code**
The code implements this **Pisot triaxial surface** in **Three.js**, a JavaScript library for 3D graphics.

### **1. Setting Up the Scene**
```js
let scene, camera, renderer;
scene = new THREE.Scene();
camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 5;
renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```
- **`scene`**: The 3D world where objects exist.
- **`camera`**: Captures the view (set to perspective mode with a field of view of 75 degrees).
- **`renderer`**: Draws the scene in a `canvas` element.

---

### **2. Generating the Pisot Triaxial Surface**
```js
const points = [];
const colors = [];
const uStep = 0.1, vStep = 0.1;

for (u = 0; u <= 2 * Math.PI; u += uStep) {
    for (v = 0; v <= 2 * Math.PI; v += vStep) {
        const x = 0.655866 * Math.cos(1.03002 + u) * (2 + Math.cos(v));
        const y = 0.71878 * Math.cos(1.40772 - u) * (2 + 0.868837 * Math.cos(2.43773 + v));
        const z = 0.868837 * Math.cos(2.43773 + v) * (2 + 0.405098 * Math.cos(0.377696 - v));
        points.push(x, y, z);

        const color = new THREE.Color();
        color.setHSL((u / (2 * Math.PI) + v / (2 * Math.PI)) % 1, 1.0, 0.5);
        colors.push(color.r, color.g, color.b);
    }
}
```
### **What This Does:**
- **Loops over u and v**:  
  - `u` and `v` vary from **0 to \( 2\pi \)** in steps of `0.1` (fine resolution).
- **Computes \( x, y, z \)** based on the parametric equations.
- **Pushes these points into an array** for later visualization.
- **Assigns colors using HSL** (Hue-Saturation-Lightness):
  - The color depends on the values of `u` and `v` to create a **mesmerizing gradient**.

---

### **3. Creating and Rendering the Points**
```js
geometry = new THREE.BufferGeometry();
geometry.setAttribute('position', new THREE.Float32BufferAttribute(points, 3));
geometry.setAttribute('color', new THREE.Float32BufferAttribute(colors, 3));

material = new THREE.PointsMaterial({ size: 0.1, vertexColors: true });
mesh = new THREE.Points(geometry, material);
scene.add(mesh);
```
- **Stores vertex positions** in `geometry`.
- **Adds colors** using `vertexColors: true`.
- **Creates a Points object** (`THREE.Points`) and adds it to the scene.

---

### **4. Adding a Space Background with Glittering Stars**
```js
function addStars() {
    const starGeometry = new THREE.BufferGeometry();
    const starVertices = [];
    const starColors = [];
    
    for (let i = 0; i < 1000; i++) {
        const x = (Math.random() - 0.5) * 200;
        const y = (Math.random() - 0.5) * 200;
        const z = (Math.random() - 0.5) * 200;
        starVertices.push(x, y, z);

        const color = new THREE.Color();
        color.setHSL(Math.random(), 1.0, 0.8);
        starColors.push(color.r, color.g, color.b);
    }

    starGeometry.setAttribute('position', new THREE.Float32BufferAttribute(starVertices, 3));
    starGeometry.setAttribute('color', new THREE.Float32BufferAttribute(starColors, 3));

    const starMaterial = new THREE.PointsMaterial({ size: 0.2, vertexColors: true });
    const stars = new THREE.Points(starGeometry, starMaterial);
    scene.add(stars);
}
```
### **What This Does:**
- **Generates 1000 stars** randomly positioned in space.
- **Applies random colors** to create a vibrant, **galaxy-like effect**.
- **Adds them to the scene**, making the background more engaging.

---

### **5. Animation Loop**
```js
function animate() {
    requestAnimationFrame(animate);
    mesh.rotation.x += 0.01;
    mesh.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```
- **Rotates the shape** over time.
- **Continuously re-renders** to animate the scene.

---

## **Final Words**
- **The Pisot Triaxial Surface is a complex shape defined by cosine functions.**
- **The code uses Three.js to visualize it in 3D.**
- **A colorful gradient effect is applied based on its parametric values.**
- **A starry background is added for a mesmerizing space-like effect.**

This **combination of mathematics, graphics, and animation** makes for a **stunning and interactive visualization!** 🚀✨

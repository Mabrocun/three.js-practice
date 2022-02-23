<template>
  <div ref="canvas" />
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

export default {
  name: "TheCanvas",
  data: function () {
    this.scene = new THREE.Scene();

    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshStandardMaterial({
      side: THREE.FrontSide,
      color: "hsl(140, 45%, 50%)",
      wireframe: false,
    });
    this.cube = new THREE.Mesh(geometry, material);

    this.camera = new THREE.PerspectiveCamera(
      80,
      window.innerWidth / window.innerHeight,
      1,
      1000
    );
    this.renderer = new THREE.WebGLRenderer({ antialias: true });
    var controls = new OrbitControls(this.camera, this.renderer.domElement); //创建控件对象
    controls.addEventListener("change", this.render);
    this.light = new THREE.DirectionalLight("hsl(0, 100%, 100%)");

    this.speedVector = [0, 0];
    return {};
  },
  created: function () {
    this.scene.add(this.camera);
    this.scene.add(this.light);
    this.scene.add(this.cube);
    this.renderer.setSize(window.innerWidth, window.innerHeight);
    this.light.position.set(10, 10, 10);
    this.camera.position.z = 10;
    this.cube.position.x = -15;
    this.scene.background = new THREE.Color("hsl(0, 100%, 100%)");
  },
  mounted: function () {
    this.$refs.canvas.appendChild(this.renderer.domElement);
    this.animate();
    this.addKeyboardListener();
  },
  methods: {
    animate: function () {
      requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
      this.cube.rotation.y += 0.01;
      this.cube.rotation.x += 0.01;
      if (Math.abs(this.cube.position.x + this.speedVector[0]) < 16)
        this.cube.position.x += this.speedVector[0];
      if (Math.abs(this.cube.position.y + this.speedVector[1]) < 6)
        this.cube.position.y += this.speedVector[1];
    },
    addKeyboardListener() {
      document.addEventListener(
        "keydown",
        (e) => {
          const ev = e || window.event;
          switch (ev.keyCode) {
            case 87:
              this.speedVector[1] = 0.3;
              break;
            case 83:
              this.speedVector[1] = -0.3;
              break;
            case 65:
              this.speedVector[0] = -0.3;
              break;
            case 68:
              this.speedVector[0] = 0.3;
              break;
            default:
              break;
          }
        },
        false
      );
      document.addEventListener(
        "keyup",
        (e) => {
          const ev = e || window.event;
          switch (ev.keyCode) {
            case 87:
              this.speedVector[1] = 0;
              break;
            case 83:
              this.speedVector[1] = 0;
              break;
            case 65:
              this.speedVector[0] = 0;
              break;
            case 68:
              this.speedVector[0] = 0;
              break;
            default:
              break;
          }
        },
        false
      );
    },
  },
};
</script>

<style scoped>
#three {
  width: 100%;
  height: 95%;
  position: fixed;
  left: 0;
  top: 5%;
}
</style>

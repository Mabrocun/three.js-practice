<template>
  <div ref="canvas" />
  <h1 v-show="isCrashed" class="msg">Crashed!</h1>
  <h1 v-show="isSuccess" class="msg">Success!</h1>
  <button v-show="isGameOver" @click="retry">Retry</button>
</template>

<script>
import * as THREE from "three";

export default {
  name: "TheCanvas",
  data: function () {
    this.scene = new THREE.Scene();

    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const geometry2 = new THREE.BoxGeometry(2, 2, 2);
    const material = new THREE.MeshStandardMaterial({
      side: THREE.FrontSide,
      color: "hsl(140, 45%, 50%)",
      wireframe: false,
    });
    const material2 = new THREE.MeshStandardMaterial({
      side: THREE.FrontSide,
      color: "hsl(0, 100%, 50%)",
      wireframe: false,
    });
    this.cube = new THREE.Mesh(geometry, material);
    this.cars = [];
    for (let i = 0; i < 7; i++) {
      this.cars.push(new THREE.Mesh(geometry, material2));
    }
    this.camera = new THREE.PerspectiveCamera(
      80,
      window.innerWidth / window.innerHeight,
      1,
      1000
    );
    this.renderer = new THREE.WebGLRenderer({ antialias: true });
    this.light = new THREE.DirectionalLight("hsl(0, 100%, 100%)");

    this.speedVector = [0, 0];
    return {
      isCrashed: false,
      isSuccess: false,
    };
  },
  created: function () {
    this.scene.add(this.camera);
    this.camera.position.z = 10;

    this.scene.add(this.light);
    this.light.position.set(10, 10, 10);

    this.scene.add(this.cube);
    this.cube.position.x = -15;
    this.cars.forEach((car, index) => {
      this.scene.add(car);
      car.position.x = -12 + index * 4;
      car.speedVectorY = 0.1 * (index + 1);
    });

    this.renderer.setSize(window.innerWidth, window.innerHeight);
    this.scene.background = new THREE.Color("hsl(1, 100%, 100%)");
  },
  mounted: function () {
    this.$refs.canvas.appendChild(this.renderer.domElement);
    this.animate();
    this.addKeyboardListener();
  },
  methods: {
    animate() {
      this.cube.rotation.y += 0.01;
      this.cube.rotation.x += 0.01;
      if (Math.abs(this.cube.position.x + this.speedVector[0]) < 16)
        this.cube.position.x += this.speedVector[0];
      if (Math.abs(this.cube.position.y + this.speedVector[1]) < 6)
        this.cube.position.y += this.speedVector[1];
      this.isCrashed = this.cars.some((car) =>
        this.isCrashedTest(this.cube, car)
      );
      this.cars.forEach((car) => {
        if (Math.abs(car.position.y + car.speedVectorY) < 6) {
          car.position.y += car.speedVectorY;
        } else {
          car.speedVectorY = -car.speedVectorY;
        }
      });

      if (this.isCrashed) return;
      if (this.cube.position.x > 15) {
        this.isSuccess = true;
        return;
      }

      requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
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
    isCrashedTest(cube1, cube2) {
      return (
        Math.abs(cube1.position.y - cube2.position.y) < 1 &&
        Math.abs(cube1.position.x - cube2.position.x) < 1
      );
    },
    retry() {
      this.isCrashed = false;
      this.isSuccess = false;
      console.log(
        "%c [ this.isSuccess ]-156",
        "font-size:13px; background:pink; color:#bf2c9f;",
        this.isSuccess
      );
      this.cube.position.x = -15;
      this.cube.position.y = 0;
      this.animate();
    },
  },
  computed: {
    isGameOver() {
      return this.isCrashed || this.isSuccess;
    },
  },
};
</script>

<style scoped>
.msg,
button {
  position: relative;
  top: -500px;
  z-index: 100;
}
</style>

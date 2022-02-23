<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  mounted() {
    this.init();
  },
  methods: {
    init() {
      // 创建场景
      const scene = new THREE.Scene();
      scene.background = new THREE.Color("#eee");

      // 创建渲染器，挂载在dom中
      const renderer = new THREE.WebGLRenderer({ antialias: true });
      this.$refs.canvas.appendChild(renderer.domElement);
      renderer.setSize(window.innerWidth, window.innerHeight);

      // 创建相机（透射相机）
      const camera = new THREE.PerspectiveCamera(
        50,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      camera.position.z = 5;

      // 创建光源 （平行光）
      const light = new THREE.DirectionalLight("hsl(0, 100%, 100%)");
      light.position.set(0, 0, 10);
      scene.add(light);

      // 创建控件（鼠标拖动检测）
      var controls = new OrbitControls(camera, renderer.domElement); //创建控件对象
      controls.addEventListener("change", renderer);

      // 创建几何体
      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshStandardMaterial({
        side: THREE.FrontSide,
        color: "hsl(140, 45%, 50%)",
        wireframe: false,
      });
      const cube = new THREE.Mesh(geometry, material);
      scene.add(cube);

      // 创建键盘监听器
      document.addEventListener(
        "keydown",
        (e) => {
          var ev = e || window.event;
          switch (ev.keyCode) {
            case 87:
              cube.position.y += 0.1;
              break;
            case 83:
              cube.position.y -= 0.1;
              break;
            case 65:
              cube.position.x -= 0.1;
              break;
            case 68:
              cube.position.x += 0.1;
              break;
            default:
              break;
          }
        },
        false
      );

      // 创建渲染函数
      function animate() {
        renderer.render(scene, camera);
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;
        requestAnimationFrame(animate);
      }
      animate();
    },
  },
};
</script>

<template>
  <div>
    <div ref="canvas" />
  </div>
</template>

<style scoped>
</style>

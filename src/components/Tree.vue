<template>
  <div id="container"></div>
</template>
<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
let scene, mesh;
import pxJpg from '../assets/images/px.jpeg'
import nxJpg from '../assets/images/nx.jpeg'
import pyJpg from '../assets/images/py.jpeg'
import nyJpg from '../assets/images/ny.jpeg'
import pzJpg from '../assets/images/pz.jpeg'
import nzJpg from '../assets/images/nz.jpeg'
export default {
  data() {
    return {
      camera: null,
      //   scene: null,
      renderer: null,
      //   mesh: null,
      controls: null,
      list: [
      ]
    };
  },
  methods: {
    init: function () {
      //  创建场景对象Scene
      scene = new THREE.Scene();

      //网格模型添加到场景中
      let geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);

      const textures = [
          pxJpg,
          nxJpg,
          pyJpg,
          nyJpg,
          pzJpg,
          nzJpg
      ];
      const materials = [];
      const textureLoader = new THREE.TextureLoader();

      for (let i = 0; i < 6; i++) {
        materials.push(
          new THREE.MeshBasicMaterial({ map: textureLoader.load(textures[i]) })
        );
      }

      mesh = new THREE.Mesh(geometry, materials);
      mesh.geometry.scale(1, 1, -1);

      scene.add(mesh);

      /**
       * 相机设置
       */
      let container = document.getElementById("container");
      this.camera = new THREE.PerspectiveCamera(
        70,
        container.clientWidth / container.clientHeight,
        0.01,
        10
      );

          /**
       * 创建渲染器对象
       */
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      container.appendChild(this.renderer.domElement);

      this.camera.position.z = 0.01; // 将相机放在里面
      const controls = new OrbitControls(this.camera, this.renderer.domElement);
      
      controls.enableZoom = false; // 禁用放大
      controls.enablePan = false; // 禁用双指缩放
      controls.enableDamping = true; // 开启阻尼效果
      controls.rotateSpeed = -0.25; // 旋转方向取反，使内部拖拽旋转方向一致

    },

    // 动画
    animate: function () {
      requestAnimationFrame(this.animate);
      //   mesh.rotation.x += 0.01;
      //   mesh.rotation.y += 0.02;
      this.renderer.render(scene, this.camera);
    },
  },
  mounted() {
    this.init();
    this.animate();
  },
};
</script>
<style>
#container {
  position: absolute;
  width: 100%;
  height: 100%;
}
</style>
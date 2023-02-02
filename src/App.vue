<template>
  <Renderer ref="renderer" antialias :orbit-ctrl="{ enableDamping: true, target }" resize >
    <Camera :position="{ x: 0, y: 200, z: 300 }" />
    <Scene ref="scene" background="black" >
      <AmbientLight color="#808080" />
      <PointLight color="#ff6000" />
      <PointLight ref="light" color="#0060ff" :intensity="0.5" />
      <PointLight color="#ff6000" :intensity="0.5" :position="{ x: 100}" />
      <PointLight color="#0000ff" :intensity="0.5" :position="{ x: -100}" />
      <SpotLight
        color="#aaaaaa"
        :intensity="40"
        :position="{ x: 0, y: 1, z: 2 }"
        :angle="Math.PI / 3"
        :penumbra="0.5"
        cast-shadow :shadow-map-size="{ width: 1024, height: 1024 }"
      />
      <DirectionalLight
        :position="{ x: 0, y: 200, z: 0 }"
        cast-shadow :shadow-camera="{ top: 180, bottom: -120, left: -120, right: 120 }"
      />
      <Text
        text="Seance jambe avec guillaume"
        font-src="/assets/helvetiker_regular.typeface.json"
        align="center"
        :size="7"
        :height="1"
        :position="{ x: 0, y: 200, z: 210 }"
        :cast-shadow="true"
      >
      <PhysicalMaterial />
  </Text>
      <FbxModel src="/assets/models/Fall Flat.fbx" @load="onLoad" />

    </Scene>
  </Renderer>
</template>

<script>
// Model from mixamo.com
import { AnimationMixer, Clock, Fog, GridHelper, Vector3 } from 'three';
import {
  AmbientLight,
  Camera,
  DirectionalLight,
  FbxModel,
  HemisphereLight,
  Renderer,
  PointLight,
  PhysicalMaterial,
  Plane,
  Scene,
  SpotLight,
  Text
} from 'troisjs';

export default {
  components: {
    AmbientLight,
    PointLight,
    Camera,
    DirectionalLight,
    FbxModel,
    HemisphereLight,
    Renderer,
    PhysicalMaterial,
    Plane,
    Scene,
    SpotLight,
    Text,
  },
  data() {
    return {
      target: new Vector3(0, 100, 0),
      height: window.innerHeight
    };
  },
  mounted() {
    const scene = this.$refs.scene.scene;
    scene.fog = new Fog(0xa0a0a0,  height, 1000);

   
  },
  methods: {
    onLoad(object) {
      this.mixer = new AnimationMixer(object);
      const action = this.mixer.clipAction(object.animations[0]);
      action.play();

      object.traverse(function (child) {
        if (child.isMesh) {
          child.castShadow = true;
          child.receiveShadow = true;
        }
      });

      this.clock = new Clock();
      this.$refs.renderer.onBeforeRender(this.updateMixer);
    },
    updateMixer() {
      this.mixer.update(this.clock.getDelta());
    },
  },
};
</script>

<style>
body, html {
  margin: 0;
  width: 100%;
  height: 100%;
}
canvas {
  display: block;
  width: 100%;
height: 100vh;
}
</style>

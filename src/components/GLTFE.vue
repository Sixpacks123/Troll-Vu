<template>
    <Renderer ref="renderer" antialias :orbit-ctrl="{ enableDamping: true }" resize>
      <Camera :position="{ x: 1, y: 0, z: 0 }" />
      <Scene>
        <AmbientLight></AmbientLight>
        <GltfModel src="/assets/models/test.glb" />
      </Scene>
    </Renderer>
  </template>
  
  <script lang="ts">
  import {
    AmbientLight,
    Camera,
    GltfModel,
    Renderer,
    Scene,
  } from 'troisjs';
  
  </script>
<script lang="ts">
  import { T, useFrame, useThrelte } from '@threlte/core'
  import {  HTML, OrbitControls } from '@threlte/extras'
	import { AdditiveBlending, Clock, Color, CubeTextureLoader, DirectionalLight, DoubleSide, Group, Mesh, MeshBasicMaterial, MeshStandardMaterial, PlaneGeometry, RepeatWrapping, ShaderMaterial, SphereGeometry, TorusKnotGeometry, Uniform } from 'three';
	import { onMount } from 'svelte';

  import holographicVertexShader from '../../shaders/holographic/vertex.glsl?raw';
	import holographicFragmentShader from '../../shaders/holographic/fragment.glsl?raw';

	import HologramProjector from './models/HologramProjector.svelte';
	import Solly from './models/Solly.svelte';


  const { scene} = useThrelte();
  const materialParameters = {color: '#00c995'};


  const clock = new Clock();

  let sollyRef = new Group();
  let hologramProjectorRef = new Group();

  const material = new ShaderMaterial();
  material.vertexShader = holographicVertexShader;
  material.fragmentShader = holographicFragmentShader;
  material.uniforms = { uTime: new Uniform(0), uColor:  new Uniform(new Color(materialParameters.color))}
  material.transparent = true;
  material.depthWrite = false;
  material.side = DoubleSide;
  material.blending = AdditiveBlending;




  const {start, stop, started} = useFrame ((ctx,deltaTime) => {
    const elapsedTime = clock.getElapsedTime();

    material.uniforms.uTime.value = elapsedTime;
    
  })


  onMount(()=>{

    const loader = new CubeTextureLoader();
    const cubemap = loader.load([
      './space/cube_left.png', // Negative X
      './space/cube_right.png', // Positive X
      './space/cube_up.png', // Positive Y
      './space/cube_down.png', // Negative Y
      './space/cube_front.png', // Positive Z
      './space/cube_back.png'  // Negative Z
    ]);
    scene.background = cubemap;
  })

</script>

<T.PerspectiveCamera
  makeDefault
  position={[0, 2, 10]}
  fov={35}
>
  <OrbitControls
    autoRotate={true}
    autoRotateSpeed={0.5}
    enableDamping
    target.y={1.5}
    
  />
</T.PerspectiveCamera>

<T.DirectionalLight position={[0, 10, 0]} intensity={1} />

<HologramProjector position.x={-0.1} position.y={-0.6} bind:ref={hologramProjectorRef}/>

<Solly bind:ref={sollyRef} {material} />

<HTML center >
  <img src="./solace-logo-next.png" alt="Solace Logo" />
</HTML>
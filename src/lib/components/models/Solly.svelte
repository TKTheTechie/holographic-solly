<!--
Auto-generated by: https://github.com/threlte/threlte/tree/main/packages/gltf
Command: npx @threlte/gltf@2.0.3 ../../../../static/models/solly.glb
-->

<script>
  import { Group } from 'three'
  import { T, forwardEventHandlers } from '@threlte/core'
  import { useGltf } from '@threlte/extras'

  export let ref = new Group();

  const gltf = useGltf('./models/solly.glb',{useDraco: true})

  export let material;

  const component = forwardEventHandlers()
</script>

<T is={ref} dispose={false} {...$$restProps} bind:this={$component}>
  {#await gltf}
    <slot name="fallback" />
  {:then gltf}
    <T.Mesh
      geometry={gltf.nodes.otter.geometry}
      material={material}
      position={[-0.1, 1.3, 0]}
      scale={0.34}
      rotation={[Math.PI/4, 0, 0]}
    />
  {:catch error}
    <slot name="error" {error} />
  {/await}

  <slot {ref} />
</T>

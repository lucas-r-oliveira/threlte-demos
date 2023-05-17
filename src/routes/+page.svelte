<script lang="ts">
	import { browser } from '$app/environment';
	import { Pane } from 'tweakpane'
	import * as Threlte from '@threlte/core';
	import * as Three from 'three';
	import * as Utils from 'three/src/math/MathUtils';

	const gridHelper = new Three.GridHelper(30, 30);
	const axesHelper = new Three.AxesHelper(2);

	const box = {
		position: { x: 0, y: 0.5, z:0 }
	}

	if (browser) {
		// client code
		const pane = new Pane({title: 'Scene'});
		
		const boxControls = pane.addFolder({title: 'Box'});
		boxControls.addInput(box, 'position');

		boxControls.on('change', ({ value }) => {
			box.position = value as any;
		})
	}

</script>

<Threlte.Canvas>
	<Threlte.Object3DInstance object={gridHelper}/>
	<Threlte.Object3DInstance object={axesHelper}/>

	<Threlte.PerspectiveCamera
		position={{ x:10, y:2, z:10 }}
		fov={24}
	>
		<Threlte.OrbitControls autoRotate/>
	</Threlte.PerspectiveCamera>

	<!--Not really doing anything <Threlte.AmbientLight intensity={0.2}/> -->
	<!-- <Threlte.DirectionalLight color="white" intensity={2}/> -->

	<!-- THIS ONE DIDN'T DO ANYTHING EITHER. 
			FIXED: it was because I was using MeshBasicMaterial instead of MeshStandardMaterial
	-->
	<Threlte.DirectionalLight position={{x:3, y:10, z:10}} shadow/>
	<Threlte.DirectionalLight position={{x:-3, y:10, z:-10}} intensity={0.2} />
	<Threlte.AmbientLight intensity={0.2}/>


	<Threlte.Mesh
		geometry={new Three.BoxGeometry( 1, 1, 1 )}
		material={new Three.MeshStandardMaterial({ color: 'white' })}
		position={box.position}
		castShadow
	/>

	<Threlte.Mesh 
		geometry={new Three.CircleGeometry( 3, 72 )}
		material={new Three.MeshStandardMaterial({ 
			color: '#9198e5',
			side: Three.DoubleSide
		})}
		receiveShadow
		rotation={{x: Utils.degToRad(-90)}}
	/>

</Threlte.Canvas>


<script lang="ts">
  import { onMount } from 'svelte';
  import * as THREE from 'three';
  import { writable } from 'svelte/store';

  let canvasElement: HTMLCanvasElement;

  export const cameraPosition = writable({
    latitude: 0,
    longitude: 0,
    altitude: 5000000,
    bearing: 0,
    pitch: 0,
  });

  onMount(() => {
    if (!canvasElement) return;

    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5;

    const renderer = new THREE.WebGLRenderer({
      canvas: canvasElement,
      antialias: true
    });
    renderer.setSize(window.innerWidth, window.innerHeight);

    const geometry = new THREE.SphereGeometry(2, 32, 32);
    const material = new THREE.MeshBasicMaterial({
      color: 0x4477aa,
      wireframe: true
    });
    const sphere = new THREE.Mesh(geometry, material);
    scene.add(sphere);

    const handleResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    };
    window.addEventListener('resize', handleResize);

    const animate = () => {
      requestAnimationFrame(animate);
      sphere.rotation.y += 0.005;
      renderer.render(scene, camera);
    };
    animate();

    return () => {
      window.removeEventListener('resize', handleResize);
      renderer.dispose();
      geometry.dispose();
      material.dispose();
    };
  });
</script>

<div class="w-full h-full">
  <canvas bind:this={canvasElement} class="w-full h-full" />
</div>
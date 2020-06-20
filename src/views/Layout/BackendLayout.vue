<template>
  <div class="app-layout app-layout-backend">
    <nav-bar />
    <b-container fluid>
      <slot />
      
    </b-container>
  </div>
</template>

<script>
import NavBar from '@/components/NavBar.vue';


export default {
  name: 'BackendLayout',
  components: {
    NavBar
  
  }
};
</script>

<template>
 <nav>
    <RouterLink to="/house/index">House Index</RouterLink>
    <RouterLink to="/house/living">House Living</RouterLink>
    <RouterView />
 </nav>
 
</template>

<script lang="ts" setup>
import { RouterLink , RouterView} from 'vue-router'
</script>
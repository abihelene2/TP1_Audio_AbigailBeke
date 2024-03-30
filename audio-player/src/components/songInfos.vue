<script setup lang="ts">

const props = defineProps<{ 
    descSong : string  
    descArtist : string 
}>()
</script>
<template>
    <div id="songInfos" class="p-3">
      <div class="container p-3 bg-dark text-light shadow border border-5 border-primary song-zone">
        <div class="row">
          <div class="col">
            <h2>Informations de la chanson</h2>
            <p id="songDesc">{{ props.descSong }}</p>
            <p id="artistDesc">{{ props.descArtist }}</p>
          </div>
        </div>
      </div>
    </div>
</template>
<style scoped>
</style>
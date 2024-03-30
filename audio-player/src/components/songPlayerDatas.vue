<script setup lang="ts">


 const props = defineProps<{ 
    songName : string 
    artistName: string 
}>()

</script>
<template>
    <div class="row p-2">
        <div id="songPlayerDatas" class="col-12">
            <p id="songName" class="bg-secondary text-white p-2 border border-3 border-dark">
            {{ props.songName }}
            </p>
            <p id="artistName" class="bg-secondary text-white p-2 border border-3 border-dark">
            {{ props.artistName }}
            </p>
        </div>
    </div>
</template>
<style scoped>
</style>
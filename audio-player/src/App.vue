<script setup lang="ts">
import { ref } from 'vue'
import songList from './components/songList.vue'
import songPlayer from './components/songPlayer.vue'
import songInfos from './components/songInfos.vue'
import { Artist } from './scripts/artist';
import { Song } from './scripts/song';

const currentSong = ref<Song>({id : 1, fileName : "ok", songName : "Titre de la chanson", desc : "Desc de la chanson", infosUrl : "https://ocremix.org/remix/OCR01602", artistId : 1});
const currentArtist = ref<Artist>({id : 1, name:"Nom de l'artiste", desc: "Desc de l'artiste"});
const messageError = ref<string>("");
const buttonError = ref<HTMLFormElement| null>(null);

function handleSongClicked(songClicked: Song , artistToFind: Artist): void 
{
  currentSong.value = songClicked;
  currentArtist.value = artistToFind;

  handleResetFileError();
}

function handleFileError(message: string):void
{
  messageError.value = message;
  buttonError.value = document.getElementById("popover");
  buttonError.value?.classList.remove("d-none");

}

function handleResetFileError():void
{
  if(currentSong.value.songName != "Titre de la chanson")
  {
    buttonError.value = document.getElementById("popover");
    buttonError.value?.classList.add("d-none");
  }
}

</script>

<template>
  <header class="container-fluid bg-dark text-light p-4 text-center">
    <h1>Lecteur de musique HEARTLESS</h1>
  </header>
  <main>
    <button id="popover" type="button" class="btn btn-lg btn-danger d-none" data-bs-container="body" data-bs-toggle="popover" data-bs-placement="top" data-bs-content="Top popover">
      <p>{{ messageError }}</p>
    </button>
    <songPlayer @handleFileError="handleFileError" :song ="currentSong" :artist="currentArtist"/>
    <songInfos :descSong="currentSong.desc" :descArtist="currentArtist.desc"/>
    <songList :currentSong="currentSong" @update="handleSongClicked"/>
  </main>
  <footer class="container-fluid bg-secondary text-light p-1">
    <p class="text-center">Copyright Abigail Beke</p>
  </footer>
  
</template>

<style scoped>
</style>
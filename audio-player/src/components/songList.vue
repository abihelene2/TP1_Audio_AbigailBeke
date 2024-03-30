<script setup lang="ts">
import {ref} from "vue"
import { Song } from "../scripts/song";
import { Artist } from "../scripts/artist";
const props = defineProps<{ 
    currentSong: Song
}>()

const emit = defineEmits<{
  (event: 'update', songClicked: Song, artist : Artist): void
}>()

const  songList = ref<Song[]>([]);
const  artistList = ref<Artist[]>([]);

async function fetchSongs() {

  let response = await fetch("http://localhost:3000/songs");
  if (!response.ok) {
    throw new Error(`HTTP error! list songs Status: ${response.status}`);
  }
  let songs = await response.json();
  return songs;
}

fetchSongs().then(songs  => {
  songList.value = songs;
});


async function fetchArtists() {
  let response = await fetch("http://localhost:3000/artists");
  if (!response.ok) {
    throw new Error(`HTTP error! list artists Status: ${response.status}`);
  }
  let artists = await response.json();
  return artists;
}

fetchArtists().then(artists => {
  artistList.value = artists;
});


function onSelectedSong(songClicked: Song)
{
    emit('update',songClicked, getArtist(songClicked.artistId));
}

function getArtist(artistId: number) : Artist
{
  const artistToFind : Artist = artistList.value.find(artist => artist.id == artistId) as Artist;
  return artistToFind;
}

function onShuffleClicked(): void 
{
  //src ChatGPT
  //Genère un  nombre aléatoire entre 0 et la longueur de la liste(exclue)
  let index : number = Math.floor(Math.random() * songList.value.length);
  let selectedSong: Song = songList.value[index];
  onSelectedSong(selectedSong);

}

function getIndexFirstSong():number
{
  let indexFirstSong : number= 0;
  return indexFirstSong;
}

function getIndexLastSong():number
{
  let indexLastSong: number = songList.value.length - 1;
  return indexLastSong;
}

function getIndexCurrentSong(): number
{
  let currentSong : number = songList.value.findIndex(song => song.id == props.currentSong.id);
  return currentSong;
}

function getIndexPreviousSong(): number
{
  let indexPreviousSong: number = getIndexCurrentSong()-1;
  return indexPreviousSong;
}

function getIndexNextSong():number 
{
  let indexNextSong: number = getIndexCurrentSong()+1;
  return indexNextSong;
}

function onNextSongClicked() : void 
{
 let indexNextSong: number = getIndexNextSong();
 if(getIndexCurrentSong() == getIndexLastSong())
  {
    indexNextSong = getIndexFirstSong();
  }
  let nextSong:Song = songList.value[indexNextSong];
  onSelectedSong(nextSong);
}

function onPreviousSongClicked(): void 
{
  let indexPreviousSong: number = getIndexPreviousSong();
  if(getIndexCurrentSong() == getIndexFirstSong())
  {
    indexPreviousSong = getIndexLastSong();
  }
  let previousSong: Song = songList.value[indexPreviousSong];
  onSelectedSong(previousSong);
}
</script>
<template>
<div id="songsList" class="p-3">
      <div class="container p-3 bg-dark text-light shadow border border-5 border-primary song-zone">
        <div class="row">
            <div class="col">
                <h2>Contrôles de la liste</h2>
                <button id="btnShuffle" type="button" class="btn btn-primary" @click="onShuffleClicked()">
                  <i class="bi bi-shuffle"></i> Au hasard!
                </button>
                <button id="btnNext" type="button" class="btn btn-primary" @click="onPreviousSongClicked()">
                  <i class="bi bi-arrow-left-square"></i> Chanson précédente
                </button>
                <button id="btnPrevious" type="button" class="btn btn-primary" @click="onNextSongClicked()">
                   Chanson suivante <i class="bi bi-arrow-right-square"></i>
                </button>
            </div>
        </div>  
        <div class="row">
              <div class="col">
                  <h2>Liste des chansons</h2>
                  <ul id="songsList">
                    <li
                    v-for="song of songList" 
                        v-bind:key="song.id" 
                        v-on:click="onSelectedSong(song)">
                        {{ song.songName }}
                     </li>
                  </ul>
              </div>
          </div>
      </div>
</div>
</template>
<style scoped>
</style>
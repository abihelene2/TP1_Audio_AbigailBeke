<script setup lang="ts">
import {ref} from "vue"

import songPlayerControl from './songPlayerControl.vue'
import songPlayerDatas from './songPlayerDatas.vue'
import songPlayerTime from './songPlayerTime.vue'
import {Song} from '../scripts/song'
import {Artist} from '../scripts/artist'


const props = defineProps<{ 
    song : Song 
    artist: Artist
}>()

const emit = defineEmits<{
  (event: 'handleFileError', messageError :string): void
}>()


const media = ref<any>(null);
const isLoad = ref<boolean>(false);
const duration = ref<string>("00:00");
const elapsedTime = ref<string>("00:00");
const currentInterval = ref<number>(0);

function onLoad() : void 
{
    //src Chat GPT
    //Récupère fichier audio source et ajoute un eventListener
    //lors du chargemnent du fichier audio
    media.value = new Audio();
    media.value.src = `../src/assets/songs/${props.song.fileName}`;
    checkFileExistence();
    media.value.addEventListener('loadedmetadata', onLoadedMetaData);
    media.value.load();

}

function handleFileError(): void 
{
    emit('handleFileError', "Erreur de chargement ou fichier non existant");
}


function checkFileExistence(): void  {
    //src chat gpt
    media.value.onerror = () => {
      handleFileError();
    };
}

     
function onLoadedMetaData(): void
{
    //récupère les données du fichier
    //(la durée et le temps écoulé depuis le chargement)
    setDuration();
    setElapsedTime();
}

function setElapsedTime(): void
{
    //src chat gpt
    currentInterval.value  = setInterval(updateElapsedTime, 1000);
}

function updateElapsedTime(): void 
{
    const currentTime  = media.value.currentTime;
    elapsedTime.value = formattedTime(currentTime);
}

function handlePlay(): void
{
    if(isLoad.value == false)
    {
        onLoad();
        isLoad.value = true;
    }
    media.value.play();
}

function  handlePause(): void 
{
    media.value.pause();
    clearInterval(currentInterval.value);
}

function handleStop(): void 
{
    media.value.pause();
    isLoad.value = false;
    resetElpasedTime();
}

function resetElpasedTime(): void 
{
    media.value.currentTime = 0;
    updateElapsedTime();
}

function formattedTime(timeToFormat: number ) : string
{
    //src Chat GPT
    //Division par 60 le nb de secondes pr trouver le nombre de minutes.
    //Pour trouver le nb de secondes on utilise le modulo qui nous donnera le 
    //reste des minutes.
    const minutes: number  = Math.floor(timeToFormat / 60);
    const seconds: number  = Math.floor(timeToFormat % 60);


    return  `${minutes < 10 ? '0' : ''}${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
}
function setDuration(): void 
{
    duration.value  = formattedTime(media.value.duration);
}

</script>
<template>
    <div id="songPlayer" class="p-3">
      <div class="container p-3 bg-dark text-light shadow border border-5 border-primary">
        <songPlayerDatas :songName="props.song.songName" :artistName="props.artist.name"/>
        <div class="row p-2">
            <songPlayerControl @handlePlay="handlePlay" @handlePause="handlePause" @handleStop="handleStop" />
            <songPlayerTime :duration="duration" :elapsedTime="elapsedTime" />
        </div>
        
      </div>
    </div>

</template>
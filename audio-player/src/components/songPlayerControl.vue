<script setup lang="ts">
import {ref} from "vue"


const emit = defineEmits<{
  (event: 'handlePlay'): void,
  (event: 'handlePause'): void,
  (event: 'handleStop'): void
}>()

const audioPause = ref<HTMLFormElement| null>(null);
const audioPlay = ref<HTMLFormElement| null>(null);
const audioStop = ref<HTMLFormElement| null>(null);

function onButtonPlay(): void 
 {
    const songPlayerControl: HTMLFormElement  = document.getElementById('songPlayerControls') as HTMLFormElement;
    audioPause.value = songPlayerControl.querySelector('#btnPause');
    audioPlay.value = songPlayerControl.querySelector('#btnPlay');
    audioStop.value = songPlayerControl.querySelector('#btnStop');

    emit('handlePlay');
    onClickedPlayButton();
}


function onButtonPause (): void 
{
    emit('handlePause');
    onClickedPauseButton();
}

function onButtonStop(): void 
{
    emit('handleStop');
    onClickedStopButton();
}

function onClickedPlayButton(): void 
{
    toShowAudio(audioPause.value?.classList);
    toShowAudio(audioStop.value?.classList);
    toHideAudio(audioPlay.value?.classList);
}

function onClickedStopButton(): void 
{
    toHideAudio(audioPause.value?.classList);
    toHideAudio(audioStop.value?.classList);
    toShowAudio(audioPlay.value?.classList);
}

function onClickedPauseButton(): void 
{
    toHideAudio(audioPause.value?.classList);
    toShowAudio(audioStop.value?.classList);
    toShowAudio(audioPlay.value?.classList);
}

function toShowAudio(elementToShow: any): void 
{
    elementToShow.remove("d-none")
}

function toHideAudio(elementToHide: any): void 
{
    elementToHide.add("d-none")
}

</script>
<template>
    <div class="row p-2">
        <div id="songPlayerControls" class="col-6">
        <button id="btnPlay" type="button" class="btn btn-primary" @click="onButtonPlay()">
            <i class="bi bi-play-fill"></i> Jouer
        </button>
        <button id="btnPause" type="button" class="btn btn-primary d-none" @click="onButtonPause()">
            <i class="bi bi-pause-fill"></i> Pause
        </button>
        <button id="btnStop" type="button" class="btn btn-primary d-none" @click="onButtonStop()">
            <i class="bi bi-stop-fill"></i> Stop
        </button>
        </div>
    </div>
</template>
<style scoped>
</style>
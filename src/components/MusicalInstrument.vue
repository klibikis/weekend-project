<template>
  <div class="band__wrapper">
    <img src="../assets/images/Music-Band-PNG-HD-Image.png" class="band__image">
    <div :class="{lights__illuminate1: isStrobeLightsOn }"></div>
    <div :class="{lights__illuminate2: isStrobeLightsOn && isRightStrobeLightOn}"></div>
    <div :class="{ lights__bass: isButtonPressed.bass }"></div>
    <div :class="{ lights__drums: isButtonPressed.drums }"></div>
    <div :class="{ lights__solo: isButtonPressed.solo }"></div>
    <div :class="{ lights__guitar: isButtonPressed.guitar }"></div>
  </div>
  <div class="controls__wrapper">
    <div class="demo__wrapper">
      <button class="btn-instruments" @click="handleDemoPlay" :disabled="isDemoPlaying">Play demo</button>
    </div>
    <button @click="handleBassEventOnZ" class="btn-instruments" :disabled="isDemoPlaying">Bass Z</button>
    <button @click="handleDrumsEventOnX" class="btn-instruments" :disabled="isDemoPlaying">Drums X</button>
    <button @click="handleDrumsEventOnC" class="btn-instruments" :disabled="isDemoPlaying">Drums C</button>
    <button @click="handleSoloEventOnV" class="btn-instruments" :disabled="isDemoPlaying">Solo V</button>
    <button @click="handleSoloEventOnB" class="btn-instruments" :disabled="isDemoPlaying">Solo B</button>
    <button @click="handleGuitarEventOnN" class="btn-instruments" :disabled="isDemoPlaying">Guitar N</button>
    <button @click="handleGuitarEventOnM" class="btn-instruments" :disabled="isDemoPlaying">Guitar M</button>
    <div class="toggler__wrapper">
        <p class="temperature-units--text">Party mode</p>
        <label class="switch">
          <input type="checkbox" v-on:change="handleStrobeMode">
          <span class="slider round"></span>
        </label>
      </div>
  </div>
</template>

<script lang="ts">
import { reactive, onMounted, ref } from 'vue'
import { Howl } from 'howler';

export default {
  setup() {
    const isButtonPressed = reactive({
      bass: false,
      drums: false,
      solo: false,
      guitar: false
    })

    const isStrobeLightsOn = ref(true)
    const isRightStrobeLightOn = ref(true)
    const isBoxChecked = ref(false)
    const strobeModeIntervalId = ref(0)
    const isDemoPlaying = ref(false)

    const bassOnZ_ID = ref(0)
    const drumsOnX_ID = ref(0)
    const drumsOnC_ID = ref(0)
    const soloOnV_ID = ref(0)
    const soloOnB_ID = ref(0)
    const guitarOnN_ID = ref(0)
    const guitarOnM_ID = ref(0)

    const drumAudio1 = new Howl({
      src: ['../src/assets/audio/drum1.mp3'],
      onend: () => {
        drumsOnX_ID.value = 0
        isButtonPressed.drums = false
      }})
    const drumAudio2 = new Howl({
      src: ['../src/assets/audio/drum2.wav'],
      volume: 0.4,
      onend: () => {
        drumsOnC_ID.value = 0
        isButtonPressed.drums = false
      }})
    const guitarAudio1 = new Howl({
      src: ['../src/assets/audio/guitar1.wav'],
      onend: () => {
        guitarOnN_ID.value = 0
        isButtonPressed.guitar = false
      }})
    const guitarAudio2 = new Howl({
      src: ['../src/assets/audio/guitar2.wav'],
      volume: 0.4,
      onend: () => {
        guitarOnM_ID.value = 0
        isButtonPressed.guitar = false
      }})
    const soloAudio1 = new Howl({
      src: ['../src/assets/audio/solo1.wav'],
      volume: 0.5,
      onend: () => {
        soloOnV_ID.value = 0
        isButtonPressed.solo = false
      }})
      const soloAudio2 = new Howl({
      src: ['../src/assets/audio/solo2.wav'],
      volume: 0.5,
      onend: () => {
        soloOnB_ID.value = 0
        isButtonPressed.solo = false
      }})
      const bassAudio1 = new Howl({
      src: ['../src/assets/audio/bass1.wav'],
      volume: 0.7,
      onend: () => {
        isButtonPressed.bass = false
        bassOnZ_ID.value = 0
      }})



    function handleStrobeMode() {
      isBoxChecked.value = !isBoxChecked.value
      if(isBoxChecked.value){
        strobeModeIntervalId.value = setInterval(() => {
        isStrobeLightsOn.value = !isStrobeLightsOn.value
      }, 80)
      }else{
        clearInterval(strobeModeIntervalId.value)
        isStrobeLightsOn.value = true
      }
    }

    function handleRightStrobe() {

      if(isBoxChecked.value){
        return
      }
      setInterval(() => {

        changeRightStrobeValue()
        setTimeout(() => {
          changeRightStrobeValue()
        },50)
        setTimeout(() => {
          changeRightStrobeValue()
        },100)
        setTimeout(() => {
          changeRightStrobeValue()
        },150)
      }, 4000)
    }

    function changeRightStrobeValue() {
      isRightStrobeLightOn.value = !isRightStrobeLightOn.value
    }

    function handleBassEventOnZ() {
      if(bassOnZ_ID.value){
        return
      }
      isButtonPressed.bass = true
      bassOnZ_ID.value = bassAudio1.play()
    }

    function handleDrumsEventOnX() {
      if(drumsOnX_ID.value){
        return
      }
      isButtonPressed.drums = true
      drumsOnX_ID.value = drumAudio1.play()
    }

    function handleDrumsEventOnC() {
      if(drumsOnC_ID.value){
        return
      }
      isButtonPressed.drums = true
      drumsOnC_ID.value = drumAudio2.play()
    }

    function handleSoloEventOnV() {
      if(soloOnV_ID.value){
        return
      }
      isButtonPressed.solo = true
      soloOnV_ID.value = soloAudio1.play()
    }

    function handleSoloEventOnB() {
      if(soloOnB_ID.value){
        return
      }
      isButtonPressed.solo = true
      soloOnB_ID.value = soloAudio2.play()
    }

    function handleGuitarEventOnN() {
      if(guitarOnN_ID.value){
        return
      }
      isButtonPressed.guitar = true
      guitarOnN_ID.value = guitarAudio1.play()
    }

    function handleGuitarEventOnM() {
      if(guitarOnM_ID.value){
        return
      }
      isButtonPressed.guitar = true
      guitarOnM_ID.value = guitarAudio2.play()
    }

    function handleDemoPlay() {
      //DEMO GOES HERE
      isDemoPlaying.value = true
      handleBassEventOnZ()
      setTimeout(() => {
        handleSoloEventOnV()
      }, 2800)
      setTimeout(() => {
        handleDrumsEventOnX()
      }, 5700)
      setTimeout(() => {
        handleGuitarEventOnM()
      }, 5700)
      setTimeout(() => {
        handleDrumsEventOnX()
      }, 9100)
      setTimeout(() => {
        handleSoloEventOnB()
      }, 10700)
      setTimeout(() => {
        isDemoPlaying.value = false
        console.log("finished")
      }, 15000)
    }



    onMounted(() => {

      handleRightStrobe()

      window.addEventListener('keyup', function(event) {

        if (event.key == 'z') { 
          handleBassEventOnZ()
        }
        if (event.key == 'x') { 
          handleDrumsEventOnX()
        }
        if (event.key == 'c') { 
          handleDrumsEventOnC()
        }
        if (event.key == 'v') { 
          handleSoloEventOnV()
        }
        if (event.key == 'b') { 
          handleSoloEventOnB()
        }
        if (event.key == 'n') { 
          handleGuitarEventOnN()
        }
        if (event.key == 'm') { 
          handleGuitarEventOnM()
        }
      })

    })
    return {
      isButtonPressed,
      handleBassEventOnZ,
      handleDrumsEventOnX,
      handleDrumsEventOnC,
      handleSoloEventOnV,
      handleSoloEventOnB,
      handleGuitarEventOnN,
      handleGuitarEventOnM,
      isStrobeLightsOn,
      isRightStrobeLightOn,
      handleStrobeMode,
      handleDemoPlay,
      isDemoPlaying
    }
  }
}
</script>



<style scoped>
.controls__wrapper{
  padding: 20px 0;
  display: flex;
  justify-content: center;
  gap: 10px;
  position: relative;
  box-shadow: -2px 0 5px black;
}
  .band__wrapper{
    display: flex;
    justify-content: center;
    align-items: flex-end;
    width: 100%;
    height: 90vh;
    position: relative;
  }
  .band__image{
    min-width: 670px;
    width: 40%;
    z-index: 10;
  }
  .lights__bass{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: linear-gradient(180deg, #4704ff, #4b0bfc4e);
    clip-path: polygon(20% 0, 22% 0, 80% 100%, 20% 100%);

  }
  .lights__drums{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: linear-gradient(180deg, #a4ff1cda, #a4ff1c55);
    clip-path: polygon(36% 0, 38% 0, 80% 100%, 20% 100%);
  }
  .lights__solo{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: linear-gradient(180deg, #fc42ef93, #fefefe25);
    clip-path: polygon(53% 0, 55% 0, 80% 100%, 20% 100%);
  }
  .lights__guitar{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: linear-gradient(180deg, #ff2525c1, #ff252559);
    clip-path: polygon(76% 0, 78% 0, 80% 100%, 20% 100%);
  }
  .lights__illuminate1{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: linear-gradient(120deg, #ffffff93, #fefefe25);
    clip-path: polygon(10% 0, 0 0, 0 10%, 25% 100%, 100% 100%, 100% 80%);
  }
  .lights__illuminate2{
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: linear-gradient(-120deg, #ffffff93, #fefefe25);
    clip-path: polygon(90% 0, 100% 0, 100% 10%, 75% 100%, 0 100%, 0 80%);
  }
  .btn-instruments{
    all: unset;
    background-color: rgba(255, 255, 255, 0.719);
    box-shadow: 0 0 3px white;
    padding: 8px 14px;
    border-radius: 3px;
    cursor: pointer;
    transition: all 0.2s ease-in;
  }
  .btn-instruments:hover{
    background-color: rgb(255, 255, 255);
  }

  .demo__wrapper{
  position: absolute;
  top: 20px;
  left: 10%;
  display: flex;
  align-items: center;
  gap: 10px;
}

  /* SLIDER */
  .toggler__wrapper{
  position: absolute;
  top: 25px;
  right: 10vw;
  display: flex;
  align-items: center;
  gap: 10px;
  color: white;
}
.switch {
  position: relative;
  display: inline-block;
  width: 38px;
  height: 22px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 2px;
  bottom: 2px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #ff000087;
}

input:focus + .slider {
  box-shadow: 0 0 1px #f3f4d6;
}

input:checked + .slider:before {
  -webkit-transform: translateX(16px);
  -ms-transform: translateX(16px);
  transform: translateX(16px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 44px;
}

.slider.round:before {
  border-radius: 50%;
}

@media screen and (max-width: 1200px) {
  .demo__wrapper{
  position: relative;
  display: flex;
  top: 0;
  left: 0;
}
  .toggler__wrapper{
    position: relative;
    display: flex;
    top: 0;
    left: 0;
  }
  .btn-instruments{
    padding: 3px 5px;
  }
  .controls__wrapper{
  padding: 5px 0;
  display: flex;
flex-wrap: wrap;
  gap: 5px;
}
}
</style>

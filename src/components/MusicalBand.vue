<template>
  <div class="band__wrapper">
    <img src="../assets/images/Music-Band-PNG-HD-Image.png" class="band__image" />
    <div :class="{ lights__illuminate1: isStrobeLightsOn }"></div>
    <div :class="{ lights__illuminate2: isStrobeLightsOn && isRightStrobeLightOn }"></div>
    <div :class="{ lights__bass: isInstrumentPlaying.bass }"></div>
    <div :class="{ lights__drums: isInstrumentPlaying.drums }"></div>
    <div :class="{ lights__solo: isInstrumentPlaying.solo }"></div>
    <div :class="{ lights__guitar: isInstrumentPlaying.guitar }"></div>
  </div>
  <ControlsMenu
    :isDemoPlaying="isDemoPlaying"
    @handleBassEventOnZ="handleBassEventOnZ"
    @handleDrumsEventOnX="handleDrumsEventOnX"
    @handleDrumsEventOnC="handleDrumsEventOnC"
    @handleSoloEventOnV="handleSoloEventOnV"
    @handleSoloEventOnB="handleSoloEventOnB"
    @handleGuitarEventOnN="handleGuitarEventOnN"
    @handleGuitarEventOnM="handleGuitarEventOnM"
    @handleStrobeMode="handleStrobeMode"
    @handleDemoPlay="handleDemoPlay"
  />
</template>

<script lang="ts">
import { reactive, onMounted, ref } from 'vue'
import { Howl } from 'howler'
import ControlsMenu from './ControlsMenu.vue'

export default {
  setup() {
    
    const isInstrumentPlaying = reactive({
      bass: false,
      drums: false,
      solo: false,
      guitar: false
    })

    const isStrobeLightsOn = ref(true)
    const isRightStrobeLightOn = ref(true)
    const isPartyModeChecked = ref(false)
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
      src: ['./audio/drum1.mp3'],
      onend: () => {
        drumsOnX_ID.value = 0
        isInstrumentPlaying.drums = false
      }
    })
    const drumAudio2 = new Howl({
      src: ['./audio/drum2.wav'],
      volume: 0.4,
      onend: () => {
        drumsOnC_ID.value = 0
        isInstrumentPlaying.drums = false
      }
    })
    const guitarAudio1 = new Howl({
      src: ['./audio/guitar1.wav'],
      onend: () => {
        guitarOnN_ID.value = 0
        isInstrumentPlaying.guitar = false
      }
    })
    const guitarAudio2 = new Howl({
      src: ['./audio/guitar2.wav'],
      volume: 0.4,
      onend: () => {
        guitarOnM_ID.value = 0
        isInstrumentPlaying.guitar = false
      }
    })
    const soloAudio1 = new Howl({
      src: ['./audio/solo1.wav'],
      volume: 0.5,
      onend: () => {
        soloOnV_ID.value = 0
        isInstrumentPlaying.solo = false
      }
    })
    const soloAudio2 = new Howl({
      src: ['./audio/solo2.wav'],
      volume: 0.5,
      onend: () => {
        soloOnB_ID.value = 0
        isInstrumentPlaying.solo = false
      }
    })
    const bassAudio1 = new Howl({
      src: ['./audio/bass1.wav'],
      volume: 0.7,
      onend: () => {
        isInstrumentPlaying.bass = false
        bassOnZ_ID.value = 0
      }
    })

    function handleStrobeMode() {
      isPartyModeChecked.value = !isPartyModeChecked.value
      if (isPartyModeChecked.value) {
        strobeModeIntervalId.value = setInterval(() => {
          isStrobeLightsOn.value = !isStrobeLightsOn.value
        }, 80)
      } else {
        clearInterval(strobeModeIntervalId.value)
        isStrobeLightsOn.value = true
      }
    }
    function handleRightStrobe() {
      if (isPartyModeChecked.value) {
        return
      }
      setInterval(() => {
        changeRightStrobeValue()
        setTimeout(() => {
          changeRightStrobeValue()
        }, 50)
        setTimeout(() => {
          changeRightStrobeValue()
        }, 100)
        setTimeout(() => {
          changeRightStrobeValue()
        }, 150)
      }, 4000)
    }
    function changeRightStrobeValue() {
      isRightStrobeLightOn.value = !isRightStrobeLightOn.value
    }
    function handleBassEventOnZ() {
      if (bassOnZ_ID.value) {
        return
      }
      isInstrumentPlaying.bass = true
      bassOnZ_ID.value = bassAudio1.play()
    }
    function handleDrumsEventOnX() {
      if (drumsOnX_ID.value) {
        return
      }
      isInstrumentPlaying.drums = true
      drumsOnX_ID.value = drumAudio1.play()
    }
    function handleDrumsEventOnC() {
      if (drumsOnC_ID.value) {
        return
      }
      isInstrumentPlaying.drums = true
      drumsOnC_ID.value = drumAudio2.play()
    }
    function handleSoloEventOnV() {
      if (soloOnV_ID.value) {
        return
      }
      isInstrumentPlaying.solo = true
      soloOnV_ID.value = soloAudio1.play()
    }
    function handleSoloEventOnB() {
      if (soloOnB_ID.value) {
        return
      }
      isInstrumentPlaying.solo = true
      soloOnB_ID.value = soloAudio2.play()
    }
    function handleGuitarEventOnN() {
      if (guitarOnN_ID.value) {
        return
      }
      isInstrumentPlaying.guitar = true
      guitarOnN_ID.value = guitarAudio1.play()
    }
    function handleGuitarEventOnM() {
      if (guitarOnM_ID.value) {
        return
      }
      isInstrumentPlaying.guitar = true
      guitarOnM_ID.value = guitarAudio2.play()
    }
    function handleDemoPlay() {

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
      }, 15000)
    }
    onMounted(() => {
      handleRightStrobe()
      window.addEventListener('keyup', function (event) {
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
      isInstrumentPlaying,
      isStrobeLightsOn,
      isRightStrobeLightOn,
      isDemoPlaying,
      handleBassEventOnZ,
      handleDrumsEventOnX,
      handleDrumsEventOnC,
      handleSoloEventOnV,
      handleSoloEventOnB,
      handleGuitarEventOnN,
      handleGuitarEventOnM,
      handleStrobeMode,
      handleDemoPlay
    }
  },

  components: { ControlsMenu }

}
</script>

<style scoped>
.controls__wrapper {
  padding: 20px 0;
  display: flex;
  justify-content: center;
  gap: 10px;
  position: relative;
  box-shadow: -2px 0 5px black;
}
.band__wrapper {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  width: 100%;
  height: 90vh;
  position: relative;
}
.band__image {
  min-width: 670px;
  width: 40%;
  z-index: 10;
}
.lights__bass {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(180deg, #4704ff, #4b0bfc4e);
  clip-path: polygon(20% 0, 22% 0, 80% 100%, 20% 100%);
}
.lights__drums {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(180deg, #a4ff1cda, #a4ff1c55);
  clip-path: polygon(36% 0, 38% 0, 80% 100%, 20% 100%);
}
.lights__solo {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(180deg, #fc42ef93, #fefefe25);
  clip-path: polygon(53% 0, 55% 0, 80% 100%, 20% 100%);
}
.lights__guitar {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(180deg, #ff2525c1, #ff252559);
  clip-path: polygon(76% 0, 78% 0, 80% 100%, 20% 100%);
}
.lights__illuminate1 {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(120deg, #ffffff93, #fefefe25);
  clip-path: polygon(10% 0, 0 0, 0 10%, 25% 100%, 100% 100%, 100% 80%);
}
.lights__illuminate2 {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(-120deg, #ffffff93, #fefefe25);
  clip-path: polygon(90% 0, 100% 0, 100% 10%, 75% 100%, 0 100%, 0 80%);
}
</style>

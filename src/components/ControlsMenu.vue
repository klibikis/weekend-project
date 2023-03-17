<template>
  <div class="controls__wrapper">
    <div class="demo__wrapper">
      <button 
        class="btn-instruments" 
        @click="$emit('handleDemoPlay')" 
        :disabled="isDemoPlaying || isInstrumentPlaying.bass || isInstrumentPlaying.drums || isInstrumentPlaying.solo || isInstrumentPlaying.guitar"
      >
        Play demo
      </button>
    </div>
    <div class="controls__buttons-wrapper">
        <button 
          @click="$emit('handleBassEventOnZ')" 
          class="btn-instruments" 
          :disabled="isDemoPlaying"
          >
          Bass Z
        </button>
        <button 
          @click="$emit('handleDrumsEventOnX')" 
          class="btn-instruments" 
          :disabled="isDemoPlaying"
        >
          Drums X
        </button>
        <button 
          @click="$emit('handleDrumsEventOnC')" 
          class="btn-instruments" 
          :disabled="isDemoPlaying"
        >
          Drums C
        </button>
        <button 
          @click="$emit('handleSoloEventOnV')"
          class="btn-instruments" 
          :disabled="isDemoPlaying"
        >
          Solo V
        </button>
        <button 
          @click="$emit('handleSoloEventOnB')" 
          class="btn-instruments" 
          :disabled="isDemoPlaying"
        >
          Solo B
        </button>
        <button 
          @click="$emit('handleGuitarEventOnN')" 
          class="btn-instruments" 
          :disabled="isDemoPlaying"
        >
          Guitar N
        </button>
        <button 
          @click="$emit('handleGuitarEventOnM')" 
          class="btn-instruments" 
          :disabled="isDemoPlaying"
        >
          Guitar M
        </button>
      </div>
      <div class="controls__info-wrapper">
        <p>Click button or press corresponding key!</p>
      </div>
      <div class="toggler__wrapper">
        <p class="temperature-units--text">Party mode</p>
        <label class="switch">
          <input 
            type="checkbox" 
            v-on:change="$emit('handleStrobeMode')" 
          />
          <span class="slider round"></span>
        </label>
      </div>
      
  </div>
</template>

<script lang="ts">

export default {
  props: {
    isDemoPlaying: Boolean,
    isInstrumentPlaying: {
      type: Object,
      default: () => ({}),
    }
  }
}
</script>

<style scoped>
.controls__wrapper{
  display: flex;
  flex-direction: column;
  gap: 5px;
  align-items: center;
}
.controls__buttons-wrapper{
  display: flex;
  justify-content: center;
  width: 60%;
  gap: 5px;
}
.controls__info-wrapper{
  color: white
}
.instrument-buttons-section__wrapper{
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.instrument-buttons__wrapper{
  display: flex;
}
.btn-instruments {
  all: unset;
  background-color: rgba(255, 255, 255, 0.719);
  box-shadow: 0 0 3px white;
  padding: 8px 14px;
  border-radius: 3px;
  cursor: pointer;
  transition: all 0.2s ease-in;
}
.btn-instruments:hover {
  background-color: rgb(255, 255, 255);
}

.demo__wrapper {
  position: absolute;
  top: 20px;
  left: 10%;
  display: flex;
  align-items: center;
  gap: 10px;
}

/* SLIDER */
.toggler__wrapper {
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
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: '';
  height: 18px;
  width: 18px;
  left: 2px;
  bottom: 2px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
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
  .demo__wrapper {
    position: relative;
    display: flex;
    top: 0;
    left: 0;
  }

  .toggler__wrapper {
    position: relative;
    display: flex;
    top: 0;
    left: 0;
  }

  .btn-instruments {
    padding: 3px 5px;
  }

  .controls__wrapper {
    padding: 5px 0;
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
  }
}
</style>

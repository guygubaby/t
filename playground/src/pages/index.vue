<template>
  <div class="py-30">
    <AT v-bind="transitionOptions" @before-enter="handleBe" @enter="handleE" @after-enter="handleAe" @before-leave="handleBl" @leave="handleL" @after-leave="handleAl">
      <p v-if="isShow">
        Hi there
      </p>
    </AT>
    <Controller @on-change="handleAnimationChange" />
    <button class="btn mt-10" @click="toggle()">
      toggle
    </button>
    <br>
    <p class="my-10">
      Or
    </p>
    <br>
    <button ref="buttonRef" class="btn" @click="manuallyAnimate">
      animte el directly
    </button>
    <Controller @on-change="handleManuallyAnimationChange" />

    <router-link class="icon-btn !outline-none mt-8" to="/directive">
      <div class="ring flex items-center px-2 py-1 rounded">
        <span>Directive usage</span>
        <div i-carbon-arrow-right m="l-2" />
      </div>
    </router-link>

    <br>

    <router-link class="icon-btn !outline-none mt-8" to="/gsap">
      <div class="ring flex items-center px-2 py-1 rounded">
        <span>gsap usage</span>
        <div i-carbon-flash m="l-2" text="purple-500" />
        <div i-carbon-arrow-right m="l-2" />
      </div>
    </router-link>
  </div>
</template>

<script setup lang="ts">
import { useToggle } from '@vueuse/core'
import type { AnimateCssNames } from '@bryce-loskie/at'
import AT, { AnimateCssPresets, animateElem, defineOptions } from '@bryce-loskie/at'

const isShow = ref(true)
const toggle = useToggle(isShow)

const animationName = ref<AnimateCssNames>(AnimateCssPresets.backInDown)

const transitionOptions = computed(() => {
  return defineOptions({
    appear: true,
    animate: animationName.value,
    // enterAnimate: AnimateCssPresets.backInDown,
    // leaveAnimate: AnimateCssPresets.backOutUp,
    duration: 300,
    // enterDuration: 1000,
    leaveDuration: 500,
    leaveDelay: 50,
  })
})

const handleAnimationChange = (name: AnimateCssNames) => {
  animationName.value = name
}

const handleBe = () => {
  console.log('before enter')
}

const handleE = () => {
  console.log('enter')
}

const handleAe = () => {
  console.log('after enter')
}

const handleBl = () => {
  console.log('before leave')
}

const handleL = () => {
  console.log('leave')
}

const handleAl = () => {
  console.log('after leave')
}

const buttonRef = ref()

const manuallyAnimationName = ref<AnimateCssNames>(AnimateCssPresets.backInDown)

const manuallyAnimate = () => {
  animateElem({
    elem: buttonRef,
    animation: manuallyAnimationName.value,
    repeat: 1,
    direction: 'alternate',
    onComplete: (elem) => {
      console.log('onComplete', elem)
      animateElem({
        elem,
        animation: 'jello',
        repeat: 'infinite',
      })
    },
  })
}

const handleManuallyAnimationChange = (name: AnimateCssNames) => {
  manuallyAnimationName.value = name
  manuallyAnimate()
}

onMounted(manuallyAnimate)
</script>

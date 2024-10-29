<script setup lang="ts">
// import { useWindowSize } from '@vueuse/core'

// const { width, height } = useWindowSize()
import { useDisplayMedia } from '@vueuse/core'

const { stream, start , stop } = useDisplayMedia()

// start streaming
if (process.client) {
    const video = document.getElementById('video')
    watchEffect(() => {
        // preview on a video element
        video.srcObject = stream.value
    })
}


</script>

<template>
    <video muted autoplay controls id="video" class="h-100 w-auto"></video>
    <button @click="start()">start</button>
    <button @click="stop()">stop</button>
    <!-- <UseWindowSize v-slot="{ width, height }"> -->
    <!-- Width: {{ width }}
        Height: {{ height }} -->
    <!-- </UseWindowSize> -->
</template>

<!-- <script setup>
const colorMode = useColorMode()

colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'

</script>

<template>
    <LayouttHeader />
    <p class="dark:bg-green-500"><i class="ti ti-armchair text-3xl p-2"></i>Hello</p>
    <UChip text="3" size="2xl">
        <UButton icon="i-shopping-cart-sign" color="gray" />
    </UChip>
    <LayouttFooter />
</template> -->

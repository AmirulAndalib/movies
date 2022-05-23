<script setup lang="ts">
import type { Media } from '~/types'
import { TMDB_IMAGE_BASE_ORIGINAL } from '~/constants/images'
import { formatTime } from '~/composables/utils'

const { item } = defineProps<{
  item: Media
}>()

const trailer = $computed(() => getTrailer(item))

const showModal = useIframeModal()
function playTrailer() {
  if (trailer)
    showModal(trailer)
}
</script>

<template>
  <div>
    <div relative class="aspect-ratio-3/2 lg:aspect-ratio-25/9" bg-black>
      <div
        absolute top-0 right-0
        lt-lg="left-0"
        lg="bottom-0 left-1/3"
      >
        <img
          :src="TMDB_IMAGE_BASE_ORIGINAL + item.backdrop_path"
          h-full w-full object-cover
        >
      </div>
      <Transition appear name="hero">
        <div
          absolute bottom-0 left-0 px-10 justify-center
          lt-lg="bg-gradient-to-t right-0 p10"
          lg="top-0 px25 w-2/3 bg-gradient-to-r"
          from-black via-black to-transparent
        >
          <h1 mt-2 text-4xl lg:text-5xl>
            {{ item.title || item.name }}
          </h1>
          <div row gap3 items-center mt4>
            <StarsRate w-25 :value="item.vote_average" />
            <div op50>
              {{ item.vote_average }}
            </div>
            <div op50>
              {{ item.vote_count }} Reviews
            </div>
            <div v-if="item.release_date" op50>
              {{ item.release_date.slice(0, 4) }}
            </div>
            <div v-if="item.runtime" op50>
              {{ formatTime(item.runtime) }}
            </div>
          </div>
          <p mt-2 op80 leading-relaxed of-hidden line-clamp-3 md:line-clamp-5>
            {{ item.overview }}
          </p>
          <div v-if="trailer" py5 display-none lg:block>
            <button
              flex="~ gap2" items-center p="x6 y3"
              bg="gray/15 hover:gray/20" transition
              @click="playTrailer()"
            >
              <div i-ph-play />
              Watch Trailer
            </button>
          </div>
        </div>
      </Transition>
      <div v-if="trailer" lg:hidden absolute left-0 top-0 right-0 h="2/3" items-center justify-center>
        <button
          items-center p10 text-5xl op20 hover:op80 transition
          @click="playTrailer()"
        >
          <div i-ph-play-circle-light />
        </button>
      </div>
    </div>
  </div>
</template>

<style>
.hero-enter-active,
.hero-leave-active {
  transition: transform .75s cubic-bezier(.4, .25, .3, 1), opacity .3s cubic-bezier(.4, .25, .3, 1);
}

.hero-enter,
.hero-leave-to {
  opacity: 0;
  transform: translate3d(0, 2rem, 0);
}

.hero-enter-to,
.hero-leave {
  opacity: 1;
  transform: translateZ(0);
}
</style>
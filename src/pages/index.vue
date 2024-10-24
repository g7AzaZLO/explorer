<script lang="ts" setup>
import { Icon } from '@iconify/vue';
import {
  useDashboard,
  LoadingStatus,
  type ChainConfig,
} from '@/stores/useDashboard';
import ChainSummary from '@/components/ChainSummary.vue';
import AdBanner from '@/components/ad/AdBanner.vue';

import { computed, onMounted, ref } from 'vue';
import { useBlockchain } from '@/stores';

const dashboard = useDashboard();

const keywords = ref('');
const chains = computed(() => {
  if (keywords.value) {
    const lowercaseKeywords = keywords.value.toLowerCase();

    return Object.values(dashboard.chains).filter(
      (x: ChainConfig) => x.chainName.toLowerCase().indexOf(lowercaseKeywords) > -1 
      || x.prettyName.toLowerCase().indexOf(lowercaseKeywords) > -1
    );
  } else {
    return Object.values(dashboard.chains);
  }
});

const featured = computed(() => {
  const names = ["cosmos", "osmosis", "akash", "celestia", "evmos", "injective", "dydx", "noble"];
  return chains.value
    .filter(x => names.includes(x.chainName))
    .sort((a, b)=> (names.indexOf(a.chainName) - names.indexOf(b.chainName)))
})

const chainStore = useBlockchain()

</script>
<template>
  <div class="">
    <div class="flex md:!flex-row flex-col items-center justify-center mb-6 mt-14 gap-2">
      <div class="w-16 rounded-full">
        <svg id="_Слой_3" data-name="Слой_3" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 150 132">
          <defs>
            <style>
              .cls-1 {
                fill: none;
              }

              .cls-2 {
                fill: #66c6e4;
              }
            </style>
          </defs>
          <path class="cls-2" d="M100.73,30.9l-4.71,7.14c-5.17-3.76-11.5-5.96-18.34-5.96-17.4,0-31.51,14.29-31.51,31.92,0,11.17,5.66,20.99,14.23,26.69l-4.43,6.72c-10.65-7.2-17.66-19.39-17.66-33.22,0-22.14,17.95-40.09,40.08-40.09,8.27,0,15.96,2.51,22.34,6.8Z"/>
          <path class="cls-1" d="M118.47,64.19c0,22.13-17.94,40.08-40.08,40.08-8.31,0-16.02-2.53-22.42-6.86l4.43-6.72c-8.57-5.7-14.23-15.52-14.23-26.69,0-17.63,14.11-31.92,31.51-31.92,6.84,0,13.17,2.2,18.34,5.96l4.71-7.14c10.7,7.2,17.74,19.42,17.74,33.29Z"/>
          <path class="cls-1" d="M109.2,64c0,17.63-14.11,31.92-31.52,31.92-6.38,0-12.32-1.92-17.28-5.23-8.57-5.7-14.23-15.52-14.23-26.69,0-17.63,14.11-31.92,31.51-31.92,6.84,0,13.17,2.2,18.34,5.96,7.98,5.79,13.18,15.26,13.18,25.96Z"/>
          <polygon class="cls-2" points="118.27 60.23 112.95 68.19 82.32 114.03 72.53 114.03 103.02 68.19 74.06 68.19 74.06 60.23 118.27 60.23"/>
        </svg>
      </div>
      <h1 class="text-primary dark:invert text-3xl md:!text-6xl font-bold">
        {{ $t('pages.title') }}
      </h1>
    </div>
    <div class="text-center text-base">
      <p class="mb-1">
        {{ $t('pages.slogan') }}
      </p>
    </div>
    <div
      v-if="dashboard.status !== LoadingStatus.Loaded"
      class="flex justify-center"
    >
      <progress class="progress progress-info w-80 h-1"></progress>
    </div>

    <div v-if="featured.length>0" class="text-center text-base mt-6 text-primary">
      <h2 class="mb-6"> Featured Blockchains 🔥 </h2>
    </div>

    <div v-if="featured.length>0"
      class="grid grid-cols-1 gap-4 mt-6 md:!grid-cols-3 lg:!grid-cols-4 2xl:!grid-cols-5"
    >
    <ChainSummary
        v-for="(chain, index) in featured"
        :key="index"
        :name="chain.chainName"
      />
    </div>

    <AdBanner id="home-banner-ad" unit="banner" width="970px" height="90px" />

    <div class="text-center text-base mt-6 text-primary">
      <h2 class="mb-6">{{ $t('pages.description') }}</h2>
    </div>

    <div class="flex items-center rounded-lg bg-base-100  border border-gray-200 dark:border-gray-700 mt-10">
      <Icon icon="mdi:magnify" class="text-2xl text-gray-400 ml-3"/>
      <input :placeholder="$t('pages.search_placeholder')" class="px-4 h-10 bg-transparent flex-1 outline-none text-base" v-model="keywords" />
      <div class="px-4 text-base hidden md:!block">{{ chains.length }}/{{ dashboard.length }}</div>
    </div>

    <div
      class="grid grid-cols-1 gap-4 mt-6 md:!grid-cols-3 lg:!grid-cols-4 2xl:!grid-cols-5"
    >
      <ChainSummary
        v-for="(chain, index) in chains"
        :key="index"
        :name="chain.chainName"
      />
    </div>
  </div>
</template>

<style scoped>
 .logo path{
  fill: #171d30;
}
</style>@/components/ad/ad

<template>
  <div class="ml-1 lg:ml-6">
    <span 
      class="py-1 px-2 my-4 inline-block cursor-pointer"
      @click="store.filters.enabled=!store.filters.enabled">
      <span v-if="!store.filters.enabled">Apply Filters</span>
      <span v-if="store.filters.enabled">Disable Filters</span>
    </span>
    <div v-if="store.filters.enabled" class="bg-black/10 py-3 px-4">
      <!-- valid -->
      <div class="mb-4" v-if="this.store.tasks.lastUpdate?.['relays/nip11']">
        <span 
          @click="toggleFilter('valid/nip11', true, true)"
          class="text-black/50 dark:text-white/30 cursor-pointer mr-2 mb-2 py1 px-3 bg-black/20 inline-block"
          :class="{
              'bg-white/20': store?.filters?.getRule('valid/nip11', true)?.length
          }">
          Valid Pubkey
        </span>
      </div>


      <!-- By nip -->
      <div class="mb-4">
        <span  
          class="text-black/50 dark:text-white/70 block py-1 mb-1 cursor-pointer :hover:dark:bg-black/20"
          @click="store.filters.show['nips']=!store.filters.show['nips']">
          NIPs
          <span class="dark:text-white/40"> {{ store.filters.show['nips'] ? 'hide' : 'show'  }}</span>
        </span>
        <span v-if="store.filters.show['nips']">
          <span
            v-for="nip in Object.keys(store.stats?.nips || {})" 
            :key="`filter-nip-${nip.key}`" 
            @click="toggleFilter('nips', store.stats?.nips[nip].key, false)"
            :class="{
              'bg-white/20': store.filters.getRule('nips', store.stats?.nips[nip].key)?.length
            }"
            class="cursor-pointer mr-2 mb-2 py1 px-3 bg-black/20 inline-block">
            NIP-{{ store.stats?.nips[nip].key}} 
            <span class="text-xs text-white/50">
              {{ store.stats?.nips[nip].count }}
            </span>
          </span>
        </span>
        
      </div>

      <!-- By software -->
      <div class="mb-4">
        <span  
          class="text-black/50 dark:text-white/70  block py-1 mb-1 cursor-pointer :hover:dark:bg-black/20"
          @click="store.filters.show['software']=!store.filters.show['software']">
          Software
          <span class="dark:text-white/40"> {{ store.filters.show['software'] ? 'hide' : 'show'  }}</span>

        </span>
        <span v-if="store.filters.show['software']">
          <span 
            v-for="sw in Object.keys(store.stats?.software || {})" 
            :key="`filter-software-${store.stats?.software[sw].key}`" 
            @click="toggleFilter('software', store.stats?.software[sw].key, true)"
            :class="{
              'bg-white/20': store.filters.getRule('software', store.stats?.software[sw].key)?.length
            }"
            
            class="cursor-pointer mr-2 mb-2 py1 px-3 bg-black/20 inline-block">
            {{ store.stats?.software[sw].key}} 
            <span class="text-xs text-white/50">
              {{ store.stats?.software[sw].count }}
            </span>
          </span>
        </span>
      </div>

      <!-- By country -->
      <div class="mb-4">
        <span  
          class="text-black/50 dark:text-white/70 block py-1 mb-1 cursor-pointer :hover:dark:bg-black/20"
          @click="store.filters.show['countries']=!store.filters.show['countries']">
          Countries
          <span class="dark:text-white/40"> {{ store.filters.show['countries'] ? 'hide' : 'show'  }}</span>
        </span>
        <span v-if="store.filters.show['countries']">
          <span 
            v-for="country in Object.keys(store.stats?.countries || {})" 
            :key="`filter-countries-${store.stats?.countries[country].key}`" 
            @click="toggleFilter('countries', store.stats?.countries[country].key, true, 'continents')"
            :class="{
              'bg-white/20': store.filters.getRule('countries', store.stats?.countries[country].key)?.length
            }"
            class="cursor-pointer mr-2 mb-2 py1 px-3 bg-black/20 inline-block">
            {{ store.stats?.countries[country].key}} 
            <span class="text-xs text-white/50">
              {{ store.stats?.countries[country].count }}
            </span>
          </span>
        </span> 
      </div>

      <!-- By continent -->
      <div class="mb-4">
        <span  
          class="text-black/50 dark:text-white/70 block py-1 mb-1 cursor-pointer :hover:dark:bg-black/20"
          @click="store.filters.show['continents']=!store.filters.show['continents']">
            Continents 
            <span class="dark:text-white/40"> {{ store.filters.show['continents'] ? 'hide' : 'show'  }}</span>
        </span>
        <span v-if="store.filters.show['continents']">
          <span 
            v-for="continent in Object.keys(store.stats?.continents || {})" 
            :key="`filter-continents-${store.stats?.continents[continent].key}`" 
            @click="toggleFilter('continents', store.stats?.continents[continent].key, true, 'countries')"
            :class="{
              'bg-white/20': store.filters.getRule('continents', store.stats?.continents[continent].key)?.length
            }"
            
            class="cursor-pointer mr-2 mb-2 py1 px-3 bg-black/20 inline-block">
            {{ store.stats?.continents[continent].key}} 
            <span class="text-xs text-white/50">
              {{ store.stats?.continents[continent].count }}
            </span>
          </span>
        </span> 
      </div>
    </div>
  </div>

</template>
<script>
  import RelaysLib from '@/shared/relays-lib.js'
  import { defineComponent, toRefs } from 'vue'
  import SharedComputed from '@/shared/computed.js'
  import { setupStore } from '@/store'

  export default defineComponent(
  {
    name: 'FiltersPartial',
    components: {},
    setup(props){
      const {resultsProp: results} = toRefs(props)
      return { 
        store : setupStore(),
        results: results,
      }
    },
    beforeMount(){
      // this.foundNips = this.collateSupportedNips()
    },
    mounted(){
    },
    updated(){
      
    },
    beforeUnmount(){
    },
    unmounted(){
      delete this.results
    },
    props: {
      resultsProp: {
        type: Object,
        default(){
          return {}
        }
      },
    },
    data() {
      return {

      }
    },
    methods: Object.assign(RelaysLib, {
      toggleFilter(ref, key, unique, reset){
        const rule = this.store.filters.getRule(ref, key)
        console.log('rule', rule)
        if(rule?.length) {
          console.log('filters: removing', rule)
          this.store.filters.removeRule(ref, key, unique, reset)
          console.log('filters: effect', this.store.filters.rules)
          
        } else {
          console.log('filters: adding', rule)
          this.store.filters.addRule(ref, key, unique, reset)
          console.log('filters: effect', this.store.filters.rules)
        }
      }
    }),
    computed: Object.assign(SharedComputed, {

    })
  })
</script>
<script setup>
const props = defineProps(["accounts", "selectedAccount", "selectedMetric"]);
defineEmits(["setAccount", "setSelectedMetric"]);
</script>
<template>
  <div class="filter flex items-center justify-center flex-col md:flex-row">
    <div class="flex items-center justify-center mt-5">
      <div class="flex items-center justify-center mr-2 text-xs">
        Select Ad Account
        <img src="/@assets/arrow-right.svg" class="ml-1 w-3" />
      </div>
      <template v-if="accounts">
        <div
          v-for="account in accounts"
          :class="['filter-item', { active: account == selectedAccount }]"
          :key="account.id"
          @click="$emit('setAccount', account)"
        >
          {{ account.name }}
        </div>
      </template>
    </div>
    <div class="seperator ml-5 mt-5 mr-5 hidden md:block"></div>
    <div class="flex items-center justify-center mt-5">
      <div class="flex items-center justify-center mr-2 text-xs">
        Select Metric
        <img src="/@assets/arrow-right.svg" class="ml-1 w-3" />
      </div>
      <div
        :class="['filter-item', { active: selectedMetric == 'clicks' }]"
        @click="$emit('setSelectedMetric', 'clicks')"
      >
        Clicks
      </div>
      <div
        :class="['filter-item', { active: selectedMetric == 'impressions' }]"
        @click="$emit('setSelectedMetric', 'impressions')"
      >
        Impressions
      </div>
      <div
        :class="['filter-item', { active: selectedMetric == 'spend' }]"
        @click="$emit('setSelectedMetric', 'spend')"
      >
        Spend
      </div>
    </div>
  </div>
</template>
<style scoped>
@import "./Filter.css";
</style>

<script setup>
import { onMounted, ref, reactive } from "vue";
import VueApexCharts from "vue3-apexcharts";
import SocialIcons from "./components/SocialIcons.vue";
import Filter from "./components/Filter.vue";

const chartOptions = reactive({
  chart: {
    id: "vuechart-example",
    toolbar: {
      show: false,
    },
  },
  xaxis: {
    type: "datetime",
  },
});
const series = reactive([
  {
    name: "",
    data: [],
  },
]);

const accounts = ref(null);
const selectedAccount = ref(null);
const selectedMetric = ref("clicks");
const leftIcons = [
  { src: "google-analytics.svg", class: "social-icon" },
  { src: "wordpress.svg", class: "social-icon" },
  { src: "linkedin.svg", class: "social-icon" },
  { src: "woocommerce.svg", class: "social-icon" },
  { src: "yandex.svg", class: "social-icon gradient-bottom" },
];
const rightIcons = [
  { src: "facebook.svg", class: "social-icon gradient-top" },
  { src: "youtube.svg", class: "social-icon" },
  { src: "instagram.svg", class: "social-icon" },
  { src: "shopify.svg", class: "social-icon" },
  { src: "snapchat.svg", class: "social-icon" },
];

onMounted(() => {
  fetch("https://api.wask.co/demo/myaccounts", {
    method: "GET",
    mode: "cors",
    headers: {
      "Content-Type": "application/json",
      Token: "2zcgJnjDyb",
    },
  })
    .then((res) => res.json())
    .then((data) => {
      accounts.value = data.accounts;
      setAccount(accounts.value[0]);
    });
});
const setAccountData = () => {
  let dates = [];
  let seriesData = [];
  let insights = selectedAccount.value.insights.data;
  insights.forEach((selectedAccountData) => {
    // dates.push(new Date(selectedAccountData.date_start).getTime());
    seriesData.push([
      new Date(selectedAccountData.date_start).getTime(),
      selectedAccountData[selectedMetric.value],
    ]);
  });
  series[0].name = selectedMetric.value;
  series[0].data = seriesData;
  console.log("chart options :", chartOptions);
};
const setAccount = (account) => {
  selectedAccount.value = account;
  setAccountData();
};
const setSelectedMetric = (metric) => {
  selectedMetric.value = metric;
  setAccountData();
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <div class="grid grid-cols-12 gap-3 text-center">
      <div class="col-span-2 md:block hidden">
        <SocialIcons :icons="leftIcons"></SocialIcons>
      </div>
      <div class="lg:col-span-8 md:col-span-8 col-span-12">
        <h1 class="header mb-5">Marketing Integrations</h1>
        <h2 class="sub-header mb-5">
          Trust WASK's smart optimization features
        </h2>
        <Filter
          :accounts="accounts"
          :selectedAccount="selectedAccount"
          :selectedMetric="selectedMetric"
          @setAccount="setAccount"
          @setSelectedMetric="setSelectedMetric"
        ></Filter>
        <div class="chart-wrapper mt-5">
          <VueApexCharts
            type="line"
            height="350"
            :options="chartOptions"
            :series="series"
          ></VueApexCharts>
        </div>
      </div>
      <div class="col-span-2 md:block hidden">
        <SocialIcons :icons="rightIcons"></SocialIcons>
      </div>
    </div>
  </div>
</template>
<style scoped>
@import "./App.css";
</style>

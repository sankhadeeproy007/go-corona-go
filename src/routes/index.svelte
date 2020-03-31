<script>
  import { onMount } from "svelte";
  import Chart from "chart.js";

  import Card from "../components/Card.svelte";

  let data = {};
  let countryName = "";
  let chart;

  onMount(() => {
    getAllData();
  });
  const drawChart = () => {
    const ctx = document.getElementById("chart");
    if (chart) {
      chart.data.datasets[0].data = [data.active, data.recovered, data.deaths];
      chart.update();
      return;
    }
    chart = new Chart(ctx, {
      type: "doughnut",
      data: {
        labels: ["Active", "Recovered", "Deaths"],
        datasets: [
          {
            label: "# of Votes",
            data: [data.active, data.recovered, data.deaths],
            backgroundColor: [
              "rgb(194, 42, 124)",
              "rgb(20, 216, 112)",
              "rgb(255, 128, 0)"
            ],
            borderWidth: 0
          }
        ]
      },
      options: {}
    });
  };
  const getAllData = async () => {
    const res = await fetch(`https://corona.lmao.ninja/all`);
    data = await res.json();
    countryName = "";
    drawChart();
  };
  const getDataByCountry = async () => {
    const response = await fetch(
      `https://corona.lmao.ninja/countries/${countryName}`
    );
    data = await response.json();
    drawChart();
  };
</script>

<style>
  .search-container {
    display: grid;
    grid-template-columns: 270px 90px 90px;
    grid-gap: 10px;
    margin-bottom: 12px;
  }
  input {
    height: 35px;
    font-size: 14px;
    font-weight: 500;
    padding-inline-start: 10px;
  }
  button {
    background: #5939ec;
    border-radius: 4px;
    color: #fff;
    font-size: 14px;
    font-weight: 500;
  }
  .row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 10px;
  }
  .chart-container {
    margin-top: 20px;
    display: grid;
    justify-content: center;
    align-items: center;
  }
  #chart {
    max-width: 400px;
  }
</style>

<svelte:head>
  <title>Go corona go!</title>
</svelte:head>
<h1>
  {#if !data.country}Global data{:else}{`Showing data for ${data.country}`}{/if}
</h1>
<h2>Search by country</h2>
<div class="search-container">
  <input bind:value={countryName} />
  <button on:click={getDataByCountry}>Search</button>
  <button on:click={getAllData}>Reset</button>
</div>
<div class="row">
  <Card type="infected" value={data.cases ? data.cases : '0'} />
  <Card type="deaths" value={data.deaths ? data.deaths : '0'} />
  <Card type="recovered" value={data.recovered ? data.recovered : '0'} />
</div>

<div class="chart-container">
  <canvas id="chart" width="400" height="400" />
</div>

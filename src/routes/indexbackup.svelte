<script>
  import { onMount, afterUpdate } from "svelte";
  import Chart from "chart.js";

  import Card from "../components/Card.svelte";

  let data = {};
  let countryName = "";
  let ctx;
  let myChart;

  onMount(() => {
    getAllData();
  });

  const getAllData = async () => {
    const res = await fetch(`https://corona.lmao.ninja/all`);
    data = await res.json();
    drawChart();
    countryName = "";
  };

  const getDataByCountry = async () => {
    const response = await fetch(
      `https://corona.lmao.ninja/countries/${countryName}`
    );
    data = await response.json();
    drawChart();
  };

  const drawChart = () => {
    if (myChart) {
      myChart.destroy();
    }
    ctx = document.getElementById("chart");
    myChart = new Chart(ctx, {
      type: "pie",
      data: {
        labels: ["Active", "Recovered", "Death"],
        datasets: [
          {
            data: [data.active, data.recovered, data.deaths],
            backgroundColor: [
              "rgba(194, 42, 124, 1)",
              "rgba(20, 216, 112, 1)",
              "rgba(255, 128, 0, 1)"
            ],
            borderWidth: 0
          }
        ]
      }
    });
  };
</script>

<style>
  .search-container {
    display: grid;
    grid-template-columns: 4fr 1fr 1fr;
    grid-gap: 10px;
    margin-bottom: 20px;
  }
  input {
    height: 30px;
    padding-inline-start: 10px;
    font-size: 14px;
    font-weight: 500;
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
  }
  .pie-chart {
    max-width: 400px;
  }
</style>

<svelte:head>
  <title>Go corona go!</title>
</svelte:head>
<h1>
  {#if !data.country}Global data{:else}{`Showing data for ${data.country}`}{/if}
</h1>
<h2>Filter by country</h2>
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
  <canvas class="pie-chart" id="chart" width="400" height="400" />
</div>

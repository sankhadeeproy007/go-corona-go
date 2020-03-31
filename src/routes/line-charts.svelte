<script>
  import Chart from "chart.js";
  import { onMount } from "svelte";

  let data = {};
  let countryName = "China";
  let chart;
  const COUNTRIES = ["China", "India", "US", "Italy", "Spain"];

  const reset = () => {
    countryName = "China";
    drawChart();
  };

  onMount(async () => {
    const response = await fetch(
      `https://pomber.github.io/covid19/timeseries.json`
    );
    data = await response.json();
    drawChart();
  });

  const drawChart = (userCountry = "") => {
    const searchCountry = userCountry ? userCountry : countryName;
    const country = data[searchCountry];
    const dates = country.map(day => day.date);
    const confirmed = country.map(day => day.confirmed);
    const recovered = country.map(day => day.recovered);
    const deaths = country.map(day => day.deaths);
    const ctx = document.getElementById("chart");
    if (chart) {
      chart.data.datasets[0].data = confirmed;
      chart.data.datasets[1].data = deaths;
      chart.data.datasets[2].data = recovered;
      chart.update();
      return;
    }
    chart = new Chart(ctx, {
      type: "line",
      data: {
        labels: dates,
        datasets: [
          {
            label: "Cases",
            data: confirmed,
            backgroundColor: ["transparent"],
            borderColor: ["rgb(194, 42, 124)"],
            borderWidth: 2
          },
          {
            label: "Deaths",
            data: deaths,
            backgroundColor: ["transparent"],
            borderColor: ["rgb(255, 128, 0)"],
            borderWidth: 2
          },
          {
            label: "Recovered",
            data: recovered,
            backgroundColor: ["transparent"],
            borderColor: ["rgb(20, 216, 112)"],
            borderWidth: 2
          }
        ]
      },
      options: {}
    });
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
  .buttons {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 10px;
    margin-bottom: 12px;
    height: 40px;
  }
</style>

<svelte:head>
  <title>More charts</title>
</svelte:head>

<h2>Search by country</h2>
<div class="search-container">
  <input bind:value={countryName} />
  <button on:click={drawChart}>Search</button>
  <button on:click={reset}>Reset</button>
</div>
<div class="buttons">
  {#each COUNTRIES as country}
    <button on:click={() => drawChart(country)}>{country}</button>
  {/each}
</div>
<canvas id="chart" width="16" height="9" />

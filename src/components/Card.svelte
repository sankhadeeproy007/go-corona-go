<script>
  export let type;
  export let value;
</script>

<style>
  span {
    font-size: 26px;
    font-weight: 500;
  }
  .data-block {
    padding: 20px;
    border-radius: 4px;
    font-size: 20px;
  }
  .infected {
    background: rgb(87, 41, 240);
    background: linear-gradient(
      180deg,
      rgba(87, 41, 240, 1) 0%,
      rgba(161, 42, 160, 1) 100%,
      rgba(194, 42, 124, 1) 100%
    );
    color: white;
  }
  .deaths {
    background: rgb(119, 8, 23);
    background: linear-gradient(
      180deg,
      rgba(119, 8, 23, 1) 0%,
      rgba(153, 15, 64, 1) 35%,
      rgba(255, 128, 0, 1) 100%
    );
    color: white;
  }
  .recovered {
    background: rgb(16, 78, 25);
    background: linear-gradient(
      90deg,
      rgba(16, 78, 25, 1) 0%,
      rgba(18, 153, 15, 0.9248074229691877) 39%,
      rgb(20, 216, 112) 100%
    );
    color: white;
  }
</style>

<div class="data-block {type}">
  Total {type}:
  <br />
  <span>{new Intl.NumberFormat('en-IN', {}).format(value)}</span>
</div>

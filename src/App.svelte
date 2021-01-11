<script>
  import "bulma/css/bulma.css";

  let regInput;
  let vehiclePromise;

  const fetchRegData = async (regNr) => {
    const res = await fetch(`https://v1.motorapi.dk/vehicles/${regNr}`, {
      headers: {
        "X-AUTH-TOKEN": "31bn0uzchh3i3lf4f9viclzrjaorspwi",
      },
    });
    const json = res.json();
    if (res.ok) {
      return json;
    } else {
      throw new Error(json);
    }
  };

  const handleSubmit = () => {
    vehiclePromise = fetchRegData(regInput);
  };
</script>

<style global>
  :global(body) {
    padding: 0;
  }
  .field.has-addons {
    justify-content: center !important;
  }
  /* .input {
    text-transform: uppercase;
  } */
</style>

<section class="hero is-info">
  <div class="hero-body">
    <div class="container has-text-centered">
      <h1 class="title is-1">Søg køretøj information</h1>
      <form on:submit|preventDefault={handleSubmit} class="field has-addons">
        <div class="control">
          <input
            class="input is-expanded is-large"
            type="text"
            maxlength="7"
            required
            placeholder="AT21931"
            bind:value={regInput} />
        </div>
        <div class="control">
          <button
            type="submit"
            disabled={!regInput || !regInput.match(/[a-zA-Z]{2}[\d]{5}/g)}
            class="button is-large is-primary">
            Søg
          </button>
        </div>
      </form>
    </div>
  </div>
</section>
{#if vehiclePromise}
  {#await vehiclePromise}
    <section class="section">
      <div class="container">
        <progress class="progress is-small is-primary" max="100">15%</progress>
      </div>
    </section>
  {:then vehicle}
    <section class="section">
      <div class="container">
        <pre>{JSON.stringify(vehicle, 0, 2)}</pre>
      </div>
    </section>
  {/await}
{/if}

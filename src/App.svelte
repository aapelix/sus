<script>
  import { onMount } from "svelte";

  let response_url = "";
  let favorites = [];

  function get_url() {
    fetch("https://api.waifu.im/search?is_nsfw=true")
      .then((response) => {
        if (response.ok) {
          return response.json();
        } else {
          throw new Error(
            "Request failed with status code: " + response.status
          );
        }
      })
      .then((data) => {
        console.log(data.images[0].url);
        response_url = data.images[0].url;
      })
      .catch((error) => {
        console.error("An error occurred:", error.message);
      });
  }

  function get_favorites() {
    let favorite_count = Number(localStorage.getItem("favorite_num"));

    for (let i = 0; i <= favorite_count; i++) {
      favorites.push(localStorage.getItem("favorite_" + i));
    }

    console.log(favorites.length);
  }

  onMount(() => {
    get_url();

    if (localStorage.getItem("favorite_num") != null) {
      favorite_num = Number(localStorage.getItem("favorite_num")) + 1;
    }
  });

  let favorite_num = 0;
</script>

<main class="flex justify-center items-center h-screen w-screen flex-col">
  <div>
    <div
      class="w-screen h-screen bg-black flex justify-center items-center p-5"
    >
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <img
        src={response_url}
        class="rounded-3xl w-auto h-full"
        alt=""
        on:click={get_url}
      />
    </div>

    <button
      class="p-5 m-10 text-3xl bg-black rounded-3xl absolute top-0"
      on:click={() => {
        localStorage.setItem("favorite_" + favorite_num, response_url);
        localStorage.setItem("favorite_num", favorite_num.toString());
        favorite_num++;
      }}>Like</button
    >
  </div>

  <div
    class="flex flex-wrap absolute top-[50vw] flex-row items-center justify-center"
  >
    {get_favorites()}
    {#each favorites as mogus}
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <img
        src={mogus}
        class="max-w-[30vw] m-10 rounded-3xl h-auto bg-contain hover:scale-150 duration-300"
        alt=""
        on:click={() => {
          window.open(mogus);
        }}
      />
    {/each}
  </div>
</main>

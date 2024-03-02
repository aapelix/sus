<script>
  import { onMount } from "svelte";

  // SMSMSMM

  let response_url = "";

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

  onMount(() => {
    get_url();

    if (localStorage.getItem("favorite_num") != null) {
      favorite_num = Number(localStorage.getItem("favorite_num")) + 1;
    }
  });

  let favorite_num = 0;
</script>

<main class="flex justify-center items-center h-screen w-screen">
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <img
    src={response_url}
    class="rounded-3xl w-[30vw]"
    alt=""
    on:click={get_url}
  />

  <button
    class="p-5 m-10 text-3xl bg-black rounded-3xl"
    on:click={() => {
      localStorage.setItem("favorite_" + favorite_num, response_url);
      localStorage.setItem("favorite_num", favorite_num.toString());
      favorite_num++;
    }}>Like</button
  >
</main>

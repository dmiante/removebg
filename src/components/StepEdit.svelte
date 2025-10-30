<script lang="ts">
  import "two-up-element"
  import { originalImage, modifiedImage } from "../store"

  let processingImage = true
  let tries = 0
  let intervalId: any

  $: {
    if (processingImage) {
      clearInterval(intervalId)
      intervalId = setInterval(() => {
        tries++
        const img = new Image()
        img.src = $modifiedImage
        img.onload = () => {
          processingImage = false
          clearInterval(intervalId)
        }
      }, 500)
    }
  }
</script>

<div class="flex flex-col items-center justify-center">
  {#if processingImage}
  <div class="flex items-center justify-center">
    <button type="button" class="inline-flex px-10 py-4 font-semibold leading-6 text-xl shadow rounded-md text-white bg-fuchsia-900 hover:bg-fuchsia-900/75 transition ease-in-out duration-150 cursor-not-allowed" disabled>
      <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
      </svg>
      Processing...
    </button>
  </div>
    {:else}
    <div class="flex flex-col items-center justify-center gap-10">
      <two-up>
        <img src={$originalImage} alt="Imagen original subida por el usuario" />
        <img src={$modifiedImage} alt="Imagen sin fondo subida por el usuario" />
      </two-up>
      <a
        download
        href={$modifiedImage}
        class="block bg-blue-500 hover:bg-blue-700 text-xl text-center w-full font-bold text-white rounded-full px-8 py-5 mb-20 lg:cursor-pointer lg:w-fit transition duration-300"
      >
        Download image without background
      </a>
    </div>
    {/if}
</div>
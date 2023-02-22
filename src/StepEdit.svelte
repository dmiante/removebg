<script lang="ts">
  import "two-up-element"
  import { originalImage, modifiedImage } from "./store"

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

<!-- <two-up>
  <img src={$originalImage} alt="Imagen original subida por el usuario" />
  {#if processingImage}
    <div class="flex flex-col justify-center items-center">
      <p class="text-center mt-4">Procesando imagen...</p>
    </div>
  {:else}
    <img src={$modifiedImage} alt="Imagen sin fondo subida por el usuario" />
  {/if}
</two-up> -->
 {#if processingImage}
  <button type="button" class="inline-flex items-center px-4 py-2 font-semibold leading-6 text-sm shadow rounded-md text-white bg-red-800 hover:bg-indigo-400 transition ease-in-out duration-150 cursor-not-allowed" disabled>
    <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
    </svg>
    Processing...
  </button>
  {:else}
  <two-up>
    <img src={$originalImage} alt="Imagen original subida por el usuario" />
    <img src={$modifiedImage} alt="Imagen sin fondo subida por el usuario" />
  </two-up>
  {/if}


<a
  download
  href={$modifiedImage}
  class="block bg-blue-500 hover:bg-blue-700 text-xl text-center w-full font-bold text-white rounded-full px-4 py-2 mt-10"
>
  Descargar imagen sin fondo
</a>
<script lang="ts">
  import CloudinaryLogo from "./CloudinaryLogo.svelte";
  import StepEdit from "./StepEdit.svelte";
  import StepUpload from "./StepUpload.svelte";
  import { imageStatus } from "./store";
  import { ImageStatus } from "./types.d";
</script>

<div
  class="max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4"
>
  <header class="flex flex-col text-center py-10">
    <h1 class="text-5xl font-bold text-red-600 tracking-tighter">
      Remove<span class="text-blue-600">BG</span>
    </h1>
    <p class="text-base pt-10">Remove background of your photos. Easy, Simple and <strong class="text-xl">FREE!!!</strong></p>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

<footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
  Made with <a href="https://cloudinary.com" target="_blank" rel="noreferrer"
    ><CloudinaryLogo /></a
  >
</footer>
</div>

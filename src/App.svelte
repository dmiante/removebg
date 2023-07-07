<script lang="ts">
  import StepEdit from "./components/StepEdit.svelte";
  import StepUpload from "./components/StepUpload.svelte";
  import Footer from "./pages/Footer.svelte";
  import Header from "./pages/Header.svelte";
  import { imageStatus } from "./store";
  import { ImageStatus } from "./types.d";
</script>

<div
  class=""
>
  <Header />
  <main class="my-20">
    <p class="text-3xl text-white font-mono text-center my-5">
      Remove background of your photos. Easy, Simple and <strong class="text-6xl">FREE!!!</strong>
    </p>
    <div class="flex flex-col justify-center items-center my-10">
      {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
        <StepUpload />
      {:else if $imageStatus === ImageStatus.DONE}
        <StepEdit />
      {/if}
    </div>
  </main>
  <Footer />
</div>

<script lang="ts">
  import StepEdit from "./components/StepEdit.svelte";
  import StepUpload from "./components/StepUpload.svelte";
  import Footer from "./pages/Footer.svelte";
  import Header from "./pages/Header.svelte";
  import { imageStatus } from "./store";
  import { ImageStatus } from "./types.d";
</script>

<div
  class="min-h-screen flex flex-col container mx-auto p-4"
>
  <Header />
  <main class="flex w-full flex-1 flex-col justify-center gap-10 lg:items-center">
    <p class="text-3xl text-white font-mono text-center mb-20">
      Remove background of your photos. Easy, Simple and <strong class="text-6xl">FREE!!!</strong>
    </p>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>
  <Footer />
</div>

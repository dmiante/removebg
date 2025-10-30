<script>
  import { Cloudinary } from "@cloudinary/url-gen"
  import { backgroundRemoval } from "@cloudinary/url-gen/actions/effect"
  import Dropzone from "dropzone";
  import "dropzone/dist/dropzone.css";

  import { ImageStatus } from "../types.d";
  import { imageStatus, modifiedImage, originalImage } from "../store";

  import { onMount } from "svelte";

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: "dmi4n",
    },
    url: {
      secure: true,
    },
  })

  onMount(() => {
    const dropzone = new Dropzone("#dropzone", {
      uploadMultiple: false,
      acceptedFiles: ".jpg,.png,.webp",
      maxFiles: 1,
    });

    dropzone.on("sending", (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING);
      // aqui podemos añadir la apiKey, configuracion
      formData.append("upload_preset", "qkffffub");
      formData.append("timestamp", Date.now() / 1000);
      formData.append("api_key", 789238926772466);
    });

    dropzone.on("success", (file, response) => {
      const { public_id: publicId, secure_url: url } = response;

      // crear la imagen con fondo transparente
      // y guardar en el backgroundImage
      const imageWithoutBackground = cloudinary
        .image(publicId)
        .effect(backgroundRemoval());

      console.log(imageWithoutBackground.toURL());

      imageStatus.set(ImageStatus.DONE);
      modifiedImage.set(imageWithoutBackground.toURL());
      originalImage.set(url);
    });

    dropzone.on("error", (file, response) => {
      console.log("HA IDO MAL");
      console.log(response);
    });
  });
</script>

<form
  id="dropzone"
  class="border-dashed border-2 border-[#4a044e] rounded-lg w-full flex items-center justify-center flex-col hover:bg-[#4a044e] py-10 lg:cursor-pointer lg:max-w-6xl"
  action="https://api.cloudinary.com/v1_1/midudev/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="font-bold pointer-events-none bg-blue-800 rounded-full text-bold text-white text-xl px-6 py-4 flex items-center gap-2 hover:bg-blue-600 transition"
    >
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-upload-icon lucide-upload"><path d="M12 3v12"/><path d="m17 8-5-5-5 5"/><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/></svg>
      Upload photo
    </button>
    <strong class="text-lg mt-4 text-gray-400 text-center">
      Drag & drop your image here, or click to select files
    </strong>
  {:else if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-white text-lg flex items-center gap-2">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-loader-circle-icon lucide-loader-circle animate-spin"><path d="M21 12a9 9 0 1 1-6.219-8.56"/></svg>
      Uploading files...
    </strong>
  {/if}
</form>

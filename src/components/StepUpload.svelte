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
  class="shadow-2xl border-dashed border-2 border-[#4a044e] rounded-lg aspect-video w-2/5 flex items-center justify-center flex-col cursor-pointer hover:bg-[#4a044e]"
  action="https://api.cloudinary.com/v1_1/midudev/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="font-bold pointer-events-none bg-blue-800 rounded-full text-bold text-white text-xl px-6 py-4"
    >
      Upload files
    </button>
    <strong class="text-lg mt-4 text-gray-400">or drop a file</strong>
  {:else if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-white text-lg mt-4">Uploading files...</strong>
  {/if}
</form>

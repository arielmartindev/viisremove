<script lang="ts">
  import "./app.css";
  import CloudinaryLogo from "./assets/CloudinaryLogo.svelte";
    import StepUpload from "./assets/StepUpload.svelte";
    import StepEdit from "./StepEdit.svelte";
    import { imageStatus } from "./store.js";
    import { ImageStatus } from "./type.d";
</script>

<div class="max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4">
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-blue-900 tracking-tighter">
      Viis Remove<span class="text-blue-600">BG</span>
    </h1>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
    <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
    <StepEdit/>
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
    Hecho con <a href="https://cloudinary.com" target="_blank" rel="noreferrer"
      ><CloudinaryLogo /></a
    >
  </footer>
</div>

<script lang="ts">
    import "two-up-element";
    import { originalImage, modifiedImage } from "./store";

    let processingImage = true
    let tries = 0
    let intervalId: any

    $: {
        if (processingImage) {
            clearInterval(intervalId);
            intervalId = setInterval(() => {
                tries++;
            }, 500);
        }
    }
</script>

<two-up>
    <img src={$originalImage} alt="Imagen original subida por el usuario" />
    <img
        on:load={() => (processingImage = false)}
        on:error={() => (processingImage = true)}
        src={`${$modifiedImage}&t=${tries}`}
        alt="Imagen sin fondo subida por el usuario"
    />
</two-up>

<a
    download
    href={$modifiedImage}
    class="block hover:bg-blue-700 bg-blue-500 w-full font-bold text-center text-white rounded-full px-4 py-2 mt-10"
>
    Descargar Imagen sin fondo
</a>

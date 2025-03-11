<script lang="ts">
    import { Cloudinary } from "@cloudinary/url-gen"
    import { backgroundRemoval } from "@cloudinary/url-gen/actions/effect"
    import { ImageStatus } from "../type.d";
    import { imageStatus, originalImage, modifiedImage } from "../store";
    import Dropzone from "dropzone"
    import "dropzone/dist/dropzone.css"

    import { onMount } from "svelte"

    const cloudinary = new Cloudinary({
        cloud: {
            cloudName: 'arielmartindev'
        },
        url: {
            secure: true
        }
    })

    onMount(() => {

        const dropzone = new Dropzone('#dropzone', {
            uploadMultiple: false,
            acceptedFiles: '.jpg, .png, .webp',
            maxfiles: 1,
        })

        dropzone.on('sending', (file, xhr, formData) => {
            imageStatus.set(ImageStatus.UPLOADING)
            // aqui podemos añadir la apikey, configuracion, etc
            formData.append('upload_preset', 'viisremove')
            formData.append('timestamp', (Date.now() / 1000))
            formData.append('api_key', 184122477374432)
        })

        dropzone.on('success', (file, response) => {

            console.log(response)
            console.log('Ha ido bien')

            const {
                secure_url: url,
                public_id: publicId
            } = response

            //crear imagen con fondo transparente
            const imageWithoutBackground = cloudinary
                .image(publicId)
                .effect(backgroundRemoval())

               console.log( imageWithoutBackground.toURL() ) 

            //guardar en el backgroundImage
            imageStatus.set(ImageStatus.DONE)
            originalImage.set(url)
            modifiedImage.set( imageWithoutBackground.toURL() )

            


        console.log('Ha ido BIEN!')
 
        })

        dropzone.on("error", (file, response) => {
        console.log('Ha ido MAL!')
        console.log(response)
        })

    })
</script>

<form
    id="dropzone"
    class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col"
    action="https://api.cloudinary.com/v1_1/arielmartindev/image/upload"
>
    {#if $imageStatus === ImageStatus.READY}
        <button
            class="font-bold pointer-event-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-2"
        >
            Upload files
        </button>
        <strong class="text-lg mt-4 text-gray-800">or drop a file</strong>
    {/if}

    {#if $imageStatus === ImageStatus.UPLOADING}
        <strong class="text-lg mt-4 text-gray-800">Uploading files...</strong>
    {/if}

</form>

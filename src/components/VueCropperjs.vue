<template>
<div id="app">
    <input type="file" name="image" accept="image/*"
        style="font-size: 1.2em; padding: 10px 0;"
        @change='setImage'
    />
    <br />
    <div style="max-width: 900px; display: inline-block;">
        <vue-cropper
            ref='cropper'
            :guides='guides'
            :view-mode='viewMode'
            drag-mode="crop"
            :aspect-ratio='aspectRatio'
            :auto-crop-area='autoCropArea'
            :min-container-width='minContainerWidth'
            :min-container-height='minContainerHeight'
            :background='background'
            :rotatable='rotatable'
            :src='imgSrc'
            alt="Source Image"
            :img-style='imgStyle'
            :cropmove='cropImage'
            >
        </vue-cropper>
    </div>
    <img
        :src='cropImg'
        style="width: 200px; height: 150px; border: 1px solid gray"
        alt="Cropped Image"
    />
    <br/>
    <button v-show='!!this.imgSrc' @click='rotate'>Rotate</button>
</div>
</template>

<script>
import VueCropper from 'vue-cropperjs'

export default {
    name: 'hello',
    components: {
        VueCropper
    },
    data () {
        return {
            imgSrc: '',
            cropImg: '',
            guides: true,
            viewMode: 2,
            autoCropArea: 0.5,
            minContainerWidth: 250,
            minContainerHeight: 180,
            background: true,
            rotatable: true,
            imgStyle: {width: '400px', 'height': '300px'},
            aspectRatio: 16 / 9
        }
    },
    methods: {
        setImage (e) {
            const file = e.target.files[0]

            if (!file.type.includes('image/')) {
                alert('Please select an image file')
                return
            }

            if (typeof FileReader === 'function') {
                const reader = new FileReader()

                reader.onload = (event) => {
                    this.imgSrc = event.target.result
                    // rebuild cropperjs with the updated source
                    console.log('this.$refs.cropper =')
                    console.log(this.$refs.cropper)
                    this.$refs.cropper.replace(event.target.result)
                }

                reader.readAsDataURL(file)
            } else {
                alert('Sorry, FileReader API not supported')
            }
        },
        cropImage () {
            // get image data for post processing, e.g. upload or setting image src
            this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL()
        },
        rotate () {
            this.$refs.cropper.rotate(90)
        }
    }
}
</script>

<style>

</style>

<template>
    <div class="grid-container-create-columns cmd-imagezoom">
        <div class="grid-small-item">
            <a href="#" class="thumbnails-imagezoom">
                <img :src="smallImageUrl" alt="Small Image" @mouseover="onMouseOver" @mousemove="onMouseMove" @mouseout="onMouseOut" />
            </a>
        </div>
        <div v-if="showLargeImage" class="zoom-container grid-large-item">
            <img :src="largeImageUrl" alt="Large Image" />
        </div>
        <div v-if="showLargeImage" class="zoom-overlay"></div>
    </div>
</template>

<script>
    export default {
        name: "CmdImageZoom",
        props: {
            smallImageUrl: {
                type: String,
                required: true
            },
            largeImageUrl: {
                type: String,
                required: true
            }
        },
        data() {
            return {
                showLargeImage: false
            }
        },
        methods: {
            onMouseOver(e) {
                this.showLargeImage = true
                this.$nextTick(() => {
                    const zoomContainerHeight = window.innerHeight - this.$el.getBoundingClientRect().y - 10
                    const zoomContainer = this.$el.querySelector('.zoom-container')
                    zoomContainer.style.height = `${zoomContainerHeight}px`
                    positionOverlay(this, e)
                })
            },
            onMouseMove(e) {
                positionOverlay(this, e)
            },
            onMouseOut() {
                this.showLargeImage = false
            }
        }
    }

    function positionOverlay(vm, e) {
        const largeImageContainer = vm.$el.querySelector('.zoom-container')
        const largeImage = largeImageContainer.querySelector('img')
        const zoomOverlay = vm.$el.querySelector('.zoom-overlay')

        const smallImageWidth = e.target.getBoundingClientRect().width
        const smallImageHeight = e.target.getBoundingClientRect().height

        const largeImageWidth = largeImage.getBoundingClientRect().width
        const largeImageHeight = largeImage.getBoundingClientRect().height

        const mouseX = e.pageX - e.target.getBoundingClientRect().x - window.pageXOffset
        const mouseY = e.pageY - e.target.getBoundingClientRect().y - window.pageYOffset

        const overlayWidth = Math.min(smallImageWidth, largeImageContainer.getBoundingClientRect().width / largeImageWidth * smallImageWidth)
        const overlayHeight = Math.min(smallImageHeight, largeImageContainer.getBoundingClientRect().height / largeImageHeight * smallImageHeight)

        const overlayX = clamp(mouseX - overlayWidth / 2, 0, smallImageWidth - overlayWidth)
        const overlayY = clamp(mouseY - overlayHeight / 2, 0, smallImageHeight - overlayHeight)

        const largeImageMarginLeft = -(overlayX * (largeImageWidth / smallImageWidth))
        const largeImageMarginTop = -(overlayY * (largeImageHeight / smallImageHeight))

        zoomOverlay.style.left = `${overlayX}px`
        zoomOverlay.style.top = `${overlayY}px`
        zoomOverlay.style.width = `${overlayWidth}px`
        zoomOverlay.style.height = `${overlayHeight}px`
        zoomOverlay.style.display = 'block'

        largeImage.style.marginLeft = `${largeImageMarginLeft}px`
        largeImage.style.marginTop = `${largeImageMarginTop}px`
    }

    function clamp(value, min, max) {
        if (value < min) {
            return min
        }
        if (value > max) {
            return max
        }
        return value
    }
</script>

<style lang="scss">
/* begin cmd-imagezoom --------------------------------------------------------------------------------------------------------------------------------------------------- */
.cmd-imagezoom {
    overflow: hidden;

    .zoom-container {
        display: block !important;
        overflow: hidden;
        max-width: 100rem;
        max-height: 50rem !important;

        > img {
            max-width: none;
        }
    }

    .zoom-overlay {
        position: absolute;
        display: none;
        pointer-events: none;
    }
}
/* end cmd-imagezoom --------------------------------------------------------------------------------------------------------------------------------------------------- */
</style>
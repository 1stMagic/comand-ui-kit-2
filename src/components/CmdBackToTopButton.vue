<template>
    <!-- begin cmd-back-to-top-button -->
    <transition name="fade">
        <a :class="iconClass" class="cmd-back-to-top-button" href="#" :title="tooltip" @click.prevent="onBackToTop" v-if="show"></a>
    </transition>
    <!-- end cmd-back-to-top-button -->
</template>

<script>
    export default {
        name: "CmdBackToTopButton",
        data() {
            return {
                windowInnerHeight: window.innerHeight,
                windowScrollY: window.scrollY,
                bodyScrollHeight: document.body.scrollHeight
            }
        },

        props: {
            iconClass: {
                type: String,
                default: "icon-single-arrow-up"
            },
            tooltip: {
                type: String,
                required: false
            }
        },

        created() {
            window.addEventListener('resize', this.onViewportChange);
            window.addEventListener('scroll', this.onViewportChange);
        },

        unmounted() {
            window.removeEventListener('resize', this.onViewportChange);
            window.removeEventListener('scroll', this.onViewportChange);
        },

        /* watch for changes */
        computed:  {
            show() {
                return this.windowScrollY > 0 && this.windowInnerHeight < this.bodyScrollHeight;
            }
        },

        methods: {
            onViewportChange(){
                this.windowInnerHeight = window.innerHeight;
                this.windowScrollY = window.scrollY;
                this.bodyScrollHeight = document.body.scrollHeight;
            },

           onBackToTop() {
                window.scrollTo({top: 0, left: 0, behavior: 'smooth'});
           }
        }
    }
</script>

<style lang="scss">
/* begin cmd-back-to-top-button --------------------------------------------------------------------------------------------------------------------------------------------------- */
.cmd-back-to-top-button {
    padding: var(--default-padding);
    display: inline-block;
    position: fixed;
    right: 1rem;
    bottom: 1rem;
    text-decoration: none;
    border-radius: var(--full-circle);
    z-index: 300;
}
/* cmd-back-to-top-button --------------------------------------------------------------------------------------------------------------------------------------------------- */
</style>
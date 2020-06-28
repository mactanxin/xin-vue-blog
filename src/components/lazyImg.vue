<template>
    <img
        :data-src="lazySrc"
        :data-srcset="lazySrcset"
        :style="style"
        class="AppImage"
        :alt="alt"
    />
</template>

<script>
import lozad from 'lozad';
export default {
    name: 'LazyImg',
    props: {
        backgroundColor: {
            type: String,
            default: '#efefef',
        },
        height: {
            type: Number,
            default: null,
        },
        lazySrc: {
            type: String,
            default: null,
        },
        lazySrcset: {
            type: String,
            default: null,
        },
        width: {
            type: Number,
            default: null,
        },
        alt: {
            tyep: String,
            default: 'img'
        }
    },
    data() {
        return {
            loading: true
        }
    },
    computed: {
        aspectRatio() {
            if(!this.width || !this.height){
                return null;
            }
            return (this.height / this.width) * 100;
        },
        style() {
            const style = { backgroundColor: this.backgroundColor };
            if (this.width){
                style.width = `${this.width}px`;
            }
            const applyAspectRatio = this.loading && this.aspectRatio;
            if (applyAspectRatio) {
                style.height = 0;
                style.paddingTop = `${this.aspectRatio}%`;
            }
            return style;
        }
    },
    mounted() {
        const setLoadingState = () => {
          this.loading = false;
        };
        this.$el.addEventListener('load', setLoadingState);
        this.$once('hook:destroyed', ()=>{
            this.$el.removeEventListener('load', setLoadingState);
        });
        const observer = lozad(this.$el);
        observer.observe();
    }
};
</script>

<style lang="scss" scoped>
.AppImage {
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  vertical-align: middle;
}
</style>

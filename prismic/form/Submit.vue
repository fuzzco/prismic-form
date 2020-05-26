<template>
    <input class="prismic-form-submit" type="submit" :value="text" />
</template>

<script>
export default {
    props: {
        text: {
            type: String,
            default: 'Submit'
        }
    }
}
</script>

<style lang="scss">
.prismic-form-submit {
    @include button-reset;
    @include cta-button;
}
</style>

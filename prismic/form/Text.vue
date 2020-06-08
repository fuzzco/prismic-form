<template>
    <prismic-content
        class="prismic-form-content content-reset"
        :content="text"
    />
</template>

<script>
export default {
    props: {
        text: {
            type: Array,
            default: () => []
        }
    }
}
</script>

<style lang="scss">
.prismic-form-content {
}
</style>

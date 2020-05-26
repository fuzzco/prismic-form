<template>
    <section class="prismic-form-success">
        <slice-content
            :content="cmpContent"
            class="slice"
            v-if="ready && cmpContent"
        />
        <slice-diptych
            :left_image="formData.success_left_image"
            :right_image="formData.success_right_image"
            :attribution="formData.success_attribution"
            class="slice"
            v-if="ready"
        />
    </section>
</template>

<script>
import { fetchByType } from '~/libs/prismic'

export default {
    props: {
        form: {
            form: {
                type: Object,
                required: true
            }
        }
    },
    data() {
        return {
            ready: false,
            prismicData: null
        }
    },
    async mounted() {
        // fetch form
        const results = await fetchByType({
            type: 'form',
            slug: this.form.slug
        })

        // try to set up form data
        if (results) {
            this.prismicData = results
            this.$emit('form-fetched', results)
        } else {
            this.$emit('fetch-failed')
        }

        // ready!
        this.ready = true
    },
    computed: {
        formData() {
            return _get(this.prismicData, 'data', {})
        },
        cmpContent() {
            return _get(this.formData, 'success_content')
        }
    }
}
</script>

<style lang="scss">
.prismic-form-success {
}
</style>

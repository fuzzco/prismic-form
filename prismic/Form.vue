<template>
    <div class="prismic-form">
        <!-- Heading -->
        <h2 class="form-heading" v-if="formData.form_heading">
            {{ formData.form_heading }}
        </h2>

        <!-- Form -->
        <form :class="['form']" :action="formData.action" @submit="onSubmit">
            <component
                v-for="(slice, i) in slices"
                :key="i"
                :is="cmpComponents[i]"
                v-bind="{ ...slice.primary, items: slice.items }"
            />
        </form>

        <slot name="success" v-if="submitted && !error">
            <p class="success-message">
                Form submitted successfully!
            </p>
        </slot>

        <!-- Error -->
        <slot name="error" v-if="error" v-bind:error="{ error }">
            <p class="error-message">
                Something went wrong. Please try again later.
            </p>
        </slot>
    </div>
</template>

<script>
import { prismicQuery } from '~/libs/prismic'
import { get, kebabCase } from 'lodash'
// import { TheMask } from 'vue-the-mask'
//
export default {
    props: {
        form: {
            type: Object,
            required: true,
        },
        error: {
            type: Boolean,
            default: false,
        },
        preventDefault: {
            type: Boolean,
            default: true,
        },
    },
    data() {
        return {
            prismicData: null,
            submitted: false,
        }
    },
    async fetch() {
        // fetch form
        const results = await prismicQuery({
            type: 'form',
            slug: this.form.slug,
            $prismic: this.$prismic,
        })

        // try to set up form data
        if (results) {
            this.prismicData = results
            this.$emit('form-fetched', results)
        } else {
            this.$emit('fetch-failed')
        }
    },
    computed: {
        formData() {
            return get(this.prismicData, 'data', {})
        },
        slices() {
            return get(this.formData, 'body', [])
        },
        cmpComponents() {
            return this.slices.map(
                (slice) => `prismic-form-${this.kebabCase(slice.slice_type)}`
            )
        },
    },
    methods: {
        kebabCase,
        onSubmit(evt) {
            if (this.preventDefault) {
                evt.preventDefault()
            }
            this.submitted = true
            if (this.formData.action) {
                this.$emit('submit', this.formData.action)
            }
        },
    },
}
</script>

<style lang="scss">
.prismic-form {
    input,
    select {
        @include input-reset;
    }
}
</style>

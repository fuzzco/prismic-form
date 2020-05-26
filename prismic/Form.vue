<template>
    <div class="prismic-form">
        <h2 class="form-heading">{{ formData.form_heading }}</h2>
        <form
            :class="['form', { ready }]"
            :action="formData.action"
            @submit="onSubmit"
        >
            <component
                v-for="(slice, i) in slices"
                :key="i"
                :is="cmpComponents[i]"
                v-bind="{ ...slice.primary, items: slice.items }"
            />
        </form>
        <p class="error-message" v-if="error">
            Something went wrong. Please try again later.
        </p>
    </div>
</template>

<script>
import { fetchByType } from '~/libs/prismic'
import { get, kebabCase } from 'lodash'
import { TheMask } from 'vue-the-mask'
//
export default {
    props: {
        form: {
            type: Object,
            required: true
        },
        error: {
            type: Boolean,
            default: false
        }
    },
    // components: {
    //     TheMask
    // },
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
            return get(this.prismicData, 'data', {})
        },
        slices() {
            return get(this.formData, 'body', [])
        },
        cmpComponents() {
            return this.slices.map(
                slice => `prismic-form-${this.kebabCase(slice.slice_type)}`
            )
        }
        // fields() {
        //     const output = []
        //     const raw = get(this.formData, 'fields', [])
        //     let fieldset = null
        //
        //     // loop through fields
        //     raw.forEach(field => {
        //         // if we're a fieldset...
        //         if (field.type === 'fieldset') {
        //             // ...either start a new set...
        //             if (fieldset === null) {
        //                 fieldset = [field]
        //             }
        //             // ...or complete and append an existing one
        //             else {
        //                 output.push(fieldset)
        //                 // clear the working fieldset
        //                 fieldset = null
        //             }
        //         }
        //
        //         // ...otherwise, if we're continuing a fieldset...
        //         else if (fieldset) {
        //             fieldset.push(field)
        //         }
        //
        //         // ...otherwise, just append this field
        //         else {
        //             output.push(field)
        //         }
        //     })
        //
        //     return output
        // }
    },
    methods: {
        // getFieldElementType(field) {
        //     switch (true) {
        //         case field.mask:
        //             return 'the-mask'
        //         case field.type === 'fieldset':
        //             return null
        //
        //         default:
        //             return 'input'
        //     }
        // },
        // overrideAttributes(field) {
        //     const output = {}
        //
        //     // split overrides by comma and trim whitespace
        //     const overrides = (field.overrides || '')
        //         .split(',')
        //         .map(str => str.trim())
        //
        //     // split overrides into key-values - example:
        //     // key:value
        //     overrides.forEach(str => {
        //         const matches = /([^:]*):(.*)/.exec(str)
        //
        //         if (matches && matches.length > 2) {
        //             // if we have enough matches, add to output
        //             output[matches[1]] = matches[2]
        //         }
        //     })
        //
        //     return output
        // },
        kebabCase,
        onSubmit(evt) {
            evt.preventDefault()
            if (this.formData.action) {
                this.$emit('submit', this.formData.action)
            }
        }
    }
}
</script>

<style lang="scss">
.prismic-form {
    .form-heading {
        @include fontSize(62px);

        @include bp(s) {
            @include fontSize(40px);
        }
    }

    .form {
        > :not(:first-child) {
            margin-top: 20px;

            @include bp(s) {
                margin-top: 30px;
            }
        }

        > .prismic-form-submit:last-child {
            margin-top: 40px;

            @include bp(s) {
                margin-top: 20px;
            }
        }
    }

    .error-message {
        color: red;
        margin-top: 35px;
    }
}
</style>

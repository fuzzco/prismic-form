<template>
    <input
        class="prismic-form-input"
        v-bind="options"
        :name="name"
        :id="kebabCase(name)"
        spellcheck="false"
    />
</template>

<script>
import { kebabCase } from 'lodash'

export default {
    props: {
        options: {
            type: Object,
            default: () => {},
        },
        name: {
            type: String,
            default: '',
        },
    },
    methods: {
        kebabCase,
    },
}
</script>

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
            default: () => {}
        },
        name: {
            type: String,
            default: ''
        }
    },
    methods: {
        kebabCase
    }
}
</script>

<style lang="scss">
.prismic-form-input {
    &:invalid {
        border-color: red;

        &:not(:focus) {
            color: red;
        }
    }

    &::placeholder {
    }

    &:placeholder-shown {
    }

    &:focus {
    }
}
</style>

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
    padding: 15px;
    @include fontSize(16px);
    border: 1px solid var(--grey);
    outline: none;
    transition: border-color 0.3s;

    &:invalid {
        border-color: red;

        &:not(:focus) {
            color: red;
        }
    }

    &:placeholder-shown {
        border-color: var(--grey);
    }

    &:focus {
        border-color: var(--teal);
    }

    &::placeholder {
        color: var(--dark-grey);
    }
}
</style>

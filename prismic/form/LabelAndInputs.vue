<template>
    <!-- Div with group role if more than 1 input -->
    <div
        v-if="items.length > 1"
        :class="cmpClasses"
        role="group"
        :aria-labelledby="label ? `${kebabCase(label)}-label` : ''"
    >
        <div v-if="label" class="inner-label" :id="`${kebabCase(label)}-label`">
            {{ label }}
        </div>
        <prismic-form-input
            v-for="(item, i) in items"
            :key="i"
            :options="item"
            :name="`${camelCase(label)}${i}`"
        />
    </div>

    <!-- Regular label if 1 input -->
    <label v-else class="prismic-form-label-and-inputs">
        <span v-if="label" class="inner-label">{{ label }}</span>
        <prismic-form-input
            v-for="(item, i) in items"
            :key="i"
            :options="item"
            :name="camelCase(label)"
        />
    </label>
</template>

<script>
// Note: Div with group role and aria-labelledby is used in place of fieldset
// due to fieldset styling issues in Chromium broswers:
// https://bugs.chromium.org/p/chromium/issues/detail?id=375693

import { camelCase, kebabCase } from 'lodash'

export default {
    props: {
        items: { type: Array, default: () => [] },
        label: { type: String, default: '' }
    },
    computed: {
        cmpClasses() {
            return this.label
                ? [
                      'prismic-form-label-and-inputs',
                      `${kebabCase(this.label)}-group`
                  ]
                : 'prismic-form-label-and-inputs'
        }
    },
    methods: {
        camelCase,
        kebabCase
    }
}
</script>

<style lang="scss">
.prismic-form-label-and-inputs {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-gap: 20px;

    @include bp(s) {
        grid-template-columns: 100%;
    }

    .inner-label {
        align-self: center;
    }

    .prismic-form-input {
        grid-column-start: 2;

        @include bp(s) {
            grid-column-start: initial;
        }
    }

    // Specific input type styles
    input[type='number']::-webkit-inner-spin-button,
    input[type='number']::-webkit-outer-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }

    // Specific group styles
    &.full-name-group {
        grid-template-columns: 200px repeat(2, 1fr);

        @include bp(s) {
            grid-template-columns: 100%;
        }

        .prismic-form-input {
            grid-column-start: initial;
        }
    }

    &.physical-address-group {
        grid-template-columns: 200px repeat(2, 1fr);

        @include bp(s) {
            grid-template-columns: 100%;
        }

        #physical-address-0 {
            grid-column: span 2;

            @include bp(s) {
                grid-column: initial;
            }
        }
        #physical-address-2 {
            grid-column: initial;
        }
    }
}
</style>

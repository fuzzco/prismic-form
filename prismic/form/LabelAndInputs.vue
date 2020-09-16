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
        label: { type: String, default: '' },
    },
    computed: {
        cmpClasses() {
            return this.label
                ? [
                      'prismic-form-label-and-inputs',
                      `${kebabCase(this.label)}-group`,
                  ]
                : 'prismic-form-label-and-inputs'
        },
    },
    methods: {
        camelCase,
        kebabCase,
    },
}
</script>

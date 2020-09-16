<template>
    <div
        class="prismic-form-checkboxes"
        role="group"
        :aria-labelledby="`${kebabCase(label)}-label`"
    >
        <div v-if="label" class="inner-label" :id="`${kebabCase(label)}-label`">
            {{ label }}
        </div>

        <p v-for="(item, i) in items" :key="i" class="single-checkbox">
            <input
                type="checkbox"
                :name="camelCase(item.checkbox_label)"
                :id="`${kebabCase(item.checkbox_label)}-${i}`"
                class="checkbox-input"
            />
            <label
                :for="`${kebabCase(item.checkbox_label)}-${i}`"
                class="checkbox-label"
            >
                {{ item.checkbox_label }}
            </label>
        </p>
    </div>
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
    methods: {
        camelCase,
        kebabCase,
    },
}
</script>

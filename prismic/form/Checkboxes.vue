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
        label: { type: String, default: '' }
    },
    methods: {
        camelCase,
        kebabCase
    }
}
</script>

<style lang="scss">
.prismic-form-checkboxes {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-column-gap: 20px;

    @include bp(s) {
        grid-template-columns: 100%;
    }

    .inner-label {
        align-self: center;
    }

    // p {
    //     grid-column-start: 2;
    //     margin: 10px 0;
    // }

    .single-checkbox {
        grid-column-start: 2;
        border-right: 1px solid var(--grey);
        border-left: 1px solid var(--grey);
        padding: 15px 15px 0;
        margin: 0;

        @include bp(s) {
            grid-column-start: initial;
        }

        &:first-of-type {
            border-top: 1px solid var(--grey);
            padding-top: 20px;

            @include bp(s) {
                margin-top: 20px;
            }
        }

        &:last-of-type {
            border-bottom: 1px solid var(--grey);
            padding-bottom: 20px;
        }
    }

    /* Base for label styling */
    [type='checkbox']:not(:checked),
    [type='checkbox']:checked {
        position: absolute;
        opacity: 0;
        pointer-events: none;
    }
    [type='checkbox']:not(:checked) + label,
    [type='checkbox']:checked + label {
        position: relative;
        padding-left: 25px;
        cursor: pointer;
        transition: color 0.2s;
        display: block;
        color: var(--black);
    }
    [type='checkbox']:not(:checked) + label {
        color: var(--dark-grey);
    }

    /* checkbox aspect */
    [type='checkbox']:not(:checked) + label:before,
    [type='checkbox']:checked + label:before {
        content: '';
        position: absolute;
        left: 0;
        top: -3px;
        width: 15px;
        height: 15px;
        border: 1px solid var(--dark-grey);
        transition: border 0.2s;
    }
    /* checkbox aspect changes */
    [type='checkbox']:checked + label:before {
        border-color: var(--black);
    }

    /* checked mark aspect */
    [type='checkbox']:not(:checked) + label:after,
    [type='checkbox']:checked + label:after {
        content: '';
        background-image: url('/images/checkmark.svg');
        position: absolute;
        top: -2px;
        left: 1px;
        width: 15px;
        height: 15px;
        font-size: 1.3em;
        line-height: 0.8;
        transition: opacity 0.2s, transform 0.2s;
        background-size: contain;
        background-repeat: no-repeat;
    }
    /* checked mark aspect changes */
    [type='checkbox']:not(:checked) + label:after {
        opacity: 0;
        transform: scale(0);
    }
    [type='checkbox']:checked + label:after {
        opacity: 1;
        transform: scale(1);
    }

    /* accessibility */
    [type='checkbox']:checked:focus + label:before,
    [type='checkbox']:not(:checked):focus + label:before {
        border: 1px solid var(--teal);
    }

    /* disabled checkbox */
    // [type='checkbox']:disabled:not(:checked) + label:before,
    // [type='checkbox']:disabled:checked + label:before {
    //     box-shadow: none;
    //     border-color: #bbb;
    //     background-color: #ddd;
    // }
    // [type='checkbox']:disabled:checked + label:after {
    //     color: #999;
    // }
    // [type='checkbox']:disabled + label {
    //     color: #aaa;
    // }
}
</style>

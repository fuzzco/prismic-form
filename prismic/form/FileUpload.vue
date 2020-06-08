<template>
    <div
        class="prismic-form-file-upload"
        role="group"
        :aria-labelledby="`${kebabCase(label)}-label`"
        :id="`container-${kebabCase(label)}`"
    >
        <div class="inner-label" :id="`${kebabCase(label)}-label`">
            {{ label }}
        </div>

        <div
            v-for="(item, i) in items"
            :key="i"
            :class="['single-input', { dragging }]"
            :id="`wrap-${kebabCase(label)}-${i}`"
        >
            <label :id="`label-${kebabCase(label)}-${i}`" class="single-label">
                {{ item.placeholder }}
            </label>
            <input
                type="file"
                :accept="item.accepted_types"
                :required="item.required"
                :name="`${camelCase(label)}${i}`"
                :id="`input-${camelCase(label)}-${i}`"
                ref="input"
                aria-describedby="`label-${kebabCase(label)}-${i}`"
                @change="inputUpload($event, i)"
                @dragover.prevent="dragging = true"
                @dragleave.prevent="dragging = false"
                @drop.prevent="onDrop($event, i)"
            />

            <div :class="['file-preview', { 'has-file': fileNames[i] }]">
                <span>{{ fileNames[i] || 'upload file +' }}</span>
            </div>
        </div>
    </div>
</template>

<script>
// Note: Div with group role and aria-labelledby is used in place of fieldset
// due to fieldset styling issues in Chromium broswers:
// https://bugs.chromium.org/p/chromium/issues/detail?id=375693

import { camelCase, kebabCase } from 'lodash'
import Vue from 'vue'

export default {
    props: {
        items: { type: Array, default: () => [] },
        label: { type: String, default: '' },
        optionsOverride: { type: Object, default: () => {} }
    },
    data() {
        return {
            dragging: false,
            options: {
                url: '/',
                ...this.optionsOverride
                // TODO: validate file types based on item.accepted_types
            },
            uploadedFiles: this.items.map(item => null)
        }
    },
    computed: {
        fileNames() {
            return this.uploadedFiles.map(file => {
                return file && file.name ? file.name : ''
            })
        }
    },
    methods: {
        camelCase,
        kebabCase,
        inputUpload(evt, index) {
            Vue.set(this.uploadedFiles, index, evt.target.files[0])
        },
        onDrop(e, index) {
            this.dragging = false
            this.$refs.input[index].files = e.dataTransfer.files
            Vue.set(this.uploadedFiles, index, e.dataTransfer.files[0])
        }
    }
}
</script>

<style lang="scss">
.prismic-form-file-upload {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-gap: 20px;

    @include bp(s) {
        grid-template-columns: 100%;
    }

    .inner-label {
        line-height: 1.4;
        padding: 15px 0;

        @include bp(s) {
            padding: 0;
        }
    }

    // Input row
    .single-input {
        border: 1px solid var(--grey);
        padding: 15px;
        color: var(--dark-grey);
        position: relative;
        min-height: 1rem;
        line-height: 1.4;
        display: grid;
        grid-template-columns: 1fr auto;
        grid-gap: 20px;

        @include bp(s) {
            grid-template-columns: 1fr;
            grid-gap: 10px;
            padding: 10px;
            min-height: 22px;
        }

        .is-desktop &:focus-within,
        .is-desktop &.dragging {
            border-color: var(--teal);
        }
    }

    // input covers whole area
    [type='file'] {
        position: absolute;
        cursor: pointer;
        opacity: 0;
        z-index: 5;
        width: 100%;
        height: 100%;
        bottom: 0;
        right: 0;
        left: 0;
        top: 0;
    }

    // Name preview
    .file-preview {
        max-width: 200px;
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
        color: var(--black);

        &.has-file {
            color: var(--teal);
        }
    }
}
</style>

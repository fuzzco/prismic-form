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
            class="single-input"
            :id="`wrap-${kebabCase(label)}-${i}`"
        >
            <label :id="`label-${kebabCase(label)}-${i}`" class="single-label">
                {{ item.placeholder }}
                <input
                    type="file"
                    :accept="item.accepted_types"
                    :required="item.required"
                    :name="`${camelCase(label)}${i}`"
                    :id="`input-${camelCase(label)}-${i}`"
                    ref="input"
                    aria-describedby="`label-${kebabCase(label)}-${i}`"
                    @change="inputUpload($event, i)"
                />
            </label>

            <dropzone
                :id="`dropzone-${camelCase(label)}-${i}`"
                class="dropzone"
                :options="{
                    ...options
                }"
                :include-styling="false"
                @vdropzone-success="dzUpload($event, i)"
            />

            <span class="file-preview">{{
                fileNames[i] || 'upload file +'
            }}</span>
        </div>
    </div>
</template>

<script>
// Note: Div with group role and aria-labelledby is used in place of fieldset
// due to fieldset styling issues in Chromium broswers:
// https://bugs.chromium.org/p/chromium/issues/detail?id=375693

import { camelCase, kebabCase } from 'lodash'
import Dropzone from 'nuxt-dropzone'
import Vue from 'vue'

export default {
    props: {
        items: { type: Array, default: () => [] },
        label: { type: String, default: '' },
        optionsOverride: { type: Object, default: () => {} }
    },
    components: {
        Dropzone
    },
    data() {
        return {
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
        dzUpload(evt, index) {
            Vue.set(this.uploadedFiles, index, evt)
            // this.$refs.input[index].files = evt
        },
        inputUpload(evt, index) {
            Vue.set(this.uploadedFiles, index, evt.target.files[0])
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

    .single-input {
        border: 1px solid var(--grey);
        padding: 15px;
        color: var(--dark-grey);
        position: relative;
        min-height: 1rem;
        line-height: 1.4;

        @include bp(s) {
            padding: 10px;
            min-height: 22px;
        }

        .is-desktop &:focus-within {
            border-color: var(--teal);
        }

        .single-label {
            max-width: 60%;
            display: block;

            @include bp(s) {
                max-width: initial;
                padding-bottom: 30px;
            }
        }
    }

    [type='file'] {
        position: absolute;
        opacity: 0;
        pointer-events: none;
    }

    .dz-clickable {
        background: transparent;
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        cursor: pointer;

        .dz-message {
            display: none;
        }

        &.dz-started .dz-message {
            display: none;
        }

        // &::after {
        //     content: 'upload file +';

        // }
        //
        // &.dz-started::after {
        //     content: '';
        // }
    }

    .dz-preview {
        display: none;

        // padding: 15px;
        // max-width: 40%;
        // margin-left: auto;
        // text-align: right;
        //
        // @include bp(s) {
        //     text-align: left;
        //     max-width: initial;
        //     padding: 10px;
        //     position: absolute;
        //     bottom: 0;
        // }
        //
        // .dz-image,
        // .dz-size,
        // .dz-success-mark,
        // .dz-error-mark {
        //     display: none;
        // }
    }

    .file-preview {
        color: var(--black);
        position: absolute;
        top: 15px;
        right: 15px;
        pointer-events: none;

        @include bp(s) {
            top: initial;
            right: initial;
            bottom: 10px;
            left: 10px;
        }
    }
}
</style>

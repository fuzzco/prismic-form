<template>
    <div class="prismic-form-select">
        <label :for="cmpId" v-if="label">{{ label }}</label>

        <select :name="cmpId" :id="cmpId">
            <option value="" selected disabled v-if="placeholder" key="default">
                {{ placeholder }}
            </option>
            <option v-for="({ option }, i) in items" :key="i">
                {{ option }}
            </option>
        </select>
    </div>
</template>

<script>
import { kebabCase } from 'lodash'
export default {
    props: {
        label: {
            type: String,
            default: ''
        },
        placeholder: {
            type: String,
            default: ''
        },
        items: {
            type: Array,
            default: () => []
        }
    },
    computed: {
        cmpId() {
            return kebabCase(this.label)
        }
    },
    methods: {
        kebabCase
    }
}
</script>

<style lang="scss">
.prismic-form-select {
}
</style>

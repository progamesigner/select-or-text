<template>
    <default-field :field="field" :errors="errors">
        <template slot="field">
            <select 
                v-model="selectValue"
                class="w-full form-control form-select"
                :class="errorClasses"
            >
                <option value="" selected disabled>{{ __('Choose an option') }}</option>

                <option
                    v-for="option in field.options"
                    :value="option.value"
                    :selected="option.value == field.value"
                >
                    {{ option.label }}
                </option>

                <option value="CUSTOM">{{__('Custom (Free Text)')}}</option>
            </select>

            <input v-show="isCustom" type="text"
                class="w-full form-control form-input form-input-bordered mt-6"
                :class="errorClasses"
                :placeholder="field.name"
                v-model="textValue"
            />
        </template>
    </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    data() {
        return {
            selectValue: '',
            textValue: '',
        };
    },

    methods: {
        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            formData.append(this.field.attribute, this.computedValue || '')
        },
    },

    computed: {
        isCustom() {
            return this.selectValue == 'CUSTOM'
        },

        computedValue() {
            return this.isCustom ? this.textValue : this.selectValue
        },
    },
}
</script>

<template>
    <div class="row">
        <div class="col-md-4">
            <label :for="control.name + '_gui_control'">{{control.label}}</label>
        </div>
        <div class="col-md-8">
            <input type="text"
                   class="form-control"
                   :readonly="this.control.readonly"
                   v-if="!control.isMultiLine"
                   :name="control.fieldName"
                   v-model="control.value" />
            <textarea v-else class="form-control"
                      v-model="control.value"
                      :readonly="this.control.readonly"
                      :name="control.fieldName"></textarea>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TextControl",
        props: ['control'],
        mounted() {
            if (!_.isEmpty(this.control.defaultValue)) {
                this.control.value = this.control.defaultValue;
            }
        }
    }
</script>

<style scoped>

</style>

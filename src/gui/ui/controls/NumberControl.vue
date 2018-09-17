<template>
    <div class="row">
        <div class="col-md-4">
            <label>{{control.label}}</label>
        </div>
        <div class="col-md-8">
            <div class="input-group">
                <input type="number"
                       class="form-control"
                       :readonly="this.control.readonly"
                       :name="control.fieldName"
                       :step="controlStep"
                       v-model="control.value" />
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "NumberControl",
        props: ['control'],
        mounted() {
            if (!_.isEmpty(this.control.defaultValue)) {
                this.control.value = this.control.defaultValue;
            }
        },
        watch: {
            "control.value": function (val) {
                if (this.control.isInteger === false) {
                    this.control.value = parseFloat(val).toFixed(this.control.decimalPlace);
                } else {
                    this.control.value = parseInt(val);
                }
            }
        },
        computed: {
            controlStep() {
                return 1;
            }
        }
    }
</script>

<style scoped>

</style>

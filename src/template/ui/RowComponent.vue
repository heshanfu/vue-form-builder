<template>
    <div class="rowWrapper">
        <row-item v-for="row in section.rows" :key="row.name" :row="row" @removeRow="removeRow"></row-item>
    </div>
</template>

<script>
    import RowItem from "./common/RowItem";
    import {Hooks} from 'sethFormBuilder/template/components/hook_lists';

    export default {
        components: {RowItem},
        name: "row-component",
        props: {
            section: {
                type: Object
            }
        },
        methods: {
            traverseRow() {
                let self = this;

                // prepare data
                var items = $(this.$el).find('.rowItem');
                var finalItems = [];

                // sort
                _.each(items, (item, index) => {
                    var id = $(item).attr('id');
                    var rowItem = _.find(self.section.rows, {name: id});
                    finalItems.push(rowItem);
                });

                // reset the current sections
                this.section.rows = finalItems;
            },
            removeRow(rowName) {
                var rowIndex = _.findIndex(this.section.rows, {name: rowName});
                if (this.section.rows[rowIndex].controls.length > 0) {
                    SethPhatToaster.error("Can't remove this row because it's still have controls inside.");
                    return;
                }

                var rowInfo = this.section.rows[rowIndex];
                let beforeRun = Hooks.Row.beforeRemove.runSequence(rowInfo);
                if (beforeRun === false) {
                    return;
                }

                this.section.rows.splice(rowIndex, 1);

                // final hook
                Hooks.Row.afterRemove.run(rowInfo);
            }
        },
        mounted() {
            let self = this;
            $(this.$el).sortable({
                cursor: "move",
                delay: 200,
                update: function (event, ui) {
                    self.traverseRow();
                }
            });
        }
    }
</script>

<style scoped>

</style>

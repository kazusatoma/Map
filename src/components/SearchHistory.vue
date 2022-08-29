<template>
    <div class="searchTable">
        <div style="margin-bottom: 16px">
            <a-button type="danger" :disabled="!hasSelected" @click="deleteItem(state.selectedRowKeys)">
                Delete
            </a-button>
            <span style="margin-left: 8px">
                <template v-if="hasSelected">
                    {{ `Selected ${state.selectedRowKeys.length} items` }}
                </template>
            </span>
        </div>
        <a-table :row-selection="{ selectedRowKeys: state.selectedRowKeys, onChange: onSelectChange }"
            :columns="columns" :data-source="data" :scroll="{ x: 500, y: 400 }" />
    </div>
</template>


<script setup>
import { computed, reactive, toRef, defineProps } from 'vue';
import bus from '@/libs/bus';
const props = defineProps({
    searchedList: Array,
    delList: Function
})
const data = toRef(props, 'searchedList')
const state = reactive({
    selectedRowKeys: [],
});
const columns = [{
    title: 'Location',
    dataIndex: 'Location',
    key: 'key'
},]
const hasSelected = computed(() => state.selectedRowKeys.length > 0);

const deleteItem = (selectedRowKeys) => {
    props.delList(selectedRowKeys)
    state.selectedRowKeys = [];
    bus.emit('deleteItem')
};

const onSelectChange = (selectedRowKeys) => {
    state.selectedRowKeys = selectedRowKeys;
};
</script>


<style scoped>
.searchTable {
    margin-left: 12%;
    margin-right: 12%;
    overflow: auto;
}
</style>
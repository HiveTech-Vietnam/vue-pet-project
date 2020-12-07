<template>
    <div>
        <input @change="querySearchKey" v-model="querySearchInput" placeholder="Search task..." class="search-task" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            querySearchInput: ''
        }
    },
    methods: {
        querySearchKey() {
            this.$emit('querySearchKey', this.querySearchInput)
        }
    }
}
</script>

<style scoped>
.search-task{
    width: 500px;
    height:30px;
    padding-left: 50px;
    outline: none;
}
</style>
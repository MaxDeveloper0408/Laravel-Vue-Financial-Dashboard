<template>
    <div>
        <span class="label label-info" v-if="row.type == 0">
           Buy
        </span>
        <span class="label label-danger" v-if="row.type == 1">
           Sell
        </span>
    </div>
</template>


<script>
export default {
    props: ['row']
}
</script>


<style scoped>

</style>


<template>
    <section class="content-wrapper" style="min-height: 960px;">
        <section class="content-header">
            <h1>Customers</h1>
        </section>

        <section class="content">
            <div class="row">
                <div class="col-xs-12">
                    <div class="box">
                        <div class="box-header with-border">
                            <h3 class="box-title">List</h3>
                        </div>

                        <div class="box-body">
                            <div class="btn-group">
                                <router-link :to="{ name: xprops.route + '.create' }" class="btn btn-success btn-sm">
                                    <i class="fa fa-plus"></i> Add new
                                </router-link>
                                <button type="button" class="btn btn-default btn-sm" @click="fetchData">
                                    <i class="fa fa-refresh" :class="{'fa-spin': loading}"></i> Refresh
                                </button>
                            </div>
                        </div>

                        <div class="box-body">
                            <div class="row" v-if="loading">
                                <div class="col-xs-4 col-xs-offset-4">
                                    <div class="alert text-center">
                                        <i class="fa fa-spin fa-refresh"></i> Loading
                                    </div>
                                </div>
                            </div>

                            <datatable
                                    v-if="!loading"
                                    :columns="columns"
                                    :data="data"
                                    :total="total"
                                    :query="query"
                                    :xprops="xprops"
                                    :support-backup="true"
                                    />
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </section>
</template>


<script>
import { mapGetters, mapActions } from 'vuex'
import components from '../../dtmodules/'

export default {
    components,
    data() {
        return {
            columns: [
                { title: '#', field: 'id', sortable: true, colStyle: 'width: 50px;' },
                { title: 'First name', field: 'first_name', thComp: 'DatatableFilter', sortable: true },
                { title: 'Last name', field: 'last_name', thComp: 'DatatableFilter', sortable: true },
                { title: 'Email', field: 'email', sortable: true },
                { title: 'Customer Code', field: 'customer_code', sortable: true },
                { title: 'Phone', field: 'phone', sortable: true },
                { title: 'Actions', tdComp: 'DatatableActions', visible: true, thClass: 'text-right', tdClass: 'text-right', colStyle: 'width: 130px;' }
            ],
            query: { sort: 'id', order: 'desc', 'first_name': '', 'last_name': '' },
            xprops: {
                module: 'CustomersIndex',
                route: 'customers'
            }
        }
    },
    created() {
        this.fetchData()
    },
    destroyed() {
        this.resetState()
    },
    computed: {
        ...mapGetters('CustomersIndex', ['data', 'data_all', 'total', 'loading', 'relationships']),
    },
    watch: {
        query: {
            handler(query) {
                this.setQuery(query)
                this.handleQueryChange()
            },
            deep: true
        }
    },
    methods: {
        ...mapActions('CustomersIndex', ['fetchData', 'setQuery', 'resetState', 'setAll']),
        handleQueryChange () {       
            let rows = Array()
            this.data_all.forEach(element => {
                if ( element.first_name != null )
                    if( element.first_name.toLowerCase().search(this.query['first_name'].toLowerCase()) >= 0 &&  element.last_name.toLowerCase().search(this.query['last_name'].toLowerCase()) >= 0 )
                    rows.push(element)
            });
            this.setAll(rows)
        },
    }
}
</script>


<style scoped>

</style>

<template>
    <div class="groups">
        <Table :loading="loading" :columns="columns" :data="groups"></Table>
        <Page style="text-align:center; margin-top:10px" :current="1" :total="total" simple @on-change="getGroups"></Page>
    </div>
</template>

<script>
import Util from '@/util'
export default {
    created() {
        this.getGroups(1)
    },
    data() {
        return {
            loading: false,
            groups: [],
            columns: [
                {
                    title: '小组名称',
                    render: (h, params) => {
                        return h('router-link', {
                            props: {
                                to: '/group/'+params.row.gid
                            }
                        }, params.row.name)
                    }
                },
                {
                    title: '组内名称',
                    key: 'group_name'
                },
                {
                    title: '加入时间',
                    render: (h, params) => {
                        return this.getTime(params.row.join_time)
                    }
                }
            ],
            pageSize: 10,
            total: 0
        }
    },
    methods: {
        getGroups(page) {
            this.loading = true
            this.$http.get('/user/joined_groups', {
                params: {
                    page: page,
                    page_size: this.pageSize
                }
            }).then(res => {
                this.total = res.data.total
                this.groups = res.data.data
                this.loading = false
            }).catch(res => {
                this.loading = false
            })
        },
        getTime(time) {
            return Util.getDistanceTime(time)
        }
    }
}
</script>

<style lang="stylus" scoped>

</style>



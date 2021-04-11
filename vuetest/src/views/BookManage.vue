<template>
    <div>
        <el-table
            :data="tableData"
            border
            style="width: 100%">
            <el-table-column
                fixed
                prop="id"
                label="编号"
                align="center"
                width="150">
            </el-table-column>
            <el-table-column
                prop="name"
                label="书名"
                align="center"
                width="300">
            </el-table-column>
            <el-table-column
                prop="author"
                label="作者"
                align="center"
                width="200">
            </el-table-column>
            <el-table-column
                prop="publish"
                label="出版社"
                align="center"
                width="300">
            </el-table-column>
            <el-table-column
                prop="price"
                label="价格"
                align="center"
                width="200">
            </el-table-column>
            <el-table-column
                label="操作"
                align="center"
                width="200">
                <template slot-scope="scope">
                    <el-button @click="edit(scope.row)" type="text" size="small">修改</el-button>
                    <el-button @click="dele(scope.row)" type="text" size="small">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <div style="margin-top: 30px ">
            <el-pagination
                align="center"
                background
                layout="total, prev, pager, next, jumper"
                :page-size="10"
                :total="total"
                @current-change="page">
            </el-pagination>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        dele(row) {
            const _this = this;
            this.$confirm('此操作将永久删除该图书记录, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                axios.delete('http://localhost:8181/book/deleteById/' + row.id).then((resp) => {
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                    window.location.reload();
                })
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除'
                });
            });
        },
        edit(row) {
            this.$router.push({
                path: '/update',
                query: {
                    id: row.id
                }
            })
        },
        page(currentPage) {
            const _this = this;
            axios.get('http://localhost:8181/book/findAll/' + currentPage + '/10').then((response) => {
                _this.tableData = response.data.content;
                _this.total = response.data.totalElements;
            })
        }
    },
    created() {
        const _this = this;
        axios.get('http://localhost:8181/book/findAll/1/10').then((response) => {
            _this.tableData = response.data.content;
            _this.total = response.data.totalElements;
        })
    },
    data() {
        return {
            total: null,
            tableData: null
        }
    }
}
</script>
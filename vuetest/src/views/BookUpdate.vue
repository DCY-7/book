<template>
    <el-form style="width: 60%" :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px"
             class="demo-ruleForm">

        <el-form-item label="图书编号">
            <el-input v-model="ruleForm.id" readOnly></el-input>
        </el-form-item>

        <el-form-item label="图书名称" prop="name">
            <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>

        <el-form-item label="作者" prop="author">
            <el-input v-model="ruleForm.author"></el-input>
        </el-form-item>

        <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">修改</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>

    </el-form>
</template>

<script>
export default {
    data() {
        return {
            ruleForm: {
                id: '',
                name: '',
                author: ''
            },
            rules: {
                name: [
                    {required: true, message: '图书名称不能为空', trigger: 'blur'}
                ],
                author: [
                    {required: true, message: '作者不能为空', trigger: 'blur'}
                ]
            }
        };
    },
    methods: {
        submitForm(formName) {
            const _this = this
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    this.$confirm('此操作将修改图书信息, 是否继续?', '提示', {
                        confirmButtonText: '确定',
                        cancelButtonText: '取消',
                        type: 'warning'
                    }).then(() => {
                        axios.put('http://localhost:8181/book/update', this.ruleForm).then((resp) => {
                            if (resp.data == 'success') {
                                this.$message({
                                    type: 'success',
                                    message: '修改成功!'
                                });
                                _this.$router.push('/BookManage')
                            }
                        })
                    }).catch(() => {
                        this.$message({
                            type: 'info',
                            message: '已取消修改'
                        });
                    });
                } else {
                    console.log('error submit!!');
                    return false;
                }
            });
        },
        resetForm(formName) {
            this.$refs[formName].resetFields();
        }
    },
    created() {
        const _this = this
        axios.get('http://localhost:8181/book/findById/' + this.$route.query.id).then(function (resp) {
            _this.ruleForm = resp.data
        })
    }
}
</script>
# 解决 Vue 中的Element-ui 组件el-checkbox动态赋值点击没反应问题

    <el-checkbox :value="form.reaudit === '1'" @change="val => $set(form,'reaudit',val ? '1' : '0')">是否需要复核</el-checkbox>

    data () {
        return {
          form: {
            reaudit: ''
          }
        }
    }





<template lang="html">

<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">

  <el-form-item label="商户名称" prop="memberName">
    <el-input v-model="ruleForm.memberName" style="width:215px"></el-input>
  </el-form-item>

  <el-form-item label="分类" prop="sqlType">
    <el-select v-model="ruleForm.sqlType" placeholder="请选择分类">
      <el-option label="区域一" value="shanghai"></el-option>
      <el-option label="区域二" value="beijing"></el-option>
    </el-select>
  </el-form-item>

  <el-form-item label="详情" prop="sqlDetail">
    <el-input v-model="ruleForm.sqlDetail" style="width:215px"></el-input>
  </el-form-item>


  <el-form-item label="字段参数" prop="sqlParams">
	  <el-tag :key="tag" v-for="tag in  ruleForm.sqlParams " :closable="true" :close-transition="false" @close="handleClose(tag)" style="margin-right:10px;">
		{{tag}}
	  </el-tag>
	<el-input class="" v-if="inputVisible" v-model="inputValue" ref="saveTagInput" size="small" style="width:90px"
	   @keyup.enter.native="handleInputConfirm" @blur="handleInputConfirm"
	>
	</el-input>
	<el-button v-else class="button-new-tag" size="small" @click="showInput">添加一个新的参数</el-button>
  </el-form-item>

  <el-form-item label="sql语句" prop="sqlContent">
    <el-input type="textarea" v-model="ruleForm.sqlContent" :rows="rows"></el-input>
  </el-form-item>

  <el-form-item label="备注信息" prop="desc">
    <el-input type="textarea" v-model="ruleForm.desc"></el-input>
  </el-form-item>

  <el-form-item label="提交人" prop="commitUser">
    <el-select v-model="ruleForm.commitUser" placeholder="请选择提交人">
      <el-option label="涂泽" value="tuze"></el-option>
      <el-option label="付刚" value="fugang"></el-option>
    </el-select>
  </el-form-item>

  <el-form-item>
    <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
    <el-button @click="resetForm('ruleForm')">重置</el-button>
  </el-form-item>

</el-form>
</template>


<script>
export default {
	data() {
		return {
			ruleForm: {
				memberName: '', //商户名称
				sqlType: '',//sql分类
				sqlDetail: '',//sql详情
				sqlContent:'',//sql语句
				desc: '',//备注信息
				commitUser:'',//提交人的信息
				sqlParams:[],//sql参数列表
			},
			inputVisible:false,//控制新添加的sql框的显隐
			inputValue:'',//新添加sql框的值为空
			rows:11,
			rules: {
				memberName: [{
						required: true,
						message: '商户名称不能为空',
						trigger: 'blur'
					}
				],
				sqlType: [{
					required: true,
					message: '请选择一个分类',
					trigger: 'change'
				}],
				sqlDetail: [{
					required: true,
					message: '请输入详情',
					trigger: 'blur'
				}],
				sqlContent: [{
					required: true,
					message: '请填写SQL语句',
					trigger: 'blur'
				}],
				commitUser: [{
					required: true,
					message: '请选择提交人',
					trigger: 'blur'
				}],
				sqlParams:[
					{
						required: false,
					}
				]
			}
		};
	},
	methods: {
		submitForm(formName) {
			this.$refs[formName].validate((valid) => {
				if (valid) {
					console.log(this.ruleForm);
					alert('submit!');
				} else {
					console.log('error submit!!');
					return false;
				}
			});
		},
		resetForm(formName) {
			this.$refs[formName].resetFields();
		},
		//点击按钮显示输入框
		showInput(){
			this.inputVisible=true;
			this.$nextTick(_=>{
				this.$refs.saveTagInput.$refs.input.focus();
			})
		},
		//关闭按钮
		handleClose(tag){
			this.ruleForm.sqlParams.splice(this.ruleForm.sqlParams.indexOf(tag),1)
		},
		//输入新的标签
		handleInputConfirm(){
			let inputValue=this.inputValue;
			if(inputValue){
				this.ruleForm.sqlParams.push(inputValue)
			}
			this.inputValue='';
			this.inputVisible=false;
		}

	}
}
</script>


<style lang="css">
</style>

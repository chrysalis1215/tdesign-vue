<template>
  <div>
    <!-- error-message 非必需 -->
    <t-form
      :data="formData"
      :rules="rules"
      :validate-message="validateMessage"
      ref="form"
      @reset="onReset"
      @submit="onSubmit"
      scrollToFirstError="smooth"
    >
      <t-form-item label="用户名" help="这是用户名字段帮助说明" name="account">
        <t-input v-model="formData.account"></t-input>
      </t-form-item>
      <t-form-item label="个人简介" help="一句话介绍自己" name="description">
        <t-input v-model="formData.description"></t-input>
      </t-form-item>
      <t-form-item label="密码" name="password">
        <t-input type="password" v-model="formData.password"></t-input>
      </t-form-item>
      <t-form-item label="邮箱" name="email">
        <t-input v-model="formData.email"></t-input>
      </t-form-item>
      <t-form-item label="性别" name="gender">
        <t-radio-group v-model="formData.gender">
          <t-radio value="male">男</t-radio>
          <t-radio value="femal">女</t-radio>
        </t-radio-group>
      </t-form-item>
      <t-form-item label="课程" name="course">
        <t-checkbox-group v-model="formData.course" :options="courseOptions"></t-checkbox-group>
      </t-form-item>
      <t-form-item label="学院" name="college">
        <t-select v-model="formData.college" class="demo-select-base" clearable>
          <t-option v-for="(item, index) in options" :value="item.value" :label="item.label" :key="index">
            {{ item.label }}
          </t-option>
        </t-select>
      </t-form-item>
      <t-form-item
        label="入学时间"
        name="date"
        :rules="[{ date: { delimiters: ['/', '-', '.'] }, message: '日期格式有误' }]"
      >
        <t-input v-model="formData.date"></t-input>
      </t-form-item>
      <t-form-item label="个人网站" name="content.url">
        <t-input v-model="formData.content.url"></t-input>
      </t-form-item>
      <t-form-item style="padding-top: 8px">
        <t-button theme="primary" type="submit" style="margin-right: 10px">提交</t-button>
        <t-button theme="default" variant="base" type="reset" style="margin-right: 10px">重置</t-button>
        <t-button theme="default" variant="base" @click="handleValidateMessage">设置校验信息提示</t-button>
      </t-form-item>
    </t-form>
  </div>
</template>
<script>
/* eslint-disable no-template-curly-in-string */
const INITIAL_DATA = {
  account: '',
  password: '',
  description: '',
  email: '',
  gender: '',
  college: '',
  date: '',
  content: {
    url: '',
  },
  course: [],
};
const validateMessage = {
  account: [
    {
      type: 'error',
      message: '自定义用户名校验信息提示',
    },
  ],
  description: [
    {
      type: 'error',
      message: '自定义个人简介校验信息提示',
    },
  ],
  password: [
    {
      type: 'warning',
      message: '自定义密码校验信息提示',
    },
  ],
};

export default {
  data() {
    return {
      formData: { ...INITIAL_DATA },
      validateMessage: {},
      courseOptions: [
        { label: '语文', value: '1' },
        { label: '数学', value: '2' },
        { label: '英语', value: '3' },
        { label: '体育', value: '4' },
      ],
      options: [
        { label: '计算机学院', value: '1' },
        { label: '软件学院', value: '2' },
        { label: '物联网学院', value: '3' },
      ],
      rules: {
        account: [
          { required: true },
          // { enum: ['sheep', 'name'] },
          { min: 2 },
          { max: 10, type: 'warning' },
        ],
        description: [
          { validator: (val) => val.length >= 5 },
          { validator: (val) => val.length < 10, message: '不能超过 20 个字，中文长度等于英文长度' },
        ],
        password: [
          { required: true },
          { len: 8, message: '请输入 8 位密码' },
          { pattern: /[A-Z]+/, message: '密码必须包含大写字母' },
        ],
        email: [{ required: true }, { email: { ignore_max_length: true } }],
        gender: [{ required: true }],
        course: [
          { required: true },
          { validator: (val) => val.length <= 2, message: '最多选择 2 门课程', type: 'warning' },
        ],
        'content.url': [
          { required: true },
          {
            url: {
              protocols: ['http', 'https', 'ftp'],
              require_protocol: true,
            },
          },
        ],
      },
    };
  },
  mounted() {
    this.validateMessage = validateMessage;
  },
  methods: {
    onReset() {
      this.$message.success('重置成功');
      this.validateMessage = {};
    },
    onSubmit({ validateResult, firstError }) {
      if (validateResult === true) {
        this.$message.success('提交成功');
      } else {
        console.log('Errors: ', validateResult);
        this.$message.warning(firstError);
      }
    },
    handleValidateMessage() {
      this.$message.success('设置表单校验信息提示成功');
      this.validateMessage = validateMessage;
    },
  },
};
</script>

<style scoped>
.demo-select-base {
  width: 300px;
}
</style>

<template>
  <div id="wrapper">
    <Tabs value="name1" size="small">
      <TabPane label="网络设置" name="netset" icon="ios-planet-outline">
        <Form ref="netset" :model="netset" :rules="ruleCustom" :label-width="100" style="width:300px;margin:14px auto 0 auto;">
          <FormItem label="服务器IP" prop="ip">
            <Input type="text" v-model="netset.ip"></Input>
          </FormItem>
          <FormItem label="服务器端口" prop="port">
            <InputNumber v-model="netset.port"></InputNumber>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" @click="handleSubmit('netset')" style="width:90px;">读取</Button>
            <Button shape="circle" type="info" @click="handleReset('netset')" style="margin-left:8px;width:90px;">设置</Button>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="第一页屏" name="one" icon="ios-browsers-outline">
        <Form :model="formItem" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem.select" style="width:190px;">
              <Option value="beijing">立即显示</Option>
              <Option value="shanghai">连续左移</Option>
              <Option value="shenzhen">左移（带停留）</Option>
              <Option value="beijing">连续下移</Option>
              <Option value="shanghai">下移（带停留）</Option>
              <Option value="shenzhen">闪烁</Option>
              <Option value="shenzhen">连续上移</Option>
              <Option value="shenzhen">上移（带停留）</Option>
              <Option value="shenzhen">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;">设置</Button>
            <Button shape="circle" type="waring" style="margin-left:12px;width:90px;">预览</Button>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="第二页屏" name="two" icon="ios-browsers-outline">
        <Form :model="formItem" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem.select" style="width:190px;">
              <Option value="beijing">立即显示</Option>
              <Option value="shanghai">连续左移</Option>
              <Option value="shenzhen">左移（带停留）</Option>
              <Option value="beijing">连续下移</Option>
              <Option value="shanghai">下移（带停留）</Option>
              <Option value="shenzhen">闪烁</Option>
              <Option value="shenzhen">连续上移</Option>
              <Option value="shenzhen">上移（带停留）</Option>
              <Option value="shenzhen">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;">设置</Button>
            <Button shape="circle" type="waring" style="margin-left:12px;width:90px;">预览</Button>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="第三页屏" name="three" icon="ios-browsers-outline">
        <Form :model="formItem" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem.select" style="width:190px;">
              <Option value="beijing">立即显示</Option>
              <Option value="shanghai">连续左移</Option>
              <Option value="shenzhen">左移（带停留）</Option>
              <Option value="beijing">连续下移</Option>
              <Option value="shanghai">下移（带停留）</Option>
              <Option value="shenzhen">闪烁</Option>
              <Option value="shenzhen">连续上移</Option>
              <Option value="shenzhen">上移（带停留）</Option>
              <Option value="shenzhen">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;">设置</Button>
            <Button shape="circle" type="waring" style="margin-left:12px;width:90px;">预览</Button>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="第四页屏" name="four" icon="ios-browsers-outline">
        <Form :model="formItem" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem.select" style="width:190px;">
              <Option value="beijing">立即显示</Option>
              <Option value="shanghai">连续左移</Option>
              <Option value="shenzhen">左移（带停留）</Option>
              <Option value="beijing">连续下移</Option>
              <Option value="shanghai">下移（带停留）</Option>
              <Option value="shenzhen">闪烁</Option>
              <Option value="shenzhen">连续上移</Option>
              <Option value="shenzhen">上移（带停留）</Option>
              <Option value="shenzhen">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;">设置</Button>
            <Button shape="circle" type="waring" style="margin-left:12px;width:90px;">预览</Button>
          </FormItem>
        </Form>
      </TabPane>
    </Tabs>
    <footer @click="open('http://www.jsxywg.cn/')">
      &copy;2018 江苏星云网格信息技术有限公司提供全程技术支持
    </footer>
  </div>
</template>
<script>
  export default {
    name: 'landing-page',
    components: {
      
    },
    data() {
      return {
        netset: {
          ip: '',
          port: 0
        },
        ruleCustom: {
          port: [{
            required: true,
            message: '请输入端口号',
            trigger: 'blur',
            type: 'number'
          }],
          ip: [{
            required: true,
            message: '请输入IP',
            trigger: 'blur'
          }]
        },
        formItem: {}
      }
    },
    methods: {
      open(link) {
        this.$electron.shell.openExternal(link)
      },
      handleSubmit(name) {
        this.$refs[name].validate((valid) => {
          if (valid) {
            this.$Message.success('Success!');
          } else {
            this.$Message.error('Fail!');
          }
        })
      },
      handleReset(name) {
        this.$refs[name].resetFields();
      }
    }
  }
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: 'Microsoft Yahei', sans-serif;
  }

  #wrapper {
    height: 100vh;
    padding: 40px 20px;
    width: 100vw;
  }

  footer {
    position: fixed;
    bottom: 0;
    left: 0;
    background: #f8f8f9;
    color: rgb(150, 150, 150);
    text-align: center;
    width: 100%;
    height: 35px;
    cursor: pointer;
    line-height: 3em;
  }

  .ivu-modal-mask {
    background: radial-gradient(#f7f7f7, rgba(255, 255, 255, 0.95));
  }

  .ivu-modal-content {
    box-shadow: 0 3px 25px rgba(2, 2, 2, 0.151);
    width:350px;
    margin: 0 auto;
  }

</style>
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
          <FormItem label="端口号" prop="port">
            <Select v-model="commodel" style="width:200px">
              <Option v-for="item in comlist" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" @click="handleSubmit('netset')" style="width:90px;">读取</Button>
            <Button shape="circle" type="info" @click="handleReset('netset')" style="margin-left:8px;width:90px;">设置</Button>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="第一页屏" name="one" icon="ios-browsers-outline">
        <Form :model="formItem1" :label-width="80" style="width:500px;height:600px;margin:20px auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem1.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem1.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem1.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem1.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem1.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem1.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem1.select" style="width:190px;">
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
            <Button shape="circle" type="primary" style="width:90px;" @click="test">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;" @click="serialport">设置</Button>
            <Button shape="circle" type="waring" style="margin-left:12px;width:90px;" @click="preview">预览</Button>
          </FormItem>
          <div id="preview"></div>
          <div id="mycanvas"></div>
        </Form>

      </TabPane>
      <TabPane label="第二页屏" name="two" icon="ios-browsers-outline">
        <Form :model="formItem2" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem2.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem2.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem2.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem2.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem2.select" style="width:190px;">
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
        <Form :model="formItem3" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem3.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem3.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem3.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem3.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem3.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem3.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem3.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem3.select" style="width:190px;">
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
        <Form :model="formItem4" :label-width="80" style="width:400px;height:600px;margin:14px auto 0 auto;">
          <FormItem label="是否启用">
            <i-switch v-model="formItem4.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="formItem4.textarea" placeholder="请输入第一行内容"></Input>
            <RadioGroup v-model="formItem4.radio1">
              <Radio label="left1">居左</Radio>
              <Radio label="middle1">居中</Radio>
              <Radio label="right1">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="formItem4.textarea" placeholder="请输入第二行内容"></Input>
            <RadioGroup v-model="formItem4.radio2">
              <Radio label="left2">居左</Radio>
              <Radio label="middle2">居中</Radio>
              <Radio label="right2">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="formItem.textarea" placeholder="请输入第三行内容"></Input>
            <RadioGroup v-model="formItem4.radio3">
              <Radio label="left3">居左</Radio>
              <Radio label="middle3">居中</Radio>
              <Radio label="right3">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="formItem4.select" style="width:190px;">
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
    <footer>
      &copy;2018 江苏星云网格信息技术有限公司提供全程技术支持
    </footer>
  </div>
</template>
<script>
  import {
    CRC
  } from 'crc-full'
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
        formItem: {},
        formItem1: {},
        formItem2: {},
        formItem3: {},
        formItem4: {},
        leddata: [],
        comlist: [],
        commodel: ''
      }
    },
    created() {
      /*./node_modules/.bin/electron-rebuild -v 2.0.12*/
      var SerialPort = require('serialport')
      let portInner = ""
      var that = this
      SerialPort.list(function (err, ports) {
        ports.forEach(function (port) {
          let com = port.comName
          var cominfo = {
            value: com,
            label: com
          }
          that.comlist.push(cominfo)
        })
      })
      console.log(that.comlist)
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
      },
      preview() {
        var grid = require('pixel-grid')
        this.leddata = []
        for (var i = 0; i <= 47; i++) {
          var row = []
          for (var j = 0; j <= 127; j++) {
            row.push(1)
          }
          this.leddata.push(row)
        }
        console.log('data = ', this.leddata)
        var pixels = grid(this.leddata, {
          root: document.getElementById('preview'),
          size: 2
        })
      },
      test() {
        var getPixels = require("get-pixels")
        var that = this
        getPixels("resource/heart.jpg", function (err, pixels) {
          var pix = {}
          var pp = {}

          if (err) {
            console.log("Bad image path")
            return
          }
          pix = pixels
          pp = pixels
          that.stepLife(pp, pix)

        })

        //console.log('pp',pp)
        //console.log('pix',pix)
      },
      serialport() {
        var iconv = require('iconv-lite')
        var sss = 'XYCONFIG*;*5'
        let crc = new CRC("CRC16_MODBUS", 16, 0x8005, 0xFFFF, 0x0000, true, true);
        let computed_crc = crc.compute(iconv.encode(sss, 'ascii'))

        var SerialPort = require('serialport')
        var port = new SerialPort('COM8', {
          autoOpen: false,
          baudRate: 115200,
          dataBits: 8
        });
        port.open(function (err) {
          if (err) {
            return console.log('Error opening port: ', err.message);
          }
          // Because there's no callback to write, write errors will be emitted on the port:
          //port.write('main screen turn on');
        });
        port.write(iconv.encode(sss + '*;*' + computed_crc.toString(16), 'gbk'), function (err) {
          if (err) {
            return console.log('Error on write: ', err.message);
          }
          console.log('message written');
        });
        // The open event is always emitted
        port.on('open', function () {
          console.log('成功打开')
        });
        port.on('data', function (data) {
          console.log('Return Data:', iconv.decode(data, 'gbk'));
        });

      },
      stepLife(next_state, cur_state) {
        this.leddata = []
        var arr = require("ndarray-unpack")(cur_state)
        for (var i = 0; i <= 47; i++) {
          var row = []
          for (var j = 0; j <= 127; j++) {
            if (arr[i, j][0][2] < 255){
              console.log('1')
              row.push(1)
            }else{
              console.log(arr[i, j])
              row.push(0)
            }
          }
          this.leddata.push(row)
        }
        var grid = require('pixel-grid')

        var pixels = grid(this.leddata, {
          root: document.getElementById('preview'),
          size: 2
        })

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
    background: #f8f8f9;
  }

  .ivu-modal-content {
    box-shadow: 0 3px 25px rgba(133, 133, 133, 0.151);
    width: 350px;
    margin: 0 auto;
  }

  #preview {
    margin: 0 auto;
  }
</style>
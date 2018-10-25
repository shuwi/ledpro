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
          <div id="color"></div>
        </Form>
      </TabPane>
      <TabPane label="第一页屏" name="one" icon="ios-browsers-outline">
        <Form :model="pageone" :rules="pageoneRule" :label-width="160" style="width:680px;height:680px;margin:10px auto;">
          <FormItem label="是否启用">
            <i-switch v-model="pageone.switch" size="large">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行">
            <Input v-model="pageone.lineone" placeholder="请输入第一行内容" style="width:200px;" :maxlength="8"></Input>
            <RadioGroup v-model="pageone.radioone">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行">
            <Input v-model="pageone.linetwo" placeholder="请输入第二行内容" style="width:200px;" :maxlength="8"></Input>
            <RadioGroup v-model="pageone.radiotwo">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行">
            <Input v-model="pageone.linethree" placeholder="请输入第三行内容" style="width:200px;" :maxlength="8"></Input>
            <RadioGroup v-model="pageone.radiothree">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="pageone.select" style="width:200px;">
              <Option value="ljxs">立即显示</Option>
              <Option value="lxzy">连续左移</Option>
              <Option value="zy">左移（带停留）</Option>
              <Option value="lxxy">连续下移</Option>
              <Option value="xy">下移（带停留）</Option>
              <Option value="ss">闪烁</Option>
              <Option value="lxsy">连续上移</Option>
              <Option value="sy">上移（带停留）</Option>
              <Option value="px">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;" @click="test">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;" @click="serialport">设置</Button>
            <Button shape="circle" type="warning" style="margin-left:12px;width:90px;" @click="preview">预览</Button>
          </FormItem>
          <div id="preview" ref="pageonepre"></div>
        </Form>
      </TabPane>
      <TabPane label="第二页屏" name="two" icon="ios-browsers-outline">
        2
      </TabPane>
      <TabPane label="第三页屏" name="three" icon="ios-browsers-outline">
        3
      </TabPane>
      <TabPane label="第四页屏" name="four" icon="ios-browsers-outline">
        4
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
  const {
    createCanvas,
    loadImage
  } = require('canvas')
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
        pageone: {
          switch: '',
          select: 'ljxs',
          lineone: '',
          linetwo: '',
          linethree: '',
          radioone: 'left',
          radiotwo: 'left',
          radiothree: 'left'
        },
        pageoneRule: {

        },
        leddata: [],
        comlist: [],
        commodel: '',
        imgsrc: ''
      }
    },
    mounted() {
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
        const {
          createCanvas,
          loadImage
        } = require('canvas')
        const canvas = createCanvas(128, 48)
        const ctx = canvas.getContext('2d')
        var that = this

        that.leddata = []
        ctx.font = '16px 宋体'

        if (that.pageone.radioone == 'left') {
          ctx.fillText(that.pageone.lineone, 0, 14)
        }
        if (that.pageone.radioone == 'middle') {
          ctx.textAlign = "center"
          ctx.fillText(that.pageone.lineone, 64, 14)
        }
        if (that.pageone.radioone == 'right') {
          ctx.textAlign = "right"
          ctx.fillText(that.pageone.lineone, 128, 14)
        }

        if (that.pageone.radiotwo == 'left') {
          ctx.textAlign = "left"
          ctx.fillText(that.pageone.linetwo, 0, 30)
        }
        if (that.pageone.radiotwo == 'middle') {
          ctx.textAlign = "center"
          ctx.fillText(that.pageone.linetwo, 64, 30)
        }
        if (that.pageone.radiotwo == 'right') {
          ctx.textAlign = "right"
          ctx.fillText(that.pageone.linetwo, 128, 30)
        }

        if (that.pageone.radiothree == 'left') {
          ctx.textAlign = "left"
          ctx.fillText(that.pageone.linethree, 0, 46)
        }
        if (that.pageone.radiothree == 'middle') {
          ctx.textAlign = "center"
          ctx.fillText(that.pageone.linethree, 64, 46)
        }
        if (that.pageone.radiothree == 'right') {
          ctx.textAlign = "right"
          ctx.fillText(that.pageone.linethree, 128, 46)
        }

        var base64Img = require('base64-img')
        loadImage(require('path').join('../../../bg.png')).then((image) => {
          ctx.drawImage(image, 0, 0, 0, 48)
          that.imgsrc = canvas.toDataURL()
          base64Img.img(that.imgsrc, 'resources/', 'pageone', function (err, filepath) {
            var zeros = require("zeros")
            var fill = require("ndarray-fill")
            var x = zeros([48, 128])

            var getPixels = require("get-pixels")
            var ndarray = require("ndarray")
            getPixels(filepath, function (err, pixels) {
              if (err) {
                console.log("Bad image path")
                return
              }
              var temp = require("ndarray-unpack")(pixels)

              fill(x, function (i, j) {
                return (temp[j][i])[3] > 0 ? 1 : 0
              })
              const math = require('mathjs')

              var Matrix = require('matrix-slicer')
              var mm = new Matrix(require("ndarray-unpack")(x))

              var basetemp = []
              //间隔时间链式列循环生成像素矩阵
              var iii = 0
              setInterval(function () {
                for (var ii = 0; ii < 128; ii++) {
                  if (ii < 127 && ii > 0) {
                    basetemp[ii] = mm.getColumn(ii + 1)
                  } else {
                    if (ii == 0)
                      basetemp[ii] = mm.getColumn(1)
                    if (ii == 127)
                      basetemp[ii] = mm.getColumn(0)
                  }

                }
                iii++
                if (iii < 128)
                  mm = new Matrix(math.transpose(basetemp))
                else {
                  iii = 0
                  mm = new Matrix(require("ndarray-unpack")(x))
                }
                that.leddata = math.transpose(basetemp)

                for (var i = 0; i < 128; i++) {
                  for (var j = 0; j < 48; j++) {
                    if (that.leddata[j][i] == 1) {
                      that.leddata[j][i] = '#ff0000'
                    } else {
                      that.leddata[j][i] = '#333333'
                    }
                  }
                }
                try {
                  that.$refs.pageonepre.innerHTML = ""
                  var grid = require('pixel-grid')
                  var pixels = grid(that.leddata, {
                    root: document.getElementById('preview'),
                    size: 3,
                    formatted: false
                  })
                } catch (ex) {

                }
              }, 0)
            })
          })
        })

      },
      test() {
        var getPixels = require("get-pixels")
        var that = this
        getPixels("resource/tt.jpg", function (err, pixels) {
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
        console.log('size = ', cur_state.size)

        var arr = require("ndarray-unpack")(cur_state)
        var grid = require('pixel-grid')
        console.log('arr = ', arr)
        var pixels = grid(arr, {
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
    padding-left: 20px;
  }

  vanvas {
    color: red;
  }
</style>
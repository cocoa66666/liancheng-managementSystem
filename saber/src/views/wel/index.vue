<template>
  <basic-container>
    <div>
      <div class="content">
        <div class="chart1">
          <div class="chart1_title">今日订单量</div>
          <div class="chart1_title2">今日0:00-24:00创建的订单量（单）</div>
          <div class="chart1_title3">177</div>
          <div id="chart1"></div>
          <div class="chart1_bottom">其中有5.82%异常订单</div>
        </div>
        <div class="chart1">
          <div class="chart1_title">今日充电量</div>
          <div class="chart1_title2">今日0:00-24:00充电总量（度）</div>
          <div class="chart1_title3">5060.97</div>
          <div id="chart2"></div>
        </div>
        <div class="chart1">
          <div class="chart1_title">今日营收</div>
          <div class="chart1_title2">订单金额</div>
          <div class="chart1_title3">5414.53</div>
          <div class="chart1_title4">
            电费：<span style="color: #000000">3519.46</span>
          </div>
          <div class="chart1_title5">
            服务费：<span style="color: #000000">1950.06</span>
          </div>
          <div id="chart3"></div>
        </div>
        <div class="chart1">
          <div class="chart1_title">充电枪状态</div>
          <div class="chart1_title2">今日0:00-24:00充电枪状态</div>
          <div class="chart1_title3">62</div>
          <div id="chart4"></div>
        </div>
      </div>
      <div class="content2">
        <div class="chart">
          <div class="chart_zhexian">充电数据</div>
          <div id="chart5"></div>
        </div>
        <div class="chart">
          <div class="chart_zhexian">创建的订单量</div>
          <div id="chart6"></div>
        </div>
      </div>
    </div>
  </basic-container>
</template>

<script>
import { mapGetters } from "vuex";
let echarts = require("echarts");
export default {
  name: "wel",
  data() {
    return {
      chart1: [
        { value: 58, name: "充电中" },
        { value: 33, name: "待支付" },
        { value: 86, name: "已支付" },
      ],
    };
  },
  computed: {
    ...mapGetters(["userInfo"]),
  },
  mounted() {
    this.chart_a();
    this.chart_b();
    this.chart_c();
    this.chart_d();
    this.chart_e();
    this.chart_f();
  },
  methods: {
    handleChange(val) {
      window.console.log(val);
    },
    chart_a() {
      var myChart1 = echarts.init(document.getElementById("chart1"));
      myChart1.setOption({
        color: ["#FF6C75", "#F8C478", "#578DD6"],
        tooltip: {
          trigger: "item",
          borderWidth: "0",
          formatter: function (params) {
            var result = "";
            result =
              "<div class='a" +
              params.dataIndex +
              "'></div>" +
              params.data.name +
              ": " +
              params.data.value +
              " (" +
              params.percent +
              "%)";
            return result;
          },
          position: function (pos, params, dom, rect, size) {
            // 鼠标在左侧时 tooltip 显示到右侧，鼠标在右侧时 tooltip 显示到左侧。
            var obj = { top: 60 };
            obj[["left", "right"][+(pos[0] < size.viewSize[0] / 2)]] = 5;
            return obj;
          },
          backgroundColor: "#585858",
          textStyle: {
            fontSize: 5,
            color: "#fff",
          },
        },
        legend: {
          orient: "horizontal",
          top: 200,
          data: [
            {
              name: "充电中",
            },
            { name: "待支付" },
            { name: "已支付" },
          ],
        },
        graphic: [
          {
            //环形图中间添加文字
            type: "text", //通过不同top值可以设置上下显示
            left: "center",
            top: "30%",
            style: {
              text: "今日订单量",
              textAlign: "center",
              fill: "#999999", //文字的颜色
              width: 30,
              height: 30,
              fontSize: 12,
              fontFamily: "Microsoft YaHei",
            },
          },
          {
            type: "text",
            left: "center",
            top: "37%",
            style: {
              text: "177",
              textAlign: "center",
              fill: "#333333",
              width: 30,
              height: 30,
              fontSize: 12,
            },
          },
        ],
        series: [
          {
            name: "访问来源",
            type: "pie",
            height: 200,
            radius: ["55%", "70%"],
            avoidLabelOverlap: false,
            hoverAnimation: false, //移入放大
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 58, name: "充电中" },
              { value: 33, name: "待支付" },
              { value: 86, name: "已支付" },
            ],
          },
        ],
      });
    },
    chart_b() {
      var myChart2 = echarts.init(document.getElementById("chart2"));
      myChart2.setOption({
        color: ["#FF6C75", "#F8C478", "#578DD6", "#9891CD", "#87DAC7"],
        tooltip: {
          trigger: "item",
          borderWidth: "0",
          formatter: function (params) {
            var result = "";
            result =
              "<div></div>" +
              params.data.name +
              ": " +
              params.data.value +
              "度 (" +
              params.percent +
              "%)";
            return result;
          },
          position: function (pos, params, dom, rect, size) {
            // 鼠标在左侧时 tooltip 显示到右侧，鼠标在右侧时 tooltip 显示到左侧。
            var obj = { top: 60 };
            obj[["left", "right"][+(pos[0] < size.viewSize[0] / 2)]] = 5;
            return obj;
          },
          backgroundColor: "#585858",
          textStyle: {
            fontSize: 5,
            color: "#fff",
          },
        },
        legend: {
          orient: "horizontal",
          top: 200,
          data: [
            { name: "00:00-07:00" },
            { name: "07:00-09:00" },
            { name: "09:00-15:00" },
            { name: "15:00-20:00" },
            { name: "20:00-22:00" },
            { name: "22:00-23:00" },
            { name: "23:00-24:00" },
          ],
        },
        graphic: [
          {
            //环形图中间添加文字
            type: "text", //通过不同top值可以设置上下显示
            left: "center",
            top: "30%",
            style: {
              text: "今日充电量",
              textAlign: "center",
              fill: "#999999", //文字的颜色
              width: 30,
              height: 30,
              fontSize: 12,
              fontFamily: "Microsoft YaHei",
            },
          },
          {
            type: "text",
            left: "center",
            top: "37%",
            style: {
              text: "5060.97",
              textAlign: "center",
              fill: "#333333",
              width: 30,
              height: 30,
              fontSize: 12,
            },
          },
        ],
        series: [
          {
            name: "访问来源",
            type: "pie",
            height: 200,
            radius: ["55%", "70%"],
            avoidLabelOverlap: false,
            hoverAnimation: false, //移入放大
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 1482.46, name: "00:00-07:00" },
              { value: 169.05, name: "07:00-09:00" },
              { value: 1856.61, name: "09:00-15:00" },
              { value: 1552.85, name: "15:00-20:00" },
              { value: 0, name: "20:00-22:00" },
              { value: 0, name: "22:00-23:00" },
              { value: 0, name: "23:00-24:00" },
            ],
          },
        ],
      });
    },
    chart_c() {
      var myChart3 = echarts.init(document.getElementById("chart3"));
      myChart3.setOption({
        color: ["#FF6C75", "#F8C478", "#578DD6"],
        tooltip: {
          trigger: "item",
          borderWidth: "0",
          formatter: function (params) {
            var result = "";
            result =
              "<div></div>" +
              params.data.name +
              ": " +
              params.data.value +
              "元 (" +
              params.percent +
              "%)";
            return result;
          },
          position: function (pos, params, dom, rect, size) {
            // 鼠标在左侧时 tooltip 显示到右侧，鼠标在右侧时 tooltip 显示到左侧。
            var obj = { top: 60 };
            obj[["left", "right"][+(pos[0] < size.viewSize[0] / 2)]] = 5;
            return obj;
          },
          backgroundColor: "#585858",
          textStyle: {
            fontSize: 5,
            color: "#fff",
          },
        },
        legend: {
          orient: "vertical",
          top: 200,
          data: [
            { name: "今日创建订单" },
            { name: " 昨日创建订单" },
            { name: "之前挂单订单" },
          ],
        },
        graphic: [
          {
            //环形图中间添加文字
            type: "text", //通过不同top值可以设置上下显示
            left: "center",
            top: "30%",
            style: {
              text: "订单金额",
              textAlign: "center",
              fill: "#999999", //文字的颜色
              width: 30,
              height: 30,
              fontSize: 12,
              fontFamily: "Microsoft YaHei",
            },
          },
          {
            type: "text",
            left: "center",
            top: "37%",
            style: {
              text: "5414.53",
              textAlign: "center",
              fill: "#333333",
              width: 30,
              height: 30,
              fontSize: 12,
            },
          },
        ],
        series: [
          {
            name: "访问来源",
            type: "pie",
            height: 200,
            radius: ["55%", "70%"],
            avoidLabelOverlap: false,
            hoverAnimation: false, //移入放大
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 5159.7, name: "今日创建订单" },
              { value: 260.05, name: " 昨日创建订单" },
              { value: 49.77, name: "之前挂单订单" },
            ],
          },
        ],
      });
    },
    chart_d() {
      var myChart4 = echarts.init(document.getElementById("chart4"));
      myChart4.setOption({
        color: ["#FF6C75", "#F8C478", "#578DD6", "#9891CD", "#87DAC7"],
        tooltip: {
          trigger: "item",
          borderWidth: "0",
          formatter: function (params) {
            var result = "";
            result =
              "<div class='a" +
              params.dataIndex +
              "'></div>" +
              params.data.name +
              ": " +
              params.data.value +
              " (" +
              params.percent +
              "%)";
            return result;
          },
          position: function (pos, params, dom, rect, size) {
            // 鼠标在左侧时 tooltip 显示到右侧，鼠标在右侧时 tooltip 显示到左侧。
            var obj = { top: 60 };
            obj[["left", "right"][+(pos[0] < size.viewSize[0] / 2)]] = 5;
            return obj;
          },
          backgroundColor: "#585858",
          textStyle: {
            fontSize: 5,
            color: "#fff",
          },
        },
        legend: {
          orient: "horizontal",
          top: 200,
          data: [
            { name: "离线" },
            { name: "空闲" },
            { name: "占用未充电" },
            { name: "充电中" },
            { name: "故障" },
          ],
        },
        graphic: [
          {
            //环形图中间添加文字
            type: "text", //通过不同top值可以设置上下显示
            left: "center",
            top: "30%",
            style: {
              text: "充电枪状态",
              textAlign: "center",
              fill: "#999999", //文字的颜色
              width: 30,
              height: 30,
              fontSize: 12,
              fontFamily: "Microsoft YaHei",
            },
          },
          {
            type: "text",
            left: "center",
            top: "37%",
            style: {
              text: "62",
              textAlign: "center",
              fill: "#333333",
              width: 30,
              height: 30,
              fontSize: 12,
            },
          },
        ],
        series: [
          {
            name: "访问来源",
            type: "pie",
            height: 200,
            radius: ["55%", "70%"],
            avoidLabelOverlap: false,
            hoverAnimation: false, //移入放大
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 0, name: "离线" },
              { value: 48, name: "空闲" },
              { value: 5, name: "占用未充电" },
              { value: 7, name: "充电中" },
              { value: 2, name: "故障" },
            ],
          },
        ],
      });
    },
    chart_e() {
      var myChart5 = echarts.init(document.getElementById("chart5"));

      myChart5.setOption({
        color: ["#ff8e6c", "#54CAAF", "#4D86D3", "#857CCC"],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "line",
            lineStyle: {
              type: "linear",
              color: "#419D87",
            },
          },
        },
        toolbox: {
          feature: {
            dataView: { show: true, readOnly: false },
            magicType: { show: true, type: ["line", "bar"] },
            restore: { show: true },
            saveAsImage: { show: true },
          },
        },
        legend: {
          itemWidth: 15,
          itemHeight: 10,
          top: 10,
          data: [
            { name: "充电量", icon: "circle" },
            { name: "充电总收入", icon: "circle" },
            { name: "充电电费", icon: "circle" },
            { name: "充电服务费", icon: "circle" },
          ],
        },
        xAxis: [
          {
            type: "category",
            data: [
              "01日",
              "02日",
              "03日",
              "04日",
              "05日",
              "06日",
              "07日",
              "08日",
              "09日",
              "10日",
              "11日",
              "12日",
            ],
            axisTick: {
              alignWithLabel: true,
            },
          },
        ],
        yAxis: [
          {
            type: "value",
            name: "充电量(kwh)",
            min: 0,
            max: 20000,
            interval: 4000,
            axisLabel: {
              formatter: "{value}",
            },
          },
          {
            type: "value",
            name: "充电金额(元)",
            min: 0,
            max: 18000,
            interval: 2000,
            axisLabel: {
              formatter: "{value}",
            },
          },
        ],
        series: [
          {
            name: "充电量",
            type: "bar",
            barWidth: 25, //柱图宽度
            data: [
              17000,
              13000,
              13811.27,
              12385,
              13948.44,
              16893,
              13562,
              16223,
              13286,
              12395,
              12819.51,
              12852,
            ],
          },
          {
            name: "充电总收入",
            type: "line",
            yAxisIndex: 1,
            symbolSize: 7,
            data: [
              17261.39,
              15951.9,
              13811.27,
              15317.38,
              14516.18,
              15429.92,
              14741.44,
              16476.74,
              13958.47,
              12238.63,
              12819.51,
              12072.69,
            ],
          },
          {
            name: "充电电费",
            type: "line",
            symbolSize: 7,
            yAxisIndex: 1,
            data: [
              10653.5,
              10024.17,
              9040.63,
              9828.84,
              9105.64,
              10423.79,
              10066.26,
              11197.22,
              9280.28,
              8175.93,
              8490.35,
              8178.08,
            ],
          },
          {
            name: "充电服务费",
            type: "line",
            symbolSize: 7,
            yAxisIndex: 1,
            data: [
              7152.48,
              6339.43,
              5373.55,
              6031.07,
              5410.54,
              5525.74,
              5246.36,
              5831.71,
              5058.09,
              4404.7,
              4657.35,
              4288.87,
            ],
          },
        ],
      });
      window.addEventListener("resize", function () {
        myChart5.resize();
      });
    },
    chart_f() {
      var myChart6 = echarts.init(document.getElementById("chart6"));
      myChart6.setOption({
        color: ["#4D86D3", "#ff8e6c", "#54CAAF"],
        calculable: false,
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "line",
            lineStyle: {
              type: "linear",
              color: "#419D87",
            },
          },
        },
        legend: {
          itemWidth: 15,
          itemHeight: 10,
          top: 10,
          data: [
            { name: "正常订单", icon: "circle" },
            { name: "异常订单", icon: "circle" },
          ],
        },
        xAxis: [
          {
            type: "category",
            data: [
              "01日",
              "02日",
              "03日",
              "04日",
              "05日",
              "06日",
              "07日",
              "08日",
              "09日",
              "10日",
              "11日",
              "12日",
            ],
            axisTick: {
              alignWithLabel: true,
            },
          },
        ],
        yAxis: [
          {
            type: "value",
            name: "订单量(笔)",
            min: 0,
            max: 1080,
            interval: 120,
            axisLabel: {
              formatter: "{value}",
            },
          },
        ],
        series: [
          {
            type: "line",
            symbolSize: 0, // symbol的大小设置为0
            showSymbol: false, // 不显示symbol
            lineStyle: {
              width: 0, // 线宽是0
              color: "rgba(0, 0, 0, 0)", // 线的颜色是透明的
            },
            name: "总订单数",
            data: [912, 890, 782, 906, 868, 861, 816, 1050, 852, 642, 707, 713],
          },
          {
            name: "正常订单",
            type: "bar",
            stack: "one",
            barWidth: 25, //柱图宽度
            data: [875, 858, 759, 882, 851, 834, 788, 1001, 821, 614, 691, 691],
          },
          {
            name: "异常订单",
            type: "bar",
            stack: "one",
            barWidth: 25, //柱图宽度
            data: [37, 32, 23, 24, 17, 27, 28, 49, 31, 28, 16, 22],
          },
        ],
      });
      window.addEventListener("resize", function () {
        myChart6.resize();
      });
    },
  },
};
</script>
<style>
.a0 {
  position: relative;
  top: -2.5px;
  display: inline-block;
  border-radius: 50%;
  width: 5px;
  height: 5px;
  margin-right: 5px;
  background: #ff6c75;
}
.a1 {
  position: relative;
  top: -2.5px;
  display: inline-block;
  border-radius: 50%;
  width: 5px;
  height: 5px;
  margin-right: 5px;
  background: #f8c478;
}
.a2 {
  position: relative;
  top: -2.5px;
  display: inline-block;
  border-radius: 50%;
  width: 5px;
  height: 5px;
  margin-right: 5px;
  background: #578dd6;
}
</style>
<style lang="scss" scoped>
.el-font-size {
  font-size: 14px;
}
.content {
  height: 475px;
  background: #f5f7fa;
  position: relative;
  display: flex;
  .chart1 {
    flex: 1;
    height: 450px;
    position: relative;
    margin: 10px;
    background: white;
    border-radius: 5px;
    .chart1_title {
      font-size: 14px;
      position: absolute;
      top: 10px;
      left: 20px;
    }
    .chart1_title2 {
      font-size: 8px;
      position: absolute;
      top: 40px;
      left: 20px;
      color: #999999;
    }
    .chart1_title3 {
      font-size: 28px;
      position: absolute;
      top: 60px;
      left: 20px;
      color: #3a79cf;
    }
    .chart1_title4 {
      font-size: 6px;
      position: absolute;
      top: 70px;
      right: 20px;
      color: #999999;
    }
    .chart1_title5 {
      font-size: 6px;
      position: absolute;
      top: 85px;
      right: 20px;
      color: #999999;
    }
    #chart1 {
      width: 300px;
      height: 300px;
      position: absolute;
      top: 100px;
    }
    #chart2 {
      width: 300px;
      height: 300px;
      position: absolute;
      top: 100px;
    }
    #chart3 {
      width: 300px;
      height: 300px;
      position: absolute;
      top: 100px;
    }
    #chart4 {
      width: 300px;
      height: 300px;
      position: absolute;
      top: 100px;
    }
    .chart1_bottom {
      position: absolute;
      bottom: 20px;
      left: 40px;
      font-size: 5px;
      color: #999999;
    }
  }
}
.content2 {
  height: 900px;
  width: 100%;
  background: #f5f7fa;
  position: relative;
  display: flex;
  flex-direction: column;
  margin-bottom: 40px;
  .chart {
    flex: 1;
    position: relative;
    margin: 0 10px 10px 10px;
    background: white;
    border-radius: 5px;
    .chart_zhexian {
      font-size: 14px;
      position: absolute;
      top: 30px;
      left: 25px;
    }
    #chart5 {
      width: 100%;
      height: 400px;
      top: 50px;
      position: absolute;
    }
    #chart6 {
      width: 100%;
      height: 400px;
      top: 50px;
      position: absolute;
    }
  }
}
</style>


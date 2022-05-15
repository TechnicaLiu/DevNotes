1. echarts中 legend的**formatter** 格式化为 百分比  如: 单选题 5%

   ```js
   		formatter: function(name) {
               var data = option.series[0].data;
               var total = 0;
               var tarValue;
               for (var i = 0, l = data.length; i < l; i++) {
                 total += data[i].value;
                 if (data[i].name == name) {
                   tarValue = data[i].value;
                 }
               }
               var p = ((tarValue / total) * 100).toFixed(2);
               return name + " " + " " + p + "%";
             },
   ```

2. 同一个页面遍历渲染出多个不同的echarts

   ```js
   <pie-chart
     :chart-option="getEchartData(item)"
     :height="'160px'"
     :width="'160px'"
   />
     
   // 获取数据函数
    getEchartData(data) {
         const config = {   // 基本参数
           grid: {
             top: 0,
             left: 0
           },
           color: ["rgba(0, 169, 124, 1)", "rgba(0, 169, 124, 0.5)", "rgba(0, 169, 124, 0.3)", "rgba(0, 169, 124, 0.2)"],
           tooltip: {
             trigger: "item",
             backgroundColor: "rgba(255,255,255,0.8)", // 通过设置rgba调节背景颜色与透明度
             color: "black",
             borderWidth: "1",
             borderColor: "rgb(156,209,255)",
             textStyle: {
               color: "black"
             },
             formatter: function (params) {
               let html = "<p style='text-align:left;'>" + params.seriesName + "</p>" + "<div>" +
                 "<div style='display: inline-block;width:10px; margin-right: 10px;height: 10px; background: " +
                 params.color +
                 "; border-radius: 50%;'></div>" + params.name + '项' + params.value + '人' + "</div>"
               return html
             }
           },
           series: [
             {
               name: "选项人数",
               type: 'pie',
               radius: ['30%', '70%'],
               minAngle: 30,
               avoidLabelOverlap: false,
               emphasis: {
                 disabled: false,
                 scale: false
               },
               label: {
                 show: true,
                 position: 'inside',
                 color: '#fff',
                 fontSize: 16
   
               },
               data: []
   
             }
           ]
         }
         if (data.prefixStatistic) {      // 给 series中的data赋值 
           if (data.prefixStatistic.length > 0) {
             config.series[0].data = [];
             data.prefixStatistic.forEach(item => {
               let param = {
                 name: item.prefix,
                 value: item.number
               }
               config.series[0].data.push(param)
             })
           }
   
   
         }
   
         return config;
   
       }
      
   ```

   
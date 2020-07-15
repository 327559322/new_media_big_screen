<template>
    <div class="container">
        <div class="head">
            <p>福建省XXX行业新媒体矩阵平台</p>
        </div>
        <div class="content">
            <div class="left-block">
                <div class="left-content">
                    <div class="left-content-total">
                        <div class="left-title"><p>运营数据</p></div>
                        <div class="total-bar">
                            <div class="left-content-total-item" v-for="(item,index) in leftTopBarData">
                                <p :class="index%2 === 0 ?'yellow':'red'">{{item.count}}</p>
                                <p>{{item.label}}</p>
                            </div>
                        </div>
                    </div>
                    <div class="left-content-chart">
                        <div class="content-box chart-line-block">
                            <div class="box-title">
                                <img src="./assets/chart-title.png">
                                <span>文章数和评论数趋势图</span>
                            </div>
                            <div id="leftChartLine"></div>
                        </div>

                        <div class="content-box chart-bar-block">
                            <div class="box-title">
                                <img src="./assets/chart-title.png">
                                <span>粉丝画像数</span>
                            </div>
                            <div id="leftChartBar"></div>
                        </div>
                    </div>
                    <div class="left-content-grid">
                        <div class="content-box">
                            <div class="box-title">
                                <img src="./assets/chart-title.png">
                                <span>阅读数TOP50</span>
                            </div>
                            <div class="common-table">
                                <table cellspacing="0">
                                    <tr>
                                        <th width="404px">标题</th>
                                        <th width="162px">发布时间</th>
                                        <th width="141px">阅读数</th>
                                        <th width="141px">在看数</th>
                                        <th width="142px">点赞数</th>
                                        <th width="142px">评论数</th>
                                        <th width="117px">是否原创</th>
                                    </tr>
                                </table>
                                <div class="turns-table-body" style="height: 373px">
                                    <table cellspacing="0">
                                        <tr v-for="(item, index) in leftTableData.data.concat(leftTableData.data)">
                                            <td width="404px"  @mouseover="showLeftTablePop($event,item)" @mouseout="hideLeftTablePop">
                                                    <img v-if="item.hasFireIcon" class="fire" src="./assets/fire@2x.png">
                                                    <img v-else-if="item.hasLineIcon " class="fire" src="./assets/line-graph@2x.png">
                                                    <img v-else class="fire" style="visibility: hidden">
                                                <p>{{item.title}}</p>
                                            </td>
                                            <td width="162px">{{item.publishTime}}</td>
                                            <td width="141px">{{item.readNum}}</td>
                                            <td width="141px">{{item.lookNum}}</td>
                                            <td width="142px">{{item.likeNum}}</td>
                                            <td width="142px">{{item.discNum}}</td>
                                            <td width="117px">{{item.isSelf}}</td>
                                        </tr>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="right-block">
                <div class="top-content">
                    <Map @freshData="freshData"></Map>
                    <div class="map-info">
                        <p class="map-title">公众号分布图</p>
                        <div class="map-box">
                            <div>
                                <p>
                                    <span>公众号总数量（万）</span>
                                    <span class="yellow">1,000,000,000</span>
                                </p>
                                <p>
                                    <span>在看数（人）</span>
                                    <span class="red">1,000,000,000</span>
                                </p>
                            </div>
                            <div>
                                <p>
                                    全省共<i>15</i>个公众号<br>
                                    近30天增加<i>15</i>个公众号<br>
                                    遍布<i>10</i>个市<br>
                                    共<i>528</i>，<i>588</i>，<i>69</i>篇文章<br>
                                    日平均阅读量<i>10</i>万次<br>
                                    日平均发文数<i>10</i>万篇
                                </p>
                            </div>

                        </div>
                    </div>
                </div>
                <div class="bottom-content">
                    <div class="top-bar">
                        <div class="top-tab selected">
                            <p>日榜单</p>
                            <p>2020/06/01</p>
                        </div>
                        <div class="top-tab">
                            <p>周榜单</p>
                            <p>20200601-20200604</p>
                        </div>
                        <div class="top-tab">
                            <p>月榜单</p>
                            <p>20200601-20200630</p>
                        </div>
                    </div>
                    <div class="content">
                        <div class="right-table common-table">
                            <table cellspacing="0">
                                <tr>
                                    <th width="300px">公众号</th>
                                    <th width="200px">指数</th>
                                    <th width="200px">粉丝数</th>
                                    <th width="200px">文章数</th>
                                    <th width="200px">阅读数</th>
                                    <th width="200px">在看数</th>
                                </tr>
                            </table>
                            <div class="turns-table-body" style="height: 800px">
                                <table cellspacing="0">
                                    <tr :class="item.isSelected ? 'selected': ''" v-for="(item, index) in rightTableData.concat(rightTableData)"
                                        @click="selectPublicNum($event, index)">
                                        <td width="300px">
                                            <div class="public-num">
                                                <span :class="'color'+index % rightTableData.length">{{index > rightTableData.length - 1 ? index - (rightTableData.length - 1):index + 1}}</span>
                                                <img>
                                                <p>
                                                    <span>{{item.school}}</span>
                                                    <span>{{item.publicNum}}</span>
                                                </p>
                                            </div>
                                        </td>
                                        <td width="200px">
                                            <p class="index">
                                                <span>{{item.kpi[0]}}</span>
                                                <span>↑{{item.kpi[1]}}</span>
                                            </p>
                                        </td>
                                        <td width="200px">{{item.fansNum}}</td>
                                        <td width="200px">{{item.articleNum}}</td>
                                        <td width="200px">{{item.readNum}}</td>
                                        <td width="200px">{{item.lookingNum}}</td>
                                    </tr>
                                </table>
                            </div>
                        </div>
                        <div class="right-chart">
                            <div class="content-box">
                                <div class="box-title">
                                    <img src="./assets/chart-title.png">
                                    <span>文章数和评论数趋势图</span>
                                </div>
                                <div id="rightTopChartLine"></div>
                            </div>
                            <div class="content-box">
                                <div class="box-title">
                                    <img src="./assets/chart-title.png">
                                    <span>阅读数和点赞数趋势图</span>
                                </div>
                                <div id="rightBottomChartLine"></div>
                            </div>
                        </div>
                    </div>
                </div>
                <img class="earth-bg" src="./assets/earth.png">
            </div>
        </div>
        <div class="left-table-pop" :style="{top:leftTableData.popOffSet.top+'px',left:leftTableData.popOffSet.left+'px',display:(leftTableData.isShowPop ? 'flex':'none')}">
            <img>
            <div>
                <p>{{leftTableData.popData.title}}</p>
                <p>
                    <span v-for="item in leftTableData.popData.tag">{{item}}</span>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
    import Map from './components/Map.vue'
    import $ from 'jquery'
    const echarts = require('echarts');
    export default {
        name: 'App',
        components: {
            Map
        },
        data(){
          return{
              chartsAxisOption : { // 坐标轴统一样式
                  axisLine: {
                      lineStyle: {
                          color: '#175BAF'
                      }
                  },
                  axisLabel: {
                      textStyle: {
                          color: '#E7E7E7',
                      },
                      fontSize:24
                  },
                  splitLine: {
                      lineStyle: {
                          color: '#232FA9'
                      }
                  },
              },
              leftTopBarData:[
                  {count:'1,000,000,000',label:'文字总数量(篇)'},
                  {count:'1,000,000,000',label:'评论总数量(篇)'},
                  {count:'1,000,000,000',label:'阅读总数量(次)'},
                  {count:'1,000,000,000',label:'在看总数量(次)'}
              ],
              leftTableData:{
                  data:[
                      {
                          title:'1为什么学习在孩子成长中占最高权重比？',
                          publishTime:'2020-06-21',
                          readNum: '1,000,000',
                          lookNum: '1,000,000',
                          likeNum: '1,000,000',
                          discNum: '1,000,000',
                          isSelf: '是',
                          tag:['福州大学','福州大学'],
                          hasFireIcon:true,
                      },
                      {
                          title:'2为什么学习在孩子成长中占最高权重比？',
                          publishTime:'2020-06-21',
                          readNum: '1,000,000',
                          lookNum: '1,000,000',
                          likeNum: '1,000,000',
                          discNum: '1,000,000',
                          isSelf: '是',
                          tag:['福州大学','福州大学'],
                          hasLineIcon:true
                      },
                      {
                          title:'3为什么学习在孩子成长中占最高权重比？',
                          publishTime:'2020-06-21',
                          readNum: '1,000,000',
                          lookNum: '1,000,000',
                          likeNum: '1,000,000',
                          discNum: '1,000,000',
                          isSelf: '是',
                          tag:['福州大学','福州大学']
                      },
                      {
                          title:'4为什么学习在孩子成长中占最高权重比？',
                          publishTime:'2020-06-21',
                          readNum: '1,000,000',
                          lookNum: '1,000,000',
                          likeNum: '1,000,000',
                          discNum: '1,000,000',
                          isSelf: '是',
                          tag:['福州大学','福州大学']
                      },
                      {
                          title:'5为什么学习在孩子成长中占最高权重比？',
                          publishTime:'2020-06-21',
                          readNum: '1,000,000',
                          lookNum: '1,000,000',
                          likeNum: '1,000,000',
                          discNum: '1,000,000',
                          isSelf: '是',
                          tag:['福州大学','福州大学']
                      },
                  ],
                  popData:{}, // 表格弹窗数据
                  popOffSet:{ // 表格弹窗位置
                      top:0,
                      left:0
                  },
                  isShowPop:false // 是否展示弹窗
              },
              rightTableData:[ // 右边表格数据
                  {isSelected:true,school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
                  {school:'福州大学',publicNum:'jkji55455', kpi:[592,'1999+'], fansNum: '1,000,000', articleNum: '1,000,000', readNum: '1,000,000', lookingNum: '1,000,000'},
              ],
              tableRollTimer:{ // 表格滚动定时器
                  leftTable:{}, // 左边表格
                  rightTable:{} // 右边表格
              }
          }
        },
        mounted() {
            this.renderLine(document.getElementById('leftChartLine'),
                {name:'文章数', data:[0, 120, 112, 100, 300, 80]},
                {name:'评论数', data:[0, 170, 80, 130, 450, 120]},
                ['', '2020-09', '2020-10', '2020-11', '2020-12', '2021-01'])
            this.renderLine(document.getElementById('rightTopChartLine'),
                {name:'文章数', data:[0, 120, 112, 100, 300]},
                {name:'评论数', data:[0, 170, 80, 130, 450]},
                ['', '2020-09', '2020-10', '2020-11', '2020-12'])
            this.renderLine(document.getElementById('rightBottomChartLine'),
                {name:'阅读数', data:[0, 120, 112, 100, 300]},
                {name:'点赞数', data:[0, 170, 80, 130, 450]},
                ['', '2020-09', '2020-10', '2020-11', '2020-12'])
            this.renderLeftBar(
                ['2020-09', '2020-09', '2020-09', '2020-09', '2020-09', '2020-09', '2020-09', '2020-09'],
                {
                    val1:[120, 132, 101, 134, 90, 230, 210, 200],
                    val2:[220, 182, 191, 234, 290, 330, 310, 300],
                    val3:[150, 232, 201, 154, 190, 330, 410,200]
                }
            )
            $('.right-block .top-bar .top-tab').click(function () {
                $(this).addClass('selected').siblings().removeClass('selected')
            })
            if(this.leftTableData.data.length > 4){
                this.tableRoll($('.left-content-grid .turns-table-body table'),'left')
            }
            if(this.rightTableData.length > 8){
                this.tableRoll($('.right-block .turns-table-body table'),'right')
            }
        },
        methods: {
            /**
             * 图形渲染公用方法
             * **/
            renderChart(target, option) {
                echarts.init(target).setOption(option);
            },
            /**
             * 折线图渲染
             * **/
            renderLine(target, firstLineOpt, secondLineOpt,xAxisLabel){
                this.renderChart(target, {
                    /*title: {
                        text: '111'
                    },*/
                    tooltip: {
                        show: true,
                        formatter: function (params) {
                            console.log(params.marker)
                            const redMarker = '<span class="chart-legend-marker red"></span>'
                            const yellowMarker = '<span class="chart-legend-marker yellow"></span>'
                            return yellowMarker + firstLineOpt.name + '：' + firstLineOpt.data[params.dataIndex] + '<br>' + redMarker + secondLineOpt.name +'：' + secondLineOpt.data[params.dataIndex]
                        },
                        textStyle:{
                            fontSize:12
                        }
                    },
                    legend: {
                        type: 'plain',
                        right: '6%',
                        top: '2%',
                        icon: 'circle',
                        itemWidth: 25,
                        itemHeight: 25,
                        textStyle: {
                            color: '#fff',
                            fontSize:28
                        }
                    },
                    grid: {
                        left: '6%',
                        right:'7%',
                        top:'20%',
                        bottom:'8%'
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                        data: xAxisLabel,
                        ...this.chartsAxisOption,
                        axisTick: {
                            alignWithLabel: true
                        }
                    },
                    yAxis: {
                        type: 'value',
                        ...this.chartsAxisOption,
                    },
                    series: [{
                        name: firstLineOpt.name,
                        data: firstLineOpt.data,
                        type: 'line',
                        areaStyle: {
                            color: {
                                type: 'linear',
                                x: 0,
                                y: 0,
                                x2: 0,
                                y2: 2,
                                colorStops: [{
                                    offset: 0, color: '#FFC000' // 0% 处的颜色
                                }, {
                                    offset: 1, color: 'transparent' // 100% 处的颜色
                                }],
                                global: false // 缺省为 false
                            },
                            opacity: 0.5
                        },
                        lineStyle: {
                            color: '#FFC000'
                        },
                        itemStyle: {
                            color: '#FFC000',
                            borderWidth: 6,
                            borderColor: 'rgba(255, 192, 0, 0.5)',
                        },
                        symbol: 'circle',
                        symbolSize: 8
                    }, {
                        name: secondLineOpt.name,
                        data: secondLineOpt.data,
                        type: 'line',
                        areaStyle: {
                            color: {
                                type: 'linear',
                                x: 0,
                                y: 0,
                                x2: 0,
                                y2: 1,
                                colorStops: [{
                                    offset: 0, color: '#FF0072' // 0% 处的颜色
                                }, {
                                    offset: 1, color: 'transparent' // 100% 处的颜色
                                }],
                                global: false // 缺省为 false
                            },
                            opacity: 0.5
                        },
                        lineStyle: {
                            color: '#FF0072'
                        },
                        itemStyle: {
                            color: '#FF0072',
                            borderWidth: 6,
                            borderColor: 'rgba(255, 0, 144, 0.5)',
                        },
                        symbol: 'circle',
                        symbolSize: 8
                    }]
                })
            },
            /**
             * 柱状图渲染
             * **/
            renderLeftBar(dateData, value){
                this.renderChart(document.getElementById('leftChartBar'),{
                    legend: {
                        type: 'plain',
                        right: '4%',
                        top: '2%',
                        icon: 'circle',
                        itemWidth: 25,
                        itemHeight: 25,
                        textStyle: {
                            color: '#fff',
                            fontSize:28
                        }
                    },
                    label: {
                        show: true,
                    },
                    grid: {
                        left: '10%',
                        right:'4%',
                        top:'14%',
                        bottom:'8%'
                    },
                    xAxis: [
                        {
                            type: 'value',
                            ...this.chartsAxisOption,

                        }
                    ],
                    yAxis: [
                        {
                            type: 'category',
                            ...this.chartsAxisOption,
                            data: dateData
                        }
                    ],
                    series: [
                        {
                            name: '数值1',
                            type: 'bar',
                            stack: '数值',
                            itemStyle:{
                              color:'#0E6DE9'
                            },
                            data: value.val1
                        },
                        {
                            name: '数值2',
                            type: 'bar',
                            stack: '数值',
                            itemStyle:{
                                color:'#AC4ED3'
                            },
                            data: value.val2
                        },
                        {
                            name: '数值3',
                            type: 'bar',
                            stack: '数值',
                            itemStyle:{
                                color:'#E6AF08'
                            },
                            data: value.val3
                        }
                    ]
                })
            },
            /**
             * 显示左边表格弹窗
             * **/
            showLeftTablePop(event, item){
                this.$set(this.leftTableData, 'popData', item)
                this.$set(this.leftTableData.popOffSet, 'left', $(event.target).offset().left + 400)
                this.$set(this.leftTableData.popOffSet, 'top', $(event.target).offset().top - 100)
                this.$set(this.leftTableData,'isShowPop', true)
            },
            /**
             * 隐藏左边表格弹窗
             * **/
            hideLeftTablePop(){
                this.$set(this.leftTableData,'isShowPop', false)
            },
            /**
             * 右边表格行点击事件
             * **/
            selectPublicNum($event, index){
                /*$($event.currentTarget).addClass('selected').siblings().removeClass('selected')
                $('.right-table .turns-table-body tr:eq(' + (index + this.rightTableData.length) + ')').addClass('selected')
                this.renderLine(document.getElementById('rightTopChartLine'),
                    {name:'文章数', data:[0, 320, 112, 100, 300, 80]},
                    {name:'评论数', data:[0, 270, 80, 130, 450, 120]},
                    ['', '2020-09', '2020-10', '2020-11', '2020-12', '2021-01'])
                this.renderLine(document.getElementById('rightBottomChartLine'),
                    {name:'阅读数', data:[0, 120, 112, 100, 300, 80]},
                    {name:'点赞数', data:[0, 170, 80, 130, 450, 120]},
                    ['', '2020-09', '2020-10', '2020-11', '2020-12', '2021-01'])*/
            },
            /**
             * 通用表格滚动
             * **/
            tableRoll($target,tableType){
                let tblTop = 0;
                let speedHq = 50; // 数值越大越慢
                let tableHeight = $target.height()/2
                function RollStart(){
                    if(tblTop <= -tableHeight){
                        tblTop = 0;
                    } else {
                        tblTop -= 1;
                    }
                    $target.css('top', tblTop+'px');
                }
                if(tableType === 'left'){
                    clearInterval(this.tableRollTimer.leftTable);
                    this.tableRollTimer.leftTable = setInterval(RollStart,speedHq);
                    $target.hover(()=>{
                        clearInterval(this.tableRollTimer.leftTable);
                    },()=>{
                        clearInterval(this.tableRollTimer.leftTable);
                        this.tableRollTimer.leftTable = setInterval(RollStart,speedHq);
                    })
                } else if(tableType === 'right'){
                    clearInterval(this.tableRollTimer.rightTable);
                    this.tableRollTimer.rightTable = setInterval(RollStart,speedHq);
                    $target.hover(()=>{
                        clearInterval(this.tableRollTimer.rightTable);
                    },()=>{
                        clearInterval(this.tableRollTimer.rightTable);
                        this.tableRollTimer.rightTable = setInterval(RollStart,speedHq);
                    })
                }
            },
            /**
             * 地图鼠标hover刷新左右视图方法
             * **/
            freshData(data){
                function getRandom(){ // 产生随机数
                    return Math.floor(Math.random() * 300)
                }
                console.log('parent_fresh')
                this.leftTopBarData = [
                    {count:'2,000,000,'+getRandom(),label:'文字总数量'},
                    {count:'2,000,000,'+getRandom(),label:'评论总数量'},
                    {count:'2,000,000,'+getRandom(),label:'阅读总数量'},
                    {count:'2,000,000,'+getRandom(),label:'在看总数量'}
                ]
                this.renderLeftBar(
                    ['2020-09', '2020-09', '2020-09', '2020-09', '2020-09', '2020-09', '2020-09', '2020-09'],
                    {
                        val1:[getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom()],
                        val2:[getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom()],
                        val3:[getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom(), getRandom(),getRandom()]
                    }
                )
                this.renderLine(document.getElementById('leftChartLine'),
                    {name:'文章数', data:[0, getRandom(), getRandom(), getRandom(), getRandom(), getRandom()]},
                    {name:'评论数', data:[0, getRandom(), getRandom(), getRandom(), getRandom(), getRandom()]},
                    ['', '2020-09', '2020-10', '2020-11', '2020-12', '2021-01'])
                this.renderLine(document.getElementById('rightTopChartLine'),
                    {name:'文章数', data:[0, getRandom(), getRandom(), getRandom(), getRandom(), getRandom()]},
                    {name:'评论数', data:[0, getRandom(), getRandom(), getRandom(), getRandom(), getRandom()]},
                    ['', '2020-09', '2020-10', '2020-11', '2020-12', '2021-01'])
                this.renderLine(document.getElementById('rightBottomChartLine'),
                    {name:'阅读数', data:[0, getRandom(), getRandom(), getRandom(), getRandom(), getRandom()]},
                    {name:'点赞数', data:[0, getRandom(), getRandom(), getRandom(), getRandom(), getRandom()]},
                    ['', '2020-09', '2020-10', '2020-11', '2020-12', '2021-01'])
                this.$set(this.leftTableData,'data',this.leftTableData.data.map((item)=>{
                    item.readNum = '1,000,000'+ getRandom()
                    return item
                }))
                /*this.$set(this.rightTableData,this.rightTableData.map((item)=>{
                    item.fansNum = '1,000,'+ getRandom()
                    return item
                }))*/
                if(this.leftTableData.data.length > 4){
                    this.tableRoll($('.left-content-grid .turns-table-body table'),'left')
                }
                /*if(this.rightTableData.length > 8){
                    this.tableRoll($('.right-content .turns-table-body table'),'right')
                }*/
                this.rightTableData = this.rightTableData.map((item, index)=>{ // 改变右边表格选中的行
                        item.isSelected = false
                       if(index === data.currentCityIndex){
                           item.isSelected = true
                           return item
                       } else {
                           return item
                       }
                })
            }
        }
    }
</script>

<style lang="scss">
    * {
        margin: 0;
        box-sizing: border-box;
    }

    html, body {
        height: 100%;
        width: 100%;
    }

    .container {
        min-width: 3840px;
        min-height: 2160px;
        width: 100%;
        height: 100%;
        background-image: url("./assets/bg@2x.png");
        background-size: 100% 100%;
        background-image: -webkit-image-set(
                        url("./assets/bg.png") 1x,
                        url("./assets/bg@2x.png") 2x);

        .head {
            width: 100%;
            height: 125px;
            background-image: url("./assets/head@2x.png");
            background-size: 100% 100%;
            background-image: -webkit-image-set(
                            url("./assets/head.png") 1x,
                            url("./assets/head@2x.png") 2x);
            color: #00E4FF;
            font-size: 45px;
            text-align: center;
            line-height: 105.5px;

            > p {
                font-weight: bold;
                background: linear-gradient(0deg, rgba(0, 174, 255, 1) 0%, rgba(0, 255, 210, 1) 100%);
                -webkit-background-clip: text;
                -webkit-text-fill-color: transparent;
            }
        }
        .content {
            display: flex;
            flex-direction: row;
            width: 100%;
            height: calc(100% - 125px);
            .left-block {
                /*width: 1489.5px;*/
                height: 100%;
                .left-content {
                    width: 1384.5px;
                    height: calc(100% - 60px);
                    margin: 0 0 0 60px;
                    background-image: url("./assets/left-block@2x.png");
                    background-size: 100% 100%;
                    background-image: -webkit-image-set(
                                    url("./assets/left-block.png") 1x,
                                    url("./assets/left-block@2x.png") 2x);
                    display: flex;
                    flex-direction: column;
                    .left-content-total {
                        width: 100%;
                        height: 16.7%;
                        /*height: 25%;*/
                        margin: 0 auto 0 auto;
                        position: relative;
                        display: flex;
                        flex-direction: column;
                        align-items: center;
                        .left-title{
                            margin-top: 3%;
                            width: 100%;
                            >p{
                                font-size:45px;
                                font-weight:bold;
                                padding-left:54px;
                                background:linear-gradient(0deg,rgba(0,174,255,1) 0%, rgba(0,228,255,1) 100%);
                                -webkit-background-clip:text;
                                -webkit-text-fill-color:transparent;
                            }
                        }
                        .total-bar{
                            bottom: 0;
                            width: 1286.5px;
                            background-image: url("./assets/left-content-total-bar@2x.png");
                            background-size: 100% 100%;
                            background-image: -webkit-image-set(
                                            url("./assets/left-content-total-bar.png") 1x,
                                            url("./assets/left-content-total-bar@2x.png") 2x);
                            display: flex;
                            flex-direction: row;
                            height: 153px;
                            margin-top: 82px;
                            .left-content-total-item {
                                width: 25%;
                                display: flex;
                                flex-direction: column;
                                align-items: center;
                                justify-content: center;

                                > p.yellow {
                                    color: #FFDE00
                                }

                                > p.red {
                                    color: #FF0072
                                }

                                > p:first-child {
                                    font-size: 35px;
                                    font-weight: bold;
                                }

                                > p:last-child {
                                    font-size: 30px;
                                    color: #E7E7E7;
                                }
                            }
                        }

                    }
                    .left-content-chart {
                        width: 100%;
                        height: 57.2%;
                        display: flex;
                        flex-direction: column;
                        padding: 0 50px;
                        .chart-line-block {
                            width: 100%;
                            height: 50%;
                            #leftChartLine {
                                width: 100%;
                                height: 90%;
                                margin-top: 3%;
                            }
                        }

                        .chart-bar-block {
                            width: 100%;
                            height: 50%;
                            #leftChartBar {
                                width: 100%;
                                height: 90%;
                                margin-top: 3%;
                            }
                        }
                    }
                    .left-content-grid{
                        width: 1249.5px;
                        height: 26.1%;//calc(100% - 409px);
                        margin: 0 auto;
                        .box-title{
                            position: relative;
                            left: 0;
                            margin-bottom: 12px;
                        }
                        .fire{
                           width: 20px;
                            height: 24.6px;
                            vertical-align: middle;
                            margin-right: 10px;
                        }
                        .line{

                        }
                    }
                }
            }

            /*.center-block {
                width: calc(100% - 2711.5px);
                text-align: center;
                position: relative;
                overflow: hidden;

                .earth-bg {
                    animation: earthRotete 20s linear infinite;
                    display: block;
                    width: 698.5px;
                    position: absolute;
                    left: 50%;
                    top: 40%;
                }
                .map-title{
                    font-size:35px;
                    font-weight:bold;
                    margin-top: 37px;
                    background:linear-gradient(0deg,rgba(0,174,255,1) 0%, rgba(0,255,210,1) 100%);
                    -webkit-background-clip:text;
                    -webkit-text-fill-color:transparent;
                    position: relative;
                    z-index: 2;
                }
                .count-total{
                    position: relative;
                    z-index: 2;
                    margin-top: 30px;
                    span{
                        font-weight:400;
                        color:#E7E7E7;
                        font-size:20px;
                        margin-right: 40px;
                        i{
                            font-weight: bold;
                            color:#FFDE00;
                        }
                        &:last-child i{
                            color:#FF0072;
                        }
                    }
                }
                .map-bottom{
                    width: 805.5px;
                    height: 118px;
                    margin: auto;
                    position: absolute;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    bottom: 0;
                    left: 50%;
                    font-size: 20px;
                    color:#E7E7E7;
                    transform: translate(-50%);
                    background-image: url("./assets/map-bottom-bg@2x.png");
                    background-size: 100% 100%;
                    background-image: -webkit-image-set(
                                    url("./assets/map-bottom-bg.png") 1x,
                                    url("./assets/map-bottom-bg@2x.png") 2x);

                    p{
                        padding: 0 67.5px;
                        strong{
                            color:#FFDE00
                        }
                    }
                }
            }*/

            .right-block {
                /*width: 1358px;*/
                height: 100%;
                margin: auto;
                position: relative;
                .top-content{
                    height: 43.5%;
                    width: 2293.5px;
                    position: relative;
                    .map-info{
                        width: 635.5px;
                        top: 80px;
                        left: 23px;
                        height: auto;
                        position: absolute;
                        z-index: 2;
                        .map-title{
                            font-size:45px;
                            font-weight:bold;
                            margin-bottom: 34px;
                            background:linear-gradient(0deg,rgba(0,174,255,1) 0%, rgba(0,255,210,1) 100%);
                            -webkit-background-clip:text;
                            -webkit-text-fill-color:transparent;
                        }
                        .map-box{
                            height: 465.5px;
                            width: 100%;
                            padding: 30px;
                            background-image: url("./assets/map-box@2x.png");
                            background-size: 100% 100%;
                            background-image: -webkit-image-set(
                                            url("./assets/map-box.png") 1x,
                                            url("./assets/map-box@2x.png") 2x);
                            >div {
                                &:first-child{
                                    border-bottom: 1px solid #041EBB;
                                    height: 125.5px;
                                    p{
                                        line-height: 50px;
                                        span:first-child{
                                            color:#E7E7E7;
                                            font-size: 30px;
                                        }
                                        span:last-child{
                                            font-size: 35px;
                                            font-weight: bold;
                                            &.red{
                                                color:#FF0072
                                            }
                                            &.yellow{
                                                color:#FFDE00
                                            }
                                        }
                                    }
                                }
                                &:last-child{
                                    padding-top: 20px;
                                  p{
                                      line-height: 40px;
                                      color:#E7E7E7;
                                      font-size: 30px;
                                      i{
                                          font-size: 35px;
                                          font-weight: bold;
                                          color:#FFDE00;
                                      }
                                  }
                                }
                            }
                        }

                    }
                }
                .bottom-content{
                    height: 56.5%;
                    width: 2293.5px;
                    padding-bottom: 60px;
                    .top-bar {
                        height: 114px;
                        .top-tab {
                            width: 424px;
                            height: 100%;
                            cursor: pointer;
                            float: left;
                            background-image: url("./assets/right-top-tab2@2x.png");
                            background-size: 100% 100%;
                            background-image: -webkit-image-set(
                                            url("./assets/right-top-tab2.png") 1x,
                                            url("./assets/right-top-tab2@2x.png") 2x);
                            text-align: center;
                            display: flex;
                            flex-direction: column;
                            align-items: center;
                            justify-content: center;

                            > p:first-child {
                                font-size: 40px;
                                font-weight: bold;
                            }

                            > p:last-child {
                                font-size: 25px;
                            }

                            p {
                                color: #0066FF;
                            }

                            &:nth-child(2) {
                                transform: translate(-20%);
                            }

                            &:nth-child(3) {
                                transform: translate(-40%);
                            }

                            &.selected {
                                background-image: url("./assets/right-top-tab@2x.png");
                                background-size: 100% 100%;
                                background-image: -webkit-image-set(
                                                url("./assets/right-top-tab.png") 1x,
                                                url("./assets/right-top-tab@2x.png") 2x);

                                p {
                                    background: linear-gradient(0deg, rgba(0, 174, 255, 1) 0%, rgba(0, 255, 210, 1) 100%);
                                    -webkit-background-clip: text;
                                    -webkit-text-fill-color: transparent;
                                }
                            }
                        }
                    }
                    .content {
                        width: 100%;
                        height: calc(100% - 114px);
                        float: left;
                        display: flex;
                        background-image: url("./assets/right-block@2x.png");
                        background-size: 100% 100%;
                        background-image: -webkit-image-set(
                                        url("./assets/right-block.png") 1x,
                                        url("./assets/right-block@2x.png") 2x);
                        .right-table{
                            width: 1300px;
                            margin-top: 48px;
                            margin-left: 54px;
                            table{
                                tr:not(:first-child){
                                    cursor: pointer;
                                }
                                tr.selected{
                                    background:linear-gradient(90deg,rgba(32,26,143,1),rgba(4,7,62,1));
                                }
                                td{
                                    height: 100px;
                                    > .public-num{
                                        display: flex;
                                        align-items: center;
                                        padding-left: 20px;
                                        > span:first-child{
                                            font-size: 30px;
                                            color:#008AFF;
                                            width: 20px;
                                            &.color0{
                                                color:#FFC000
                                            }
                                            &.color1{
                                                color:#FF0072
                                            }
                                            &.color2{
                                                color:#00E0FF
                                            }
                                        }
                                        > img{
                                            width: 60px;
                                            height: 60px;
                                            border-radius: 100%;
                                            margin: 0 10px;
                                            flex-shrink: 0;
                                        }
                                        > p{
                                            display: flex;
                                            flex-direction: column;
                                            span:first-child{
                                                font-weight:bold;
                                                font-size:20px;
                                            }
                                            span:last-child{
                                                font-weight:400;
                                                font-size: 17px;
                                            }
                                        }
                                    }
                                    > .index{
                                        display: flex;
                                        align-items: center;
                                        > span:first-child{
                                            width: 80px;
                                            height: 40px;
                                            line-height: 40px;
                                            text-align: center;
                                            background-color: #092DD6;
                                            margin-right: 6px;
                                            font-size: 25px;
                                        }
                                        > span:last-child{
                                            color: #FFC000;
                                            font-weight:bold;
                                            font-size: 25px;
                                        }
                                    }
                                }
                            }
                        }
                        .right-chart{
                            margin-top: 50px;
                            width: calc(100% - 1300px);
                            .content-box{
                                margin-left: 46px;
                                margin-top: 10px;
                                height: 47%;
                                .box-title{
                                    top: -20px;
                                }
                                #rightTopChartLine,#rightBottomChartLine{
                                    width: 100%;
                                    height: 90%;
                                    margin-top: 3%;
                                }
                            }
                        }
                    }

                }
                .earth-bg {
                    animation: earthRotete 20s linear infinite;
                    display: block;
                    width: 905px;
                    position: absolute;
                    left: 1009px;
                    top: 12px;
                    z-index: 0;
                }
            }
            .content-box{
                position: relative;
                .box-title {
                    position: absolute;
                    background:linear-gradient(0deg,rgba(0,174,255,1) 0%, rgba(0,228,255,1) 100%);
                    -webkit-background-clip:text;
                    -webkit-text-fill-color:transparent;
                    font-size:35px;
                    font-weight:bold;
                    top: 6%;
                    left: 0;
                    >img{
                        vertical-align: middle;
                        margin-right: 10px;
                    }
                    >span{
                        vertical-align: middle;
                    }
                }
            }
            .chart-legend-marker{
                display:inline-block;
                margin-right:5px;
                border-radius:10px;
                width:10px;
                height:10px;
                &.red{
                    background-color:#FF0072;
                }
                &.yellow{
                    background-color:#FFC000;
                }
            }
            .common-table{
                table{
                    width: 100%;
                    th{
                        background-color: #033fff4d;
                        color:#2CB6FE;
                        font-size: 24px;
                        height: 55px;
                        text-align: center;
                    }
                    td{
                        color:#E7E7E7;
                        font-size:24px;
                        height: 74.5px;
                        // padding: 4px;
                        text-align: center;
                        &:first-child{
                            text-align: left;
                            font-weight: bold;
                            cursor: pointer;
                            display: flex;
                            align-items: center;
                        }
                    }
                }
                .turns-table-body{
                    overflow: hidden;
                    position: relative;
                    table{
                        position: absolute;
                        top: 0;
                        tr:not(:last-child){
                            td{
                                border-bottom: 1.5px solid #041877;
                            }
                        }
                    }
                }
            }
        }
        .left-table-pop{
            position: absolute;
            width:645.5px;
            height:130.5px;
            left: 0;
            top: 0;
            padding: 13px 30px 26px 13px;
            display: flex;
            background-image: url("./assets/left-table-pop-bg@2x.png");
            background-size: 100% 100%;
            background-image: -webkit-image-set(
                            url("./assets/left-table-pop-bg.png") 1x,
                            url("./assets/left-table-pop-bg@2x.png") 2x);
            > img{
                width: 130px;
                height: 90px;
                margin-right: 9.5px;
                flex-shrink: 0;
            }
            > div{
                p:first-child{
                    font-size: 18px;
                    color:#E0E0E0;
                    overflow: hidden;
                    text-overflow: ellipsis;
                    display: -webkit-box;
                    -webkit-line-clamp: 2;
                    -webkit-box-orient: vertical;
                }
                p:last-child{
                    text-align: center;
                    margin-top: 20px;
                    > span{
                        display: inline-block;
                        background-color: #031776;
                        // padding: 5px 6px;
                        font-size: 14px;
                        color:#00E0FF;
                        width: 70px;
                        height: 25px;
                        line-height: 25px;
                        text-align: center;
                        &:first-child{
                            margin-right: 8px;
                        }
                        &:last-child{
                            margin-left: 8px;
                        }
                    }
                }
            }

        }
    }

    @keyframes earthRotete {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
</style>

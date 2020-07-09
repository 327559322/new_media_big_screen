<template>
    <div class="map-area">
        <div id="map"></div>
        <div class="pop-little-win"
             :style="{top:popWinPosMap['福州市'][1] - 85 + 'px',left:popWinPosMap['福州市'][0] + 'px'}">
            <p>454584</p>
            <p>WCI</p>
        </div>
        <div class="pop-big-win"
             :style="{top:popWinPosMap['福州市'][1] - 85 + 'px',left:popWinPosMap['福州市'][0] - 200 + 'px'}">
            <img>
            <p><span>微信号：</span><span>454584</span></p>
            <p><span>WCI：</span><span>454584</span></p>
            <p><span>粉丝数：</span><span>454584</span></p>
            <p><span>阅读数：</span><span>454584</span></p>
            <p><span>在看数：</span><span>454584</span></p>
            <p><span>所属城市：</span><span>杭州</span></p>
            <p><span>所属标签：</span><span>医疗</span></p>
        </div>
    </div>

</template>

<script>
    const echarts = require('echarts');
    require('echarts-gl');
    // require('echarts/map/js/province/fujian')
    require('../fujian')
    import $ from 'jquery'

    export default {
        name: "Map",
        data() {
            return {
                mapObj:{}, // 地图实例
                currentArea: '福州市', // 当前选中的城市
                popWinPosMap: { // 弹出窗位置设置
                    '福州市': [791, 222],
                    '厦门市': [367, 274],
                    '泉州市': [481, 237],
                    '漳州市': [255, 205],
                    '龙岩市': [380, 80],
                    '三明市': [610, 47],
                    '南平市': [755, 41],
                    '莆田市': [648, 251],
                    '宁德市': [924, 155]
                },
                cityList: ['福州市', '厦门市', '泉州市', '漳州市', '龙岩市', '三明市', '南平市', '莆田市', '宁德市'], // 城市列表
                currentCityIndex:1, // 当前选中的城市序号
                mapRollTimer:'' // 地图区域选择定时器
            }
        },
        methods:{
            /**
             * 切换区域方法
             * **/
          selectedCurrentArea(){
              this.currentArea = this.cityList[this.currentCityIndex]
              this.mapObj.setOption({ // 添加区域选中颜色
                  geo3D: {
                      regions: [
                          {
                              name: this.currentArea,		// 所对应的地图区域的名称
                              regionHeight: 20,		// 区域的高度，可以设置不同的高度用来表达数据的大小。当 GeoJSON 为建筑的数据时，也可以通过这个值表示简直的高度。
                              itemStyle: {			// 单个区域的样式设置
                                  color: '#0076ff',
                                  // borderColor:'#4c6fff',
                                  opacity: 1,
                              },
                          }
                      ]
                  }
              })
              $('.pop-little-win').css({
                  'left': this.popWinPosMap[this.currentArea][0],
                  'top': this.popWinPosMap[this.currentArea][1] - 85
              })
              this.$emit('freshData',{currentArea:this.currentArea,currentCityIndex:this.currentCityIndex}) // 传递事件给父组件刷新视图
              this.currentCityIndex++;
              if(this.currentCityIndex === this.cityList.length){
                  this.currentCityIndex = 0
              }
          }
        },
        mounted() {
            this.mapObj = echarts.init(document.getElementById('map'));
            this.mapObj.setOption({
                // backgroundColor: '#000129',//canvas的背景颜色
                environment: 'auto',
                geo3D: { //地图的具体参数
                    map: '福建', //地图范围
                    left: 0,
                    boxHeight: 20,
                    label: {
                        show: true,
                        textStyle: {                // 标签的字体样式
                            color: '#fff',                  // 地图初始化区域字体颜色
                            fontSize: 14,                    // 字体大小
                            opacity: 1,                     // 字体透明度
                            backgroundColor: 'rgba(0,23,11,0)'      // 字体背景色
                        },
                    },
                    shading: 'color',//'realistic', //光照带来的明暗
                    groundPlane: {
                        show: false,
                        color: '#0a16a6'
                    },
                    regionHeight: 5,
                    /*light: { // 光照相关的设置。在 shading 为 'color' 的时候无效。
                        main: { //场景主光源的设置
                            intensity: 2,//主光源的强度
                            shadow: true,//主光源是否投射阴影
                            shadowQuality: 'high',//阴影的质量
                            alpha: 30, //主光源绕 x 轴偏离的角度
                            beta:0 //主光源绕 y 轴偏离的角度
                        },
                        ambient: { //全局的环境光设置。
                            intensity: 0.5//环境光的强度
                        }
                    },*/
                    viewControl: {//用于鼠标的旋转，缩放等视角控制
                        distance: 68,//默认视角距离主体的距离
                        alpha: 50, // 让canvas在x轴有一定的倾斜角度
                        beta: 50,
                        center: [0, 0, 10],
                        animation: true,
                        rotateSensitivity: 0, // 禁止转动
                        maxDistance: 70, //最大的值 （默认400）
                        minDistance: 70,
                        orthographicSize: 110, //控制地图大小
                        maxOrthographicSize: 110,
                        minOrthographicSize: 110,
                    },
                    /*postEffect: {//为画面添加高光，景深，环境光遮蔽（SSAO），调色等效果
                        enable: true, //是否开启
                        SSAO: {//环境光遮蔽
                            radius: 100,//环境光遮蔽的采样半径。半径越大效果越自然
                            intensity: 1,//环境光遮蔽的强度
                            enable: false
                        }
                    },
                    temporalSuperSampling: {//分帧超采样。在开启 postEffect 后，WebGL 默认的 MSAA 会无法使用,分帧超采样用来解决锯齿的问题
                        enable: true
                    },*/
                    /*postEffect: {
                        enable: true,
                    },*/
                    light: {
                        main: {
                            intensity: 1,
                            shadow: true
                        },
                        ambient: {
                            intensity: 2,
                            quality: 'high'
                        },
                        ambientCubemap: {
                            exposure: 1.0,
                            diffuseIntensity: 2,
                            specularIntensity: 2
                        }
                    },
                    itemStyle: {//三维图形的视觉属性
                        color: '#0016ba',
                        borderWidth: 1,
                        borderColor: '#00151d',//'#4c6fff',//'#4c6fff',
                        opacity: 0.5,
                    },
                    regions: [{
                        name: '福州市',		// 所对应的地图区域的名称
                        regionHeight: 20,		// 区域的高度，可以设置不同的高度用来表达数据的大小。当 GeoJSON 为建筑的数据时，也可以通过这个值表示简直的高度。
                        itemStyle: {			// 单个区域的样式设置
                            color: '#0076ff',
                            // borderColor:'#4c6fff',
                            opacity: 1,
                        },
                    }],
                    emphasis: {
                        itemStyle: {
                            opacity: 1,
                            color: '#0076ff',
                        },
                        label: {
                            formatter: (params) => {
                                console.log(params)
                                $('.pop-big-win').hide() // 鼠标离开柱子进入地图区域则隐藏大弹窗
                                if(params.name !== this.currentArea){ // 鼠标选择的区域与之前不同则切换视图
                                    this.currentCityIndex = this.cityList.indexOf(params.name) // 获取城市序号
                                    this.selectedCurrentArea()
                                }
                                return params.name;
                            },
                        }
                    },
                    tooltip: {
                        show: true,
                        trigger: 'axis'
                    },
                },
                /*visualMap: {
                    max: 40,
                    calculable: true,
                    realtime: false,
                    inRange: {
                        color: ['#313695', '#4575b4', '#74add1', '#abd9e9', '#e0f3f8', '#ffffbf', '#fee090', '#fdae61', '#f46d43', '#d73027', '#a50026']
                    },
                    outOfRange: {
                        colorAlpha: 0
                    }
                },*/
                series: [{
                    type: 'bar3D',
                    coordinateSystem: 'geo3D',
                    shading: 'realistic',
                    data: [
                        [119.306239, 26.075302, 50], // 福州市
                        [118.11022, 24.490474, 100], // 厦门市
                        [118.589421, 24.908853, 50], // 泉州市
                        [117.561801, 24.310897, 50], // 漳州市
                        [117.02978, 25.091603, 100], // 龙岩市
                        [117.635001, 26.265444, 50], // 三明市
                        [118.178459, 26.935627, 50], // 南平市
                        [119.007558, 25.431011, 100], // 莆田市
                        [119.527082, 26.88924, 100]   // 宁德市
                    ],
                    barSize: 2,
                    bevelSize: 1,
                    bevelSmoothness: 10,
                    realisticMaterial: {
                        roughness: 0
                    },
                    minHeight: 10,
                    silent: false,
                    emphasis: {
                        label: {
                            formatter: (params) => {
                                console.log(params)
                                $('.pop-big-win').css({
                                    'left': this.popWinPosMap[this.cityList[params.dataIndex]][0] - 250,
                                    'top': this.popWinPosMap[this.cityList[params.dataIndex]][1] - 100
                                }).show()
                                return '';
                            },
                        }
                    },
                    itemStyle: {
                        color: (params) => {
                            const regionIndexMap = {
                                '福州市': 0,
                                '厦门市': 1,
                                '泉州市': 2,
                                '漳州市': 3,
                                '龙岩市': 4,
                                '三明市': 5,
                                '南平市': 6,
                                '莆田市': 7,
                                '宁德市': 8
                            }
                            const colorList = ['#554c17', '#0016ba']
                            if (params.dataIndex === regionIndexMap[this.currentArea]) {
                                return colorList[0]
                            } else {
                                return colorList[1]
                            }
                        }
                    }
                }/*,{
                    type:'scatter3D',
                    coordinateSystem: 'geo3D',
                    symbol:"path://'path://M874.666667 469.333333c17.28 42.666667 42.666667 85.333333 42.666666 128v213.333334c0 27.946667-23.893333 42.666667-64 42.666666s-64-17.28-64-42.666666v-42.666667H234.666667v42.666667c0 25.386667-23.893333 42.666667-64 42.666666s-64-14.72-64-42.666666V597.333333c0-42.666667 25.386667-85.333333 42.666666-128s-64-37.333333-64-64 5.333333-42.666667 42.666667-42.666666h64s30.293333-87.04 42.666667-128 64-64 106.666666-64h341.333334c42.666667 0 94.293333 23.04 106.666666 64s42.666667 128 42.666667 128h64c37.333333 0 42.666667 16 42.666667 42.666666s-81.28 21.333333-64 64z m-469.333334 149.333334a21.333333 21.333333 0 0 0 21.333334 21.333333h170.666666a21.333333 21.333333 0 0 0 21.333334-21.333333v-21.333334a21.333333 21.333333 0 0 0-21.333334-21.333333h-170.666666a21.333333 21.333333 0 0 0-21.333334 21.333333v21.333334zM192 618.666667a64 64 0 1 0 64-64 64 64 0 0 0-64 64z m597.333333-256c0-21.333333-42.666667-106.666667-42.666666-106.666667H277.333333s-42.666667 85.333333-42.666666 106.666667v21.333333a42.666667 42.666667 0 0 0 42.666666 42.666667h469.333334a42.666667 42.666667 0 0 0 42.666666-42.666667v-21.333333z m-21.333333 192a64 64 0 1 0 64 64 64 64 0 0 0-64-64z';\n",
                    //"image://33.svg",//"image://11.png",
                    symbolSize: 60,
                    data: [
                        [119.306239,26.075302, 4], [118.11022,24.490474, 4]
                    ],
                    itemStyle: {
                        color: '#fff800',
                        opacity: 0.8
                    },
                    /!*animation:true,
                    animationDelay: function(idx) {
                        // 越往后的数据延迟越大
                        return idx * 1000;
                    }*!/

                }*/]
            });
            this.mapObj.getZr().on('click', (params) => { // 地图点击事件
                /*if (this.currentArea && $('#map').children()[0].style.cursor === 'pointer') {

                }*/
            });
            let rollSwitch = false // 是否滚动地图区域
            setInterval(()=>{ // 监听地图是否被鼠标悬停
                if($('#map').children()[0].style.cursor === 'pointer'){ // 鼠标在地图上则取消自动切换区域
                    if(rollSwitch){
                        clearInterval(this.mapRollTimer)
                        rollSwitch = false
                    }
                } else {
                    if(!rollSwitch){
                        rollSwitch = true
                        this.mapRollTimer = setInterval(() => { // 自动切换区域
                            this.selectedCurrentArea()
                        }, 2000)
                    }
                }
            },50)
        }
    }
</script>

<style scoped lang="scss">
    .map-area {
        position: relative;

        #map {
            width: 1087px;
            height: 458px;
            margin: 50px auto auto auto;
        }

        .pop-little-win {
            width: 135.5px;
            height: 85.5px;
            position: absolute;
            background-image: url("../assets/map-little-pop-win@2x.png");
            background-size: 100% 100%;
            background-image: -webkit-image-set(
                            url("../assets/map-little-pop-win.png") 1x,
                            url("../assets/map-little-pop-win@2x.png") 2x);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;

            > p:first-child {
                color: #FFDE00;
                font-size: 23px;
                font-weight: bold;
            }

            > p:last-child {
                color: #E7E7E7;
                font-size: 20px;
            }
        }
        .pop-big-win{
            width: 215.5px;
            height: 329px;
            display: none;
            position: absolute;
            z-index: 2;
            background-image: url("../assets/map-big-pop-win@2x.png");
            background-size: 100% 100%;
            background-image: -webkit-image-set(
                            url("../assets/map-big-pop-win.png") 1x,
                            url("../assets/map-big-pop-win@2x.png") 2x);
            padding: 20.5px;
            > img{
                width: 70px;
                height: 70px;
                border-radius: 100%;
                margin-bottom: 20px;
            }
            > p{
                height: 28px;
                & span:first-child{
                    color:#E7E7E7;
                    font-size: 17px;
                }
                & span:last-child{
                    color:#00E5E0;
                    font-size: 17px;
                }
            }
        }
    }

</style>

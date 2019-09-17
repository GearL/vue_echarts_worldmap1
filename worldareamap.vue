<template>
    <div id="home-world-map"></div>
</template>

<script>
    import echarts from 'echarts'
    import world from '../../assets/js/world'
    import {mapState} from "vuex";

    export default {
        name: "home-world-map",
        props: {
            worldData: {
                type: Array,
                default: function () {
                    return []
                }
            },
            setSelectedOptions: {
                type: Function,
            },
            title:{
                type:String,
                default:""
            }
        },
        computed: {
            ...mapState([
                "countryList",
                "optionCountryList"
            ])
        },
        watch: {
            worldData: {
                handler(newval, oldval) {
                    if (newval.length > 0) {
                        // console.log("______");
                        // console.log(newval.filter(item=>item.value==561173));
                        //this.maxValue = newval[Math.floor(newval.length / 2) - Math.floor(newval.length / 4) - Math.floor(newval.length / 8) - Math.floor(newval.length / 16)].value
                        this.maxValue = newval[0].value
                    }
                    this.drawLine()
                },
                deep: true
            },
            title(newval,oldval){
                this.drawLine()
            }
        },
        data() {
            return {
                maxValue: 1000,
                timerId: 0
            }
        },
        mounted() {
            //this.drawLine()
        },
        methods: {
            queryCountryIdAndContinentIdByCountryName(countryName) {

                let arr = []
                this.countryList.forEach(item => {
                    item.countrys.forEach(item1 => {
                        if (item1.otherName == countryName) {
                            arr[1] = item1.id
                            arr[0] = item.id
                        }
                    })

                })
                return arr
            },
            handleResize() {
                echarts.init(document.getElementById('home-world-map')).resize()
            },
            async drawLine() {
                var _this = this;
                var chart = echarts.init(document.getElementById("home-world-map"));
                var option = {
                    title: {
                        text: this.title,
                        textStyle: {
                            color: '#79d9f1',
                            fontSize: '20'
                        },
                        left: '8%'
                    },
                    backgroundColor: new echarts.graphic.RadialGradient(0.5, 0.5, 0.4, [{
                        offset: 0,
                        color: '#4b5769'
                    }, {
                        offset: 1,
                        color: 'rgba(24, 24, 64, 0.2)'
                    }]),
                    // visualMap: {
                    //   type: 'continuous',
                    //   orient: 'horizontal',
                    //   min:0,
                    //   // max: this.maxNum,
                    //   max: this.maxValue,
                    //   bottom: 12,
                    //   right: 10,
                    //   itemWidth: 20,                           //图形的宽度，即长条的宽度。
                    //   itemHeight: 280,                         //图形的高度，即长条的高度。
                    //   text: ['高', '低'],
                    //   textStyle: {
                    //     fontSize: 14,
                    //     color: '#FFF'
                    //   },
                    //
                    //   realtime: false,
                    //   calculable: true,
                    //   // splitNumber: 5,
                    //   color: ['#0022ed','blue', 'deepskyblue', 'lightskyblue','white']
                    // },
                    visualMap: {
                        show: true,                          //是否显示 visualMap-continuous 组件。如果设置为 false，不会显示，但是数据映射的功能还存在
                        type: 'piecewise',                  // 定义为分段型 visualMap
                        // splitNumber: 15,                      //对于连续型数据，自动平均切分成几段。默认为5段
                        pieces: [                           //自定义『分段式视觉映射组件（visualMapPiecewise）』的每一段的范围，以及每一段的文字，以及每一段的特别的样式
                            {min: this.maxValue + 1, max: this.maxValue + 10000000, color: 'blue'},
                            {min: 700001, max: this.maxValue, color: '#2586E3'},
                            {min: 500001, max: 700000, color: '#3892E6'},
                            {min: 300001, max: 500000, color: '#4B9EE8'},
                            {min: 100001, max: 300000, color: '#5DAAEB'},
                            {min: 20001, max: 100000, color: '#70B7EE'},
                            {min: 10001, max: 20000, color: '#83C3F1'},
                            {min: 5001, max: 10000, color: '#95CCF4'},
                            {min: 1001, max: 5000, color: '#A8DBF7'},
                            {min: 501, max: 1000, color: '#BBE7F9'},
                            {min: 101, max: 500, color: '#CDF3FC'},
                            {min: 0, max: 100, color: 'white'},
                            //-----------------------------------------------------
                            //  {min: this.maxValue+1,max: this.maxValue+10000000,color:'#8739B0'},
                            // {min: 700001,max: this.maxValue,color:'#9A3595'},
                            // {min: 500001,max: 700000,color:'#AE2F7E'},
                            // {min: 300001,max: 500000,color:'#BF2C69'},
                            // {min: 100001,max: 300000,color:'#D42752'},
                            // {min: 20001,max: 100000,color:'#EB2137'},
                            // {min: 10001,max: 20000,color:'#FC211D'},
                            // {min: 5001,max: 10000,color:'#FE3B27'},
                            // {min: 1001,max: 5000,color:'#FC552B'},
                            // {min: 501,max: 1000,color:'#FE743B'},
                            // {min: 101,max: 500,color:'#FE943C'},
                            // {min: 0,max: 100,color:'#FDAC43'},
                            //-----------------------------------------------------
                            //  {min: this.maxValue+1,max: this.maxValue+10000000,color:'#4076A1'},
                            // {min: 700001,max: this.maxValue,color:'#3586C3'},
                            // {min: 500001,max: 700000,color:'#208DE1'},
                            // {min: 300001,max: 500000,color:'#299EC9'},
                            // {min: 100001,max: 300000,color:'#39A890'},
                            // {min: 20001,max: 100000,color:'#49B367'},
                            // {min: 10001,max: 20000,color:'#5BC03C'},
                            // {min: 5001,max: 10000,color:'#81D21A'},
                            // {min: 1001,max: 5000,color:'#AED22C'},
                            // {min: 501,max: 1000,color:'#DFD33F'},
                            // {min: 101,max: 500,color:'#ECb63D'},
                            // {min: 0,max: 100,color:'#C91914'},
                            //-----------------------------------------------------
                            //  {min: this.maxValue,max: this.maxValue+10000000,color:'#C91914'},
                            // {min: 700001,max: this.maxValue-1,color:'#ECb63D'},
                            // {min: 500001,max: 700000,color:'#DFD33F'},
                            // {min: 300001,max: 500000,color:'#AED22C'},
                            // {min: 100001,max: 300000,color:'#81D21A'},
                            // {min: 20001,max: 100000,color:'#5BC03C'},
                            // {min: 10001,max: 20000,color:'#49B367'},
                            // {min: 5001,max: 10000,color:'#39A890'},
                            // {min: 1001,max: 5000,color:'#299EC9'},
                            // {min: 501,max: 1000,color:'#208DE1'},
                            // {min: 101,max: 500,color:'#3586C3'},
                            // {min: 0,max: 100,color:'#4076A1'},
                            //-----------------------------------------------------
                            //  {min: this.maxValue,max: this.maxValue+10000000,color:'#9A3A6E'},
                            // {min: 700001,max: this.maxValue-1,color:'#CE3B58'},
                            // {min: 500001,max: 700000,color:'#F33B48'},
                            // {min: 300001,max: 500000,color:'#f7c978'},
                            // {min: 100001,max: 300000,color:'#f3a469'},
                            // {min: 20001,max: 100000,color:'#f18271'},
                            // {min: 10001,max: 20000,color:'#cc6b8e'},
                            // {min: 5001,max: 10000,color:'#a86aa4'},
                            // {min: 1001,max: 5000,color:'#8f6aae'},
                            // {min: 501,max: 1000,color:'#7b5fac'},
                            // {min: 101,max: 500,color:'#5a55ae'},
                            // {min: 0,max: 100,color:'#3f51b1'},
                            //-----------------------------------------------------
                            //  {min: this.maxValue,max: this.maxValue+10000000,color:'#9ED110'},
                            // {min: 700001,max: this.maxValue-1,color:'#50B517'},
                            // {min: 500001,max: 700000,color:'#179067'},
                            // {min: 300001,max: 500000,color:'#476EAF'},
                            // {min: 100001,max: 300000,color:'#9f49ac'},
                            // {min: 20001,max: 100000,color:'#CC42A2'},
                            // {min: 10001,max: 20000,color:'#FF3BA7'},
                            // {min: 5001,max: 10000,color:'#FF5800'},
                            // {min: 1001,max: 5000,color:'#FF8100'},
                            // {min: 501,max: 1000,color:'#FEAC00'},
                            // {min: 101,max: 500,color:'#FFCC00'},
                            // {min: 0,max: 100,color:'#EDE604'},

                        ],
                        textStyle: {
                            color: 'white'
                        }
                    },
                    tooltip: {
                        trigger: 'item',

                    },

                    toolbox: {
                        //right: 60,
                        show: true,
                        feature: {
                            dataView: {
                                lang: ['DataView','Close'],
                                title:"DataView",
                                readOnly: true,
                                textColor: '#fff',
                                backgroundColor: '#09132B',
                                buttonColor: '#fff',
                                buttonTextColor: '#000',
                                iconStyle:{
                                    color:'#409EFF',
                                    borderColor:'#245632'
                                },
                                optionToContent: function (opt) {
                                    let tdHeads = '<td  style="padding: 0 10px ;font-Size:40px">country</td><td  style="padding: 0 10px;font-Size:40px">value</td>'; //表头
                                    let tdBodys = ''; //数据
                                    let data = opt.series[0].data
                                    let table = `<table width='100%' border="1" style="border-collapse:collapse;text-align:center;color:#ccc; border:solid"><tbody><tr>${tdHeads} </tr>`;
                                    data.forEach(element => {
                                        tdBodys = `<td style="border:solid 1px">${element.otherName}</td><td style="border:solid 1px">${element.value}</td>`
                                        table += `<tr>${tdBodys}</tr>`
                                    });
                                    table += '</tbody></table>';
                                    return table;
                                }
                            },
                        }
                    },
                    series: [
                        {
                            name: '',
                            type: 'map',
                            mapType: 'world',
                            itemStyle: {
                                normal: {
                                    borderWidth: 0.6,
                                    borderColor: '#505a69',
                                    areaColor: 'rgba(24, 24, 24, 0.0)'
                                },
                                // normal:{label:{show:true}},		//默认是否显示国家名
                                emphasis: {label: {show: true}},	//鼠标移动显示国家名
                            },
                            roam: true,
                            data: this.worldData,
                            selectedMode:'single'
                        }
                    ],

                };
                //清空画布
                //chart.clear();
                // 使用刚指定的配置项和数据显示图表。
                chart.setOption(option);
                //在渲染点击事件之前先清除点击事件
                chart.off('click');
                //设置点击事件
                chart.on('click', params => {
                    if(params.name)
                    {
                        let arr = this.queryCountryIdAndContinentIdByCountryName(params.name)
                        this.setSelectedOptions(arr)
                    }
                });
                // 屏幕自适应
                this.timerId = setTimeout(function () {
                    window.addEventListener('resize', _this.handleResize)
                }, 200)
            },
        },
        beforeDestroy() {
            clearTimeout(this.timerId)
            window.removeEventListener('resize', this.handleResize)
        }
    }
</script>

<style scoped>
    #home-world-map {
        width: 100%;
        height: 100%;
    }
</style>
<style>
    /*#home-world-map > div:nth-of-type(3)>div:nth-of-type(2){*/
    /*z-index: 20;*/
    /*}*/
</style>

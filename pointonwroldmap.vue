<template>
    <div id="world-page">
        <div id="world-map"></div>
        <Slider id="slider" v-model="zoom" :step="0.1" :max="15" :min="1" show-input input-size="small"></Slider>
    </div>
</template>

<script>
    import echarts from 'echarts'
    import 'echarts/map/js/world'

    export default {
        name: 'worldMap',
        props: {
            initData: {
                type: Array
            }
        },
        data () {
            return {
                myChart: null,
                option: {},
                zoom: 1
            }
        },
        methods: {
            initMap: function () {
                let map = echarts.init(document.getElementById('world-map'))
                this.myChart = map
                let options = {
                    tooltip: {
                        trigger: 'item',
                        formatter: function (params) {
                            if (params.value[0] === 0 || params.value[1] === 0) {
                                return ''
                            }
                            return params.data.country + '<br>' + params.data.name
                        }
                    },
                    geo: {
                        map: 'world',
                        label: {
                            emphasis: {
                                show: false
                            }
                        },
                        zlevel: 0,
                        z: 0,
                        center: [0, 0],
                        zoom: 1,
                        itemStyle: {
                            normal: {
                                areaColor: '#eee',
                                borderColor: '#404a59'
                            }
                        },
                        roam: true
                    },
                    series: [
                        {
                            name: '疫情数量',
                            type: 'scatter',
                            showEffectOn: 'render',
                            symbolSize: function (val) {
                                return val[2] / 5
                            },
                            coordinateSystem: 'geo',
                            data: this.initData,
                            label: {
                                normal: {
                                    show: false
                                },
                                emphasis: {
                                    show: false
                                }
                            },
                            itemStyle: {
                                emphasis: {
                                    borderColor: '#fff',
                                    borderWidth: 2
                                }
                            }
                        }
                    ]
                }
                this.option = options
                map.setOption(options)
                map.off('click')
                map.on('click', (param) => {
                    if (param.componentType === 'series') {
                        if (param.value[0] === 0 || param.value[1] === 0) {
                            return false
                        }
                        let index = param.data.index
                        this.$emit('clickPointIndex', index)
                    }
                })
                window.addEventListener('resize', function () {
                    map.resize()
                })
            },
            selectValue: function (center, zoom, index) {
                if (center[0] !== 0 || center[1] !== 0) {
                    this.option.geo.center = center
                    this.option.geo.zoom = zoom
                }
                if (this.option.series[0].data[index].itemStyle.color === 'rgba(0,0,0,0)') {

                } else if (this.option.series[0].data[index].itemStyle.color === '#8BC34A') {
                    this.option.series[0].data[index].itemStyle.color = '#C23531'
                    this.option.series[0].data[index].value[2] = 50
                } else {
                    this.option.series[0].data[index].itemStyle.color = '#8BC34A'
                    this.option.series[0].data[index].value[2] = 100
                }
                this.myChart.setOption(this.option)
            },
            changeZoom: function (zoom) {
                this.option = this.myChart.getOption()
                this.option.geo[0].zoom = zoom
                this.myChart.setOption(this.option)
            },
            showLoading: function (obj) {
                this.myChart.showLoading(obj)
            },
            hideLoading: function () {
                this.myChart.hideLoading()
            }
        },
        mounted () {
            this.initMap()
        },
        watch: {
            initData: {
                handler: function () {
                    this.initMap()
                }
            },
            zoom: {
                handler: function (newval) {
                    this.changeZoom(newval)
                }
            }
        }
    }
</script>

<style scoped>
    #world-map {
        width: 100%;
        height: 100%;
    }
    #slider{
        position: relative;
        bottom: 10%;
        width: 40%;
        margin: auto;
    }
</style>

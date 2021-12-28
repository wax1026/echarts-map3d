<template>
  <div class="middle-top-map" id="middleTopMap"></div>
</template>

<script>
import * as echarts from 'echarts'
import 'echarts-gl' // echarts 3D插件

const xcMapData = require('../../assets/xcMapData.json')
const pointData = require('../../assets/xcMapDataPoint.json')

const pointList = []
pointData.features.forEach((item) => {
  pointList.push({
    value: [...item.geometry.coordinates, Math.ceil(Math.random() * 10)],
    name: item.properties.name
  })
})

export default {
  name: 'MiddleTopMap',
  data () {
    return {
      config: {}
    }
  },
  mounted () {
    echarts.registerMap('xiangcheng', xcMapData)
    const myCharts = echarts.init(document.getElementById('middleTopMap'))
    const data = pointList
    myCharts.setOption({
      backgroundColor: 'rgba(0,0,0,0)',
      title: {
        // 标题
        top: '5%',
        text: '3D地图',
        subtext: '',
        x: 'center',
        textStyle: {
          color: 'red',
          fontSize: 20
        }
      },

      tooltip: {
        // 柱形图提示框
        trigger: 'item',
        showDelay: 0,
        transitionDuration: 0.2,
        formatter: function (params) {
          return params.name
        }
      },
      geo3D: {
        map: 'xiangcheng',
        shading: 'lambert',

        light: {
          main: {
            // 场景主光源的设置，在 globe 组件中就是太阳光。
            color: '#fff', // 主光源的颜色。[ default: #fff ]
            intensity: 1.2, // 主光源的强度。[ default: 1 ]
            shadow: true, // 主光源是否投射阴影。默认关闭。    开启阴影可以给场景带来更真实和有层次的光照效果。但是同时也会增加程序的运行开销。
            shadowQuality: 'high', // 阴影的质量。可选'low', 'medium', 'high', 'ultra' [ default: 'medium' ]
            alpha: 55, // 主光源绕 x 轴，即上下旋转的角度。配合 beta 控制光源的方向。[ default: 40 ]
            beta: 10 // 主光源绕 y 轴，即左右旋转的角度。[ default: 40 ]
          },
          ambient: {
            // 全局的环境光设置。
            color: 'rgba(255,255,255, 0)', // 环境光的颜色。[ default: #fff ]
            intensity: 0.5 // 环境光的强度。[ default: 0.2 ]
          }
          // main: {
          //   intensity: 5,
          //   shadow: true,
          //   shadowQuality: 'high',
          //   alpha: 30
          // },
          // ambient: {
          //   intensity: 0
          // },
          // ambientCubemap: {
          //   texture: 'data-gl/asset/canyon.hdr',
          //   exposure: 1,
          //   diffuseIntensity: 0.5
          // }
        },
        viewControl: {
          // 用于鼠标的旋转，缩放等视角控制。
          projection: 'perspective', // 投影方式，默认为透视投影'perspective'，也支持设置为正交投影'orthographic'。
          autoRotate: true, // 是否开启视角绕物体的自动旋转查看。[ default: false ]
          autoRotateDirection: 'cw', // 物体自传的方向。默认是 'cw' 也就是从上往下看是顺时针方向，也可以取 'ccw'，既从上往下看为逆时针方向。
          autoRotateSpeed: 10, // 物体自传的速度。单位为角度 / 秒，默认为10 ，也就是36秒转一圈。
          autoRotateAfterStill: 3, // 在鼠标静止操作后恢复自动旋转的时间间隔。在开启 autoRotate 后有效。[ default: 3 ]
          damping: 0, // 鼠标进行旋转，缩放等操作时的迟滞因子，在大于等于 1 的时候鼠标在停止操作后，视角仍会因为一定的惯性继续运动（旋转和缩放）。[ default: 0.8 ]
          rotateSensitivity: 1, // 旋转操作的灵敏度，值越大越灵敏。支持使用数组分别设置横向和纵向的旋转灵敏度。默认为1, 设置为0后无法旋转。   rotateSensitivity: [1, 0]——只能横向旋转； rotateSensitivity: [0, 1]——只能纵向旋转。
          zoomSensitivity: 1, // 缩放操作的灵敏度，值越大越灵敏。默认为1,设置为0后无法缩放。
          panSensitivity: 1, // 平移操作的灵敏度，值越大越灵敏。默认为1,设置为0后无法平移。支持使用数组分别设置横向和纵向的平移灵敏度
          panMouseButton: 'left', // 平移操作使用的鼠标按键，支持：'left' 鼠标左键（默认）;'middle' 鼠标中键 ;'right' 鼠标右键(注意：如果设置为鼠标右键则会阻止默认的右键菜单。)
          rotateMouseButton: 'right', // 旋转操作使用的鼠标按键，支持：'left' 鼠标左键;'middle' 鼠标中键（默认）;'right' 鼠标右键(注意：如果设置为鼠标右键则会阻止默认的右键菜单。)

          distance: 80, // [ default: 100 ] 默认视角距离主体的距离，对于 grid3D 和 geo3D 等其它组件来说是距离中心原点的距离,对于 globe 来说是距离地球表面的距离。在 projection 为'perspective'的时候有效。
          // distance: 200, // [ default: 100 ] 默认视角距离主体的距离，对于 grid3D 和 geo3D 等其它组件来说是距离中心原点的距离,对于 globe 来说是距离地球表面的距离。在 projection 为'perspective'的时候有效。
          minDistance: 40, // [ default: 40 ] 视角通过鼠标控制能拉近到主体的最小距离。在 projection 为'perspective'的时候有效。
          maxDistance: 400, // [ default: 400 ] 视角通过鼠标控制能拉远到主体的最大距离。在 projection 为'perspective'的时候有效。

          alpha: 40, // 视角绕 x 轴，即上下旋转的角度。配合 beta 可以控制视角的方向。[ default: 40 ]
          beta: 15, // 视角绕 y 轴，即左右旋转的角度。[ default: 0 ]
          minAlpha: -360, // 上下旋转的最小 alpha 值。即视角能旋转到达最上面的角度。[ default: 5 ]
          maxAlpha: 360, // 上下旋转的最大 alpha 值。即视角能旋转到达最下面的角度。[ default: 90 ]
          minBeta: -360, // 左右旋转的最小 beta 值。即视角能旋转到达最左的角度。[ default: -80 ]
          maxBeta: 360, // 左右旋转的最大 beta 值。即视角能旋转到达最右的角度。[ default: 80 ]

          center: [0, 0, 0], // 视角中心点，旋转也会围绕这个中心点旋转，默认为[0,0,0]。

          animation: true, // 是否开启动画。[ default: true ]
          animationDurationUpdate: 1000, // 过渡动画的时长。[ default: 1000 ]
          animationEasingUpdate: 'cubicInOut' // 过渡动画的缓动效果。[ default: cubicInOut ]
        },
        groundPlane: {
          // 大地的颜色
          show: false,
          color: 'rgba(255,255,255, 0)'
        },
        postEffect: {
          enable: true,
          bloom: {
            enable: false
          },
          SSAO: {
            radius: 1,
            intensity: 1,
            enable: true
          },
          depthOfField: {
            enable: false,
            focalRange: 10,
            blurRadius: 10,
            fstop: 1
          }
        },
        temporalSuperSampling: {
          enable: true
        },
        itemStyle: {
          // 三维地理坐标系组件 中三维图形的视觉属性，包括颜色，透明度，描边等。
          color: 'rgba(95,158,160,0.5)', // 地图板块的颜色
          opacity: 1, // 图形的不透明度 [ default: 1 ]
          borderWidth: 0.5, // (地图板块间的分隔线)图形描边的宽度。加上描边后可以更清晰的区分每个区域   [ default: 0 ]
          borderColor: '#000' // 图形描边的颜色。[ default: #333 ]
        },
        regionHeight: 2,
        label: {
          show: true
        },
        emphasis: {
          // 当鼠标放上去的状态
          label: {
            show: true,
            color: '#e0f3f8'
          },
          itemStyle: {
            color: 'rgba(28, 134, 238, 0.5)'
          }
        },

        formatter: (val) => {
          return val
        }
      },
      visualMap: {
        max: 40,
        calculable: true,
        realtime: false,
        inRange: {
          color: [
            '#313695',
            '#4575b4',
            '#74add1',
            '#abd9e9',
            '#e0f3f8',
            '#ffffbf',
            '#fee090',
            '#fdae61',
            '#f46d43',
            '#d73027',
            '#a50026'
          ]
        },
        outOfRange: {
          colorAlpha: 0
        }
      },
      series: [
        {
          type: 'bar3D',
          coordinateSystem: 'geo3D',
          shading: 'lambert',
          data: data,
          barSize: 1,
          minHeight: 0.2,
          silent: false, // 图形是否不响应和触发鼠标事件，默认为 false，即响应和触发鼠标事件
          itemStyle: {
            color: 'orange'
            // opacity: 0.8
          },
          label: {
            show: true,
            fontSize: 12,
            distance: 0.5,
            color: '#000',
            fontWeight: 'bold',
            formatter: (info) => {
              // console.log('info', info)
              return info.name
            }
          },
          emphasis: {
            itemStyle: {
              color: 'red'
            },
            label: {
              show: true,
              color: 'yellow',
              fontSize: 20
            }
          },
          animation: 'true',
          animationEasingUpdate: 'cubicOut'
        }
      ]
    })
  }
}
</script>

<style lang="less" scoped>
.middle-top-map {
  width: 100%;
  height: 100%;
}
</style>

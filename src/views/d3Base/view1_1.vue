<template>
  <div class="container">
    <svg width="100%" height="100%"></svg>
  </div>
  <button @click="changeSelectButton1()">选择子指标1</button>
  <button @click="changeSelectButton2()">选择子指标2</button>
</template>

<script>
import * as d3 from 'd3'
import roseJson from '../../data/rose/rose.json'
export default {
  name: 'view1_1',
  data () {
    return {
      chartConfig: {
        ringValue: { innerRadius: 48, outerRadius: 52 },
        range: [{ min: 5, max: 48 }, { min: 52, max: 95 }],
        color: [['#0000FF', '#008000'], ['#1E90FF', '#00FF00'], ['#87CEEB', '#90EE90']]
      },
      chartValue: {
        selectButton: [0, 0, 0, 0, 0, 0, 0]
      }
    }
  },
  methods: {
    changeSelectButton1 () {
      this.chartValue.selectButton[3] = 1
      this.createRoseChart()
    },
    changeSelectButton2 () {
      this.chartValue.selectButton[3] = 2
      this.createRoseChart()
    },
    createRoseChart () {
      const that = this
      const main = d3.select('.container svg')
        .append('g')
        .classed('main', true)
        .attr('transform', 'translate(' + 0 + ',' + 0 + ')')
      // 画圆环
      const roseData = that.getRoseData(roseJson, 'ST国华', '海量数据', that.chartValue.selectButton)
      const roseChartData = that.calRingData(roseData)
      const views = main.append('g')
        .classed('views', true)
      views.selectAll('g')
        .data(roseData)
        .enter()
        .append('g')
        .attr('class', function (d, i) {
          return 'view' + (i + 1)
        })
      for (let i = 0; i < 7; i++) {
        let view = views.select('.view' + (i + 1))
        view = main.append('g').attr('transform', 'translate(' + (200 * i + 100) + ',' + 200 + ')')
        const arc = d3.arc()
          .outerRadius(that.chartConfig.range[0].max)
          .innerRadius(that.chartConfig.range[1].min)
          .startAngle(0)
          .endAngle(Math.PI * 2)
        const ring = view.append('path')
          .attr('d', arc)
        ring.attr('fill', 'red')
        const arcs = view.append('g')
          .classed('arcs', true)
        arcs.selectAll('g')
          .data(roseData[i] + roseData[i])
          .enter()
          .append('g')
          .attr('class', function (d, i) {
            return 'arc' + (i + 1)
          })
        const subArcs = view.append('g')
          .classed('subArcs', true)
        subArcs.selectAll('g')
          .data(roseData[i] + roseData[i])
          .enter()
          .append('g')
          .attr('class', function (d, i) {
            return 'subArc' + (i + 1)
          })
        for (let j = 0; j < 6; j++) {
          let arc1 = arcs.select('.arc' + (2 * j + 1))
          const keys = Object.keys(roseChartData[i][j])
          const key = Object.keys(roseChartData[i][j][keys[0]][0])
          arc1 = d3.arc()
            .outerRadius(roseChartData[i][j][keys[0]][0][key[1]])
            .innerRadius(roseChartData[i][j][keys[0]][0][key[0]])
            .startAngle(j * Math.PI / 3)
            .endAngle(j * Math.PI / 3 + Math.PI / 6)
          const copArc = view.append('path')
            .attr('d', arc1)
          copArc.attr('fill', that.chartConfig.color[0][0])
          let arc2 = arcs.select('.arc' + (2 * j + 2))
          arc2 = d3.arc()
            .outerRadius(roseChartData[i][j][keys[0]][1][key[1]])
            .innerRadius(roseChartData[i][j][keys[0]][1][key[0]])
            .startAngle(j * Math.PI / 3 + Math.PI / 6)
            .endAngle((j + 1) * Math.PI / 3)
          const copCorArc = view.append('path')
            .attr('d', arc2)
          copCorArc.attr('fill', that.chartConfig.color[0][1])
        }
        if (this.chartValue.selectButton[i] === 1) {
          for (let j = 0; j < 6; j++) {
            let arc1 = arcs.select('.subArc' + (2 * j + 1))
            const keys = Object.keys(roseChartData[i][j])
            const key = Object.keys(roseChartData[i][j][keys[1]][0])
            arc1 = d3.arc()
              .outerRadius(roseChartData[i][j][keys[1]][0][key[1]])
              .innerRadius(roseChartData[i][j][keys[1]][0][key[0]])
              .startAngle(j * Math.PI / 3)
              .endAngle(j * Math.PI / 3 + Math.PI / 6)
            const copArc = view.append('path')
              .attr('d', arc1)
            copArc.attr('fill', that.chartConfig.color[1][0])
            let arc2 = arcs.select('.subArc' + (2 * j + 2))
            arc2 = d3.arc()
              .outerRadius(roseChartData[i][j][keys[1]][1][key[1]])
              .innerRadius(roseChartData[i][j][keys[1]][1][key[0]])
              .startAngle(j * Math.PI / 3 + Math.PI / 6)
              .endAngle((j + 1) * Math.PI / 3)
            const copCorArc = view.append('path')
              .attr('d', arc2)
            copCorArc.attr('fill', that.chartConfig.color[1][1])
          }
        } else if (this.chartValue.selectButton[i] === 2) {
          for (let j = 0; j < 6; j++) {
            let arc1 = arcs.select('.subArc' + (2 * j + 1))
            const keys = Object.keys(roseChartData[i][j])
            const key = Object.keys(roseChartData[i][j][keys[2]][0])
            arc1 = d3.arc()
              .outerRadius(roseChartData[i][j][keys[2]][0][key[1]])
              .innerRadius(roseChartData[i][j][keys[2]][0][key[0]])
              .startAngle(j * Math.PI / 3)
              .endAngle(j * Math.PI / 3 + Math.PI / 6)
            const copArc = view.append('path')
              .attr('d', arc1)
            copArc.attr('fill', that.chartConfig.color[2][0])
            let arc2 = arcs.select('.subArc' + (2 * j + 2))
            arc2 = d3.arc()
              .outerRadius(roseChartData[i][j][keys[2]][1][key[1]])
              .innerRadius(roseChartData[i][j][keys[2]][1][key[0]])
              .startAngle(j * Math.PI / 3 + Math.PI / 6)
              .endAngle((j + 1) * Math.PI / 3)
            const copCorArc = view.append('path')
              .attr('d', arc2)
            copCorArc.attr('fill', that.chartConfig.color[2][1])
          }
        }
      }
    },
    getRoseData (file, copName, corCopName, flags) {
      const keys = Object.keys(file) // 每个指标
      const rosesDatas = []
      for (let i = 0; i < keys.length; i++) {
        const roseDatas = [] // 每个指标
        for (let j = 0; j < 6; j++) {
          const roseData = []
          const key = Object.keys(file[keys[i]][j + 4]) // 年份
          for (let k = 0; k < file[keys[i]][j + 4][key[0]].length; k++) {
            const copName = Object.keys(file[keys[i]][j + 4][key[0]][k]) // 公司
            if (k === 0) { // 存在问题，对应cop
              const index = Object.keys(file[keys[i]][j + 4][key[0]][k][copName[0]]) // 主指标和子指标
              roseData.push({
                main: file[keys[i]][j + 4][key[0]][k][copName[0]][index[0]],
                sub1: file[keys[i]][j + 4][key[0]][k][copName[0]][index[1]],
                sub2: file[keys[i]][j + 4][key[0]][k][copName[0]][index[2]]
              })
            } else if (k === 1) { // 存在问题，对应corCop
              const index = Object.keys(file[keys[i]][j + 4][key[0]][k][copName[0]]) // 主指标和子指标
              roseData.push({
                main: file[keys[i]][j + 4][key[0]][k][copName[0]][index[0]],
                sub1: file[keys[i]][j + 4][key[0]][k][copName[0]][index[1]],
                sub2: file[keys[i]][j + 4][key[0]][k][copName[0]][index[2]]
              })
            }
          }
          roseDatas.push(roseData)
        }
        rosesDatas.push(roseDatas)
      }
      return rosesDatas
    },
    calRingData (data) {
      const values = []
      for (let i = 0; i < data.length; i++) {
        const extremeValue = { max: 1, min: -1 }
        for (let j = 0; j < 6; j++) {
          for (let k = 0; k < 2; k++) {
            if (data[i][j][k].main > 0 && data[i][j][k].main > extremeValue.max) {
              extremeValue.max = data[i][j][k].main
            } else if (data[i][j][k].main < 0 && data[i][j][k].main < extremeValue.min) {
              extremeValue.min = data[i][j][k].main
            }
          }
        }
        const value = []
        for (let j = 0; j < 6; j++) {
          const arcRadiusMain = [{ inner: 0, outer: 0 }, { inner: 0, outer: 0 }]
          const arcRadiusSub1 = [{ inner: 0, outer: 0 }, { inner: 0, outer: 0 }]
          const arcRadiusSub2 = [{ inner: 0, outer: 0 }, { inner: 0, outer: 0 }]
          for (let k = 0; k < 2; k++) {
            // main
            if (data[i][j][k].main > 0) {
              arcRadiusMain[k].inner = this.chartConfig.ringValue.outerRadius
              arcRadiusMain[k].outer = data[i][j][k].main / extremeValue.max * (this.chartConfig.range[1].max - this.chartConfig.range[1].min) + this.chartConfig.ringValue.outerRadius
            } else if (data[i][j][k].main < 0) {
              arcRadiusMain[k].inner = this.chartConfig.ringValue.innerRadius - data[i][j][k].main / extremeValue.min * (this.chartConfig.range[0].max - this.chartConfig.range[0].min)
              arcRadiusMain[k].outer = this.chartConfig.ringValue.innerRadius
            }
            // sub1
            if (data[i][j][k].sub1 > 0) {
              arcRadiusSub1[k].inner = this.chartConfig.ringValue.outerRadius
              arcRadiusSub1[k].outer = data[i][j][k].sub1 / extremeValue.max * (this.chartConfig.range[1].max - this.chartConfig.range[1].min) + this.chartConfig.ringValue.outerRadius
            } else if (data[i][j][k].sub1 < 0) {
              arcRadiusSub1[k].inner = this.chartConfig.ringValue.innerRadius - data[i][j][k].sub1 / extremeValue.min * (this.chartConfig.range[0].max - this.chartConfig.range[0].min)
              arcRadiusSub1[k].outer = this.chartConfig.ringValue.innerRadius
            }
            // sub2
            if (data[i][j][k].sub2 > 0) {
              arcRadiusSub2[k].inner = this.chartConfig.ringValue.outerRadius
              arcRadiusSub2[k].outer = data[i][j][k].sub2 / extremeValue.max * (this.chartConfig.range[1].max - this.chartConfig.range[1].min) + this.chartConfig.ringValue.outerRadius
            } else if (data[i][j][k].sub2 < 0) {
              arcRadiusSub2[k].inner = this.chartConfig.ringValue.innerRadius - data[i][j][k].sub2 / extremeValue.min * (this.chartConfig.range[0].max - this.chartConfig.range[0].min)
              arcRadiusSub2[k].outer = this.chartConfig.ringValue.innerRadius
            }
          }
          value.push({
            Main: arcRadiusMain,
            sub1: arcRadiusSub1,
            sub2: arcRadiusSub2
          })
        }
        values.push(value)
      }
      return values
    }
  },
  mounted () {
    this.createRoseChart()
  }
}
</script>

<style scoped>

.container {
  width: 1200px;
  height: 400px;
  border: 2px solid black;
}

</style>

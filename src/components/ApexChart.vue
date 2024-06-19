<template>
  <div class="chart-container">
    <apexchart type="area" height="350" :options="chartOptions" :series="this.series"></apexchart>
  </div>
</template>

<script>
import VueApexCharts from "vue-apexcharts"

export default {
  name: 'ApexChart',
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    series: {
      type: Array,
      required: true
    },
    maxProfit: {
      type: Number,
      required: true
    },
    maxLoss: {
      type: Number,
      required: true
    },
    breakEvenPoints: {
      type: Array,
      required: true
    }
  },
  data() {
    let yaxis = []
    if(this.maxProfit)
      yaxis.push({
        y: this.maxProfit,
        strokeDashArray: 0,
        borderColor: '#775DD0',
        label: {
          borderColor: '#775DD0',
          style: {
            color: '#fff',
            background: '#775DD0',
          },
          text: `Max Profit: ${Math.round(this.maxProfit)}`
        }
      })
    if(this.maxLoss)
      yaxis.push({
        y: this.maxLoss,
        strokeDashArray: 0,
        borderColor: '#00E396',
        label: {
          borderColor: '#00E396',
          style: {
            color: '#fff',
            background: '#00E396',
          },
          text: `Max Loss: ${Math.round(this.maxLoss)}`
        }
      })

    let breakEvenPoints = []
    for(const point of this.breakEvenPoints) {
      breakEvenPoints.push({
        x: point.x,
        y: point.y,
        marker: {
          size: 8,
          fillColor: '#fff',
          strokeColor: 'red',
          radius: 2,
          cssClass: 'apexcharts-custom-class'
        },
        label: {
          borderColor: '#FF4560',
          offsetY: 0,
          style: {
            color: '#fff',
            background: '#FF4560',
          },
          text: 'Break Even Point',
        }
      })
    }

    return {
      chartOptions: {
        chart: {
          type: 'area',
          stacked: false,
          height: 350,
          zoom: {
            type: 'x',
            enabled: true,
            autoScaleYaxis: true
          },
          toolbar: {
            autoSelected: 'zoom'
          }
        },
        dataLabels: {
          enabled: false
        },
        markers: {
          size: 0,
        },
        title: {
          text: 'Stock Price Movement',
          align: 'left'
        },
        fill: {
          type: 'gradient',
          gradient: {
            shadeIntensity: 1,
            inverseColors: false,
            opacityFrom: 0.5,
            opacityTo: 0,
            stops: [0, 90, 100]
          },
        },
        yaxis: {
          labels: {
            formatter: function (val) {
              return Math.round(val)
            },
          },
          title: {
            text: 'Profit/Loss'
          },
        },
        xaxis: {
          tickAmount: 10,
          title: {
            text: 'Price'
          }
        },
        tooltip: {
          shared: false,
          y: {
            formatter: function (val) {
              return Math.round(val)
            }
          }
        },
        annotations: {
          yaxis: yaxis,
          points: breakEvenPoints
        }
      }
    }
  }
}
</script>

<style scoped>
.chart-container {
  margin-top: 20px;
  height: 550px;
  text-align: center;
}
</style>
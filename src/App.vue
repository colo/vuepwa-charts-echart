<template>
  <div class="echarts">
    <IEcharts
      :option="gauge"
    />
			<!--
			:loading="loading"
      @ready="onReady"
      @click="onClick"
      -->
  </div>
</template>

<script type="text/babel">
  import IEcharts from 'vue-echarts-v3/src/full.js';
  export default {
    name: 'echart',
    components: {
      IEcharts
    },
    props: {
			columns: {
				type: [String, Object],
				 default: () => ({value: 100})
			},
		},
    data: () => ({
      loading: true,
      gauge: {
					tooltip : {
							formatter: "{a} <br/>{b} : {c}%"
					},
					toolbox: {
							feature: {
									restore: {},
									saveAsImage: {}
							}
					},
					series: [
							{
									name: 'mem usage',
									type: 'gauge',
									detail: {formatter:'{value}%'},
									data: []
							}
					]
			}
    }),
    watch: {
			columns: function (val) {
				if(typeof(val) == 'string')
					val = JSON.parse(val)
				
				this.loading = !this.loading;
        this.gauge.series[0].data = [val];
				
				console.log('App columns', this.gauge.series[0].data)
				
			},
		},
		mounted () {
			
      if(typeof(this.columns) == 'string')
				this.columns = JSON.parse(this.columns)
			
			console.log(this.columns)
			
			this.gauge.series[0].data = [this.columns];
      
      
    },
    methods: {
      /**doRandom() {
        const that = this;
        let data = [];
        for (let i = 0, min = 5, max = 99; i < 6; i++) {
          data.push(Math.floor(Math.random() * (max + 1 - min) + min));
        }
        that.loading = !that.loading;
        that.gauge.series[0].data = data;
      },**/
      onReady(instance, ECharts) {
        console.log(instance, ECharts);
      },
      onClick(event, instance, ECharts) {
        console.log(arguments);
      }
    }
  };
</script>

<style scoped>
  .echarts {
    width: 400px;
    height: 400px;
  }
</style>

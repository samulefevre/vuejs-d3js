<template>
  <div>
    <h1>Histogram :</h1>
    <svg class="histogram" :width="width" :height="height"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'HistogramChart',
  props: {
    width: { type: Number },
    height: { type: Number },
    data: { type: Array },
    numBins: { type: Number, default: 40 }
  },
  data() {
    return {
      svg: null
    };
  },
  computed: {
    histogram() {
      const countHistogram = d3
        .histogram()
        .domain(d3.extent(this.data)) // min and max of this.data in an array
        .thresholds(this.numBins);

      return countHistogram(this.data).map((bins) => ({
        x: bins.x0,
        y: bins.length
      }));
    }
  },
  methods: {
    initSvg() {
      console.log('initSvg');

      this.svg = d3.select('.histogram').append('g');
      // add g tag to DOM
      this.renderSvg();
    },
    renderSvg() {
      // create scales
      const x = d3
        .scaleBand()
        .padding(0.2)
        .domain(this.histogram.map((d) => d.x))
        .rangeRound([0, this.width]);
      const y = d3
        .scaleLinear()
        .domain([0, d3.max(this.histogram, (d) => d.y)])
        .range([0, this.height]);

      // select rect and data of histogramme
      const selection = this.svg.selectAll('rect').data(this.histogram);

      selection
        .enter()
        .append('rect')
        .attr('class', 'bar')
        .merge(selection)
        .attr(
          'transform',
          (d) => `translate(${x(d.x)} ${this.height}) scale(1 -1)`
        )
        .attr('width', x.bandwidth())
        .attr('height', (d) => y(d.y));

      selection.exit().remove();
    }
  },
  mounted() {
    this.initSvg();
  },
  watch: {
    data: 'renderSvg',
    numBins: 'renderSvg'
  }
};
</script>

<style scoped>
.histogram >>> rect.bar {
  fill: #009688;
}
</style>

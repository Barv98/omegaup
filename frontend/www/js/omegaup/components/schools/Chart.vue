<template>
  <!-- id-lint off -->
  <div id="monthly-solved-problems-chart"></div>
  <!-- id-lint on -->
</template>

<script>
import T from '../../lang';
import * as UI from '../../ui';

export default {
  props: {
    data: Array,
    school: String,
  },
  mounted: function() {
    this.chart = Highcharts.chart('monthly-solved-problems-chart', {
      title: {
        text: UI.formatString(T.profileSchoolMonthlySolvedProblemsCount, {
          school: this.school,
        }),
      },
      chart: { type: 'line' },
      yAxis: {
        min: 0,
        title: {
          text: T.profileSolvedProblems,
        },
      },
      xAxis: {
        title: {
          text: T.wordsMonths,
        },
        labels: {
          rotation: -45,
        },
      },
      legend: {
        enabled: false,
      },
      tooltip: {
        headerFormat: '',
        pointFormat: '<b>{point.y}<b/>',
      },
      series: [
        {
          data: [],
        },
      ],
    });
    this.renderData();
  },
  methods: {
    renderData: function() {
      const solvedProblemsCountData = this.data.map(
        solvedProblemsCount => solvedProblemsCount.problems_solved,
      );
      const solvedProblemsCountCategories = this.data.map(
        solvedProblemsCount =>
          `${solvedProblemsCount.year}-${solvedProblemsCount.month}`,
      );

      this.chart.update(
        {
          xAxis: {
            categories: solvedProblemsCountCategories,
          },
          series: [
            {
              data: solvedProblemsCountData,
            },
          ],
        },
        true /* redraw */,
      );
    },
  },
  watch: {
    data: function() {
      this.renderData();
    },
  },
};
</script>

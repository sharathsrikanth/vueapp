

<script>
import {Bar} from 'vue-chartjs'


export default {
  extends: Bar,

  components: {
   Bar
  },
  data () {
   return {
     temp:'',
     }
  },
  props : ['plotprop','data','label'],
  
  computed: {
    chartData: function() {
      return this.data;
    },
    chartLabel:function(){
      return this.label;
    }
  },  
   watch : {


          '$self.emotionvalue': { 
          deep: true,
          handler: function(value) {          
          alert('vosaxaila')
         // this.$emit('listentoemotionclicked',this.datacollection.emotionvalue)
          }
          },
          data: function() {
          
          this._chart.destroy();
          

          this.render();

    }
   },
   methods: {
          try:function(){
           alert('ssss')
          },
         
          render:function(){
           this.renderChart({
           labels: this.chartLabel,
        datasets: [
          {
            label: 'Synonyms & Antonyms Intensity',
            backgroundColor: '#dd4814',
            data: this.chartData
          }
        ],
        emotionvalue:''
   }, {title: {
                    display: false,
                    text: 'Stacked Bars'
                },
                tooltips: {
                    mode: 'label'
                },
                responsive: true,
                scaleBeginAtZero: false,
                scaleStartValue : -50, 
                maintainAspectRatio: false,
                scales: {
                    xAxes: [
                        {    barThickness : 10,  
                            stacked: true
                        }
                    ],
                    yAxes: [
                        {   barThickness : 10,
                            stacked: true
                        }
                    ]
                },
                onClick : function(e){
                            
          let activePoints = this.getElementAtEvent(e)
          if (activePoints.length > 0) {
           // get the emotion clicked
          let chartData = activePoints[0]['_chart'].config.data  
          let datasetindex = activePoints[0]['_datasetIndex']
          let emotionindex = activePoints[0]['_index']
          let emotion = chartData.labels[emotionindex]
          chartData.emotionvalue=emotion
          
          

    }
  }

          })
        }
            
      
   },
   mounted: function(){
     this.render()  
        }
   }
  

</script>


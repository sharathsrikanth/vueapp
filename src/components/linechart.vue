
<script>
import {Line} from 'vue-chartjs'

export default {
  extends: Line,

  components: {
   Line
  },
  props : ['plotprop','data'],
  
  computed: {
    chartData: function() {
      return this.data;
    }
  },  
   watch : {
          data: function() {
          alert('inside data')
           

     
      alert(this.data)
      this.render();
    }
   },
   methods: {
        
         
          render:function(){
           this.renderChart({labels:['','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','','',''],
        datasets: [
          {
            label: 'Valence',
            backgroundColor: '#dd4814',
            data: this.chartData
          }
        ],
        emotionvalue:''}, {title: {
                    display: false,
                    text: 'Stacked Bars'
                },
                tooltips: {
                    mode: 'label'
                },
                responsive: true,
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
                onClick : 
                function(e){
                            
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


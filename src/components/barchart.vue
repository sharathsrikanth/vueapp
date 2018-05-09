

<script>
import {HorizontalBar} from 'vue-chartjs'


export default {
  extends: HorizontalBar,

  components: {
   HorizontalBar
  },
  data () {
   return {
     temp:'',
     }
  },
  props : ['plotprop','data'],
  
  computed: {
    chartData: function() {
      return this.data;
    },
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
       labels:  ['Anger', 'Anticipation', 'Disgust','Fear','Joy','Sadness','Suprise','Trust'],
        datasets: [
          {
            label: 'Emotion',
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
                    mode: 'label',
                    bodyFontColor : 'white',
                    footerFontColor :'#fff',
                    borderColor : 'rgba(0,0,0,0)'
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


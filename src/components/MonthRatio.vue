<template>
  <div @click="getMonthTwo" id="monthlyRatio">MONTHLY RATIO
    <span v-if="isHidden">
      <span v-for="ratio in ratios" :key="(ratio+Math.random())">
        {{ratio}}
      </span>
    </span>
  </div>
</template>

<script>
export default {
  name: 'MonthRatio',
  props: {
    issueList: Array
  },
  data(){
    return{
      months: Array.apply(null, new Array(12)).map(Number.prototype.valueOf,0),
      ratios:[],
      isHidden: false
    }
  },
  methods: {
    getMonthTwo: function(){
        this.months= Array.apply(null, new Array(12)).map(Number.prototype.valueOf,0)
        this.ratios=[]
        let openMonth=0

        //closedMonth=2 because the current repository has 0 closed issues, which will give the ratio as undefined
        let closedMonth=2
        let curMonth=1
        this.issueList.forEach(issue=>{
          let prevMonth = issue.created_at.slice(5,7)
          if(prevMonth!==curMonth){
            openMonth=0
            closedMonth=0
          }
          curMonth = issue.created_at.slice(5,7)
          if(issue.state==='open'){
            openMonth+=1
          } else if(issue.state==='closed'){
            closedMonth+=1
          }
          this.months[prevMonth-1] = {'open':openMonth,'closed':closedMonth}
        })
        
        this.months.forEach(month=>{
          if(month==0){
            month = {open: 0,closed:0}
          }
          this.ratios.push(calculateRatio(month.open, month.closed))
        })
        this.isHidden = !this.isHidden
        return this.ratios
      }
      
  }
}

function calculateRatio(num_1, num_2){
  console.log(num_1,num_2)
    if(num_1===0){
      return num_1+":"+num_2
    }
    for(let num=num_2; num>1; num--) {
        if((num_1 % num) == 0 && (num_2 % num) == 0) {
            num_1=num_1/num;
            num_2=num_2/num;
        }
    }
    const ratio = num_1+":"+num_2;
    return ratio;
}
</script>

<style scoped>
  #monthlyRatio {
    background:#a30cdf;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 24px;
    margin: 10px;
    padding: 5px;
    border-radius:5px;
  }
</style>
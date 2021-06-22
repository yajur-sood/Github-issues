<template>
<div>
  <div><button @click="getIssues">GET ISSUES</button></div>
  <div>
    <span class="counter">
      OPEN
      <span>{{openCount}}</span>
    <!-- </span> -->
    <!-- <span class="counter"> -->
      |
      CLOSED
      <span>{{closedCount}}</span>
    </span>
  </div>
  <WeekCount :issueList='issueList'/>
  <MonthRatio :issueList='issueList'/>
  <div>
    dates
    <ol>
      <li v-for="issues in issueList" :key="issues.id">
        {{issues.user.login}} 
        <b>{{issues.state}}</b>
        {{issues.id}}
        -
        {{issues.created_at}}
      </li>
    </ol>
  </div>
</div>
</template>

<script>
import MonthRatio from './components/MonthRatio.vue'
import WeekCount from './components/WeekCount.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    MonthRatio,
    WeekCount
  },
  data(){
    return{
      openCount: 0,
      closedCount: 0,
      issueList: [],
    }
  },
  
  methods: {
    getIssues: function(){
      //re-initializing because if user click again on the button then these values will keep adding up to previous ones
      this.issueList=[]
      this.openCount=0
      this.closedCount=0
      const user = 'vuejs'
      const reponame = 'vue'
      axios({
        method: "get",
        url: `https://api.github.com/repos/${user}/${reponame}/issues?state=all&page=1&per_page=100`,
        })
        .then(res => {
          // console.log(res.data)
            res.data.forEach(issue=>{
              if (issue.state==='open'){
                this.openCount+=1
              }
              else if(issue.state==='closed'){
                this.closedCount+=1 
              }
              this.issueList.push(issue)
            })
            this.issueList.sort((a,b)=>{
              return a.created_at-b.created_at
            });
            this.issueList.reverse()
        })
        .catch(err => {
            console.log(err);
        });
      },
    }  
}

</script>

<style scoped>
  button {
    background: #09125d;
    font-family: Arial, Helvetica, sans-serif;
    font-size:18px;
    color: white;
    padding:10px;
    margin:10px;
    border-color: white;
    border-radius: 5px;
    box-shadow: none;
  }
  .counter {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 24px;
    margin: 10px;
    padding: 5px;
    width: 300px;
    background: #01ab81;
    border-radius: 5px;
  }
</style>

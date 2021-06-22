<template>
    <div @click="getWeekCount" id="weekCountDisplay">WEEK WISE COUNT
        <div v-if="isHidden" id="weekNumContainer">
            <span v-for="week in weeks" :key="(week+Math.random()*Math.random())" class="weekNum">
                {{week}}
            </span>
        </div>
  </div>
</template>

<script>
    export default {
        name: 'WeekCount',
        props: {
            issueList: Array
        },
        data() {
            return {
                weeks: Array.apply(null, new Array(52)).map(Number.prototype.valueOf,0),
                dates: [],
                isHidden: false
            }
        },
        methods:{
            getWeekCount: function(){
                this.weeks= Array.apply(null, new Array(52)).map(Number.prototype.valueOf,0)
                this.dates= []
                this.issueList.forEach(item=>{
                    this.dates.push(item.created_at.slice(0,4)+','+item.created_at.slice(5,7)+','+item.created_at.slice(8,10))
                })
                this.dates.forEach(date=>{
                    let mydate = new Date(date)
                    this.weeks[mydate.getWeek()]+=1
                })
                this.isHidden = !this.isHidden
            },
        }
    }

    Date.prototype.getWeek = function (dowOffset) {
        /*getWeek() was developed by Nick Baicoianu at MeanFreePath: http://www.meanfreepath.com */

        dowOffset = typeof(dowOffset) == 'number' ? dowOffset : 0; //default dowOffset to zero
        let newYear = new Date(this.getFullYear(),0,1);
        let day = newYear.getDay() - dowOffset; //the day of week the year begins on
        day = (day >= 0 ? day : day + 7);
        let daynum = Math.floor((this.getTime() - newYear.getTime() - 
            (this.getTimezoneOffset()-newYear.getTimezoneOffset())*60000)/86400000) + 1;
        let weeknum;
        //if the year starts before the middle of a week
        if(day < 4) {
            weeknum = Math.floor((daynum+day-1)/7) + 1;
            if(weeknum > 52) {
                let nYear = new Date(this.getFullYear() + 1,0,1);
                let nday = nYear.getDay() - dowOffset;
                nday = nday >= 0 ? nday : nday + 7;
                /*if the next year starts before the middle of
                the week, it is week #1 of that year*/
                weeknum = nday < 4 ? 1 : 53;
            }
        }
        else {
            weeknum = Math.floor((daynum+day-1)/7);
        }
        return weeknum;
    };

</script>

<style scoped>
#weekCountDisplay {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 24px;
    background: #f98b07;
    margin: 10px;
    padding: 5px;
    border-radius:5px;
}

/* #weekNumContainer {
    margin-top:10px;
}

.weekNum {
    background: white;
    font-family: Arial, Helvetica, sans-serif;
    color:#f98b07;
    font-size: 24px;
    margin-top:10px;
    border-radius:5px;
    padding-left:5px;
    margin-right:5px; */
/* } */
</style>

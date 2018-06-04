<template>
    <div class="today"> 
        <div class="goldtoday">
            <div class="banner">
                <div>
                    <p>今日入金(万元)</p>
                    <span>{{todayInvestAmount}}</span>
                </div>

                <ul class="goldlist">
                    <li>
                        <span>{{releaseAmount}}</span>
                        <span>今日放款</span>
                    </li>
                    <li class="bor">
                        <span>5727</span>
                        <span>渠道入金(元)</span>
                    </li>
                    <li>
                        <span>{{todayUserCount}}/{{todayInvestCount}}</span>
                        <span>投资人数/笔数</span>
                    </li>
                </ul>
            </div>
        </div>
        <div class="currentvote table list">
            <h2>当前在投</h2>
            <ul>
                <li v-for='i in openStatusLoanList'>
                    <table>
                        <tr>
                            <th>{{i.termCount}}月标</th>
                            <th>{{i.openTime}}</th>
                            <th>{{i.amount}}</th>
                            <th>{{i.bishu}}/{{i.biddingAmount}}</th>
                        </tr>
                        <tr>
                            <td></td>
                            <td>开标时间</td>
                            <td>额度(元)</td>
                            <td>笔数/额度(元)</td>
                        </tr>
                    </table>
                    <div class="progress">
                        <div>
                            <div class="schedule" :style="{ width:i.schedule }">
                                
                            </div>
                        </div>
                        <span>
                            {{i.schedule}}
                        </span>
                    </div>
                </li>
                <li class="no-data" v-show="openStatusLoanList && openStatusLoanList.length === 0">暂时没有数据</li>
            </ul>
        </div>
        <div class="fullstandard table list">
            <h2>已经满标/放款</h2>
            <ul>
                <li v-for='item in finishStatusLoanList'>
                    <table>
                        <tr>
                            <th>{{item.termCount}}月标</th>
                            <th>{{item.openTime}}</th>
                            <th>{{item.amount}}</th>
                            <th>{{item.fullTime}}</th>
                        </tr>
                        <tr>
                            <td>已放款</td>
                            <td>开标时间</td>
                            <td>额度(元)</td>
                            <td>满标时间</td>
                        </tr>
                    </table>
                </li>
                <li class="no-data" v-show="finishStatusLoanList && finishStatusLoanList.length === 0">暂时没有数据</li>
            </ul>
        </div>
        <div v-show="!isReady" class="loading">
            <div>
                讀取中...
            </div>
        </div>
    </div>
</template>

<script>
export default {
  components:{
  },
  data () {
    return {
      isReady: false,
      releaseAmount: 0, // 今日放款金额
      todayInvestAmount: 0, //今日入金
      todayUserCount: 0, // 今日投资人数
      todayInvestCount: 0,  // 今日投资笔数
      ytdTitleTime: '',  // 昨日标题日期
      todayTitleTime: '',  // 今日标题日期
      
      
      openStatusLoanList: [], // 当前在投集合
      finishStatusLoanList: []  // 已满标/放款集合
    }
  },
  methods:{
    onLoaded:function(){
      this.$http({
          method:"get",
          url:"/operationData/todayOperatingData",
      }).then(res=>{

          if(res.data.retCode !== 200 ){
            return;
            }   
        // console.log(res)
        
        let 
            todayData = res.data.resultData,
                //已满标
            fullList = todayData. finishStatusLoanList, 
                //当前在投
            currentList = todayData. openStatusLoanList;
        ; 
            //今日放款
        this.releaseAmount = todayData.releaseAmount;
            //今日入金
            let todaythousand = todayData.todayInvestAmount/10000;
        this.todayInvestAmount = todaythousand;
            //人数
        this.todayUserCount = todayData.todayUserCount;
            //笔数
        this.todayInvestCount = todayData.todayInvestCount;

        // let aa = '05?20?'
        // let yesTime = todayData.todayTitleTime.replace("?", '月')
        // let todayTime = yesTime.replace('?',"日")
        // let ytdTime = todayData.ytdTitleTime.replace("?", '月')
        // let YesterdayTime = ytdTime.replace('?',"日")
            //今日
        this.todayTitleTime = todayData.todayTitleTime;
            //昨日
        this.ytdTitleTime = todayData.ytdTitleTime;

        //当前在投
        for(let i in currentList){
            let OpenTime = currentList[i].openTime;
              let Otime = new Date ( OpenTime ).toLocaleString().split(" ")[0];
              currentList[i].openTime = Otime;
              
              let Schedule = ((currentList[i].biddingAmount/currentList[i].amount)*100).toFixed(2) + '%'
              currentList[i].schedule = Schedule;
        }
        this.openStatusLoanList = currentList;
        //已满标
        for(let i in fullList){
             let OpenTime = fullList[i].openTime;
              let Otime = new Date ( OpenTime ).toLocaleString().split(" ")[0];
              
              let FullTime = fullList[i].fullTime;
              let Ftime = new Date ( FullTime ).toLocaleString().split(" ")[0];
              fullList[i].openTime = Otime;
              fullList[i].fullTime = Ftime;
        }
        this.finishStatusLoanList = fullList;
        this.isReady = true;
      }).catch(function(err){
        console.log(err);
      })
    }
   },
   created() {
      this.onLoaded()
    }
}
</script>
<style scoped>
    
    /*进度*/
    .progress{
        display:flex;
        justify-content:flex-end;
        margin-top: 0.2rem;
    }
    .progress>div{
        width:58%;
        
        height:0.14rem;
        background:#d9dbdf;
        position:relative;
    }
    .progress span{
        padding:0 0.2rem;
        font-size: 0.2rem;
        color:#296fe1;
        font-weight: 600;
        line-height: 0.15rem;
    }
    .schedule{
        /*width:5%;*/
        height:100%;
        background:#0082f6;
        position:absolute;
        left:0;
        top:0;
    }
    .goldtoday{
        background: url(../../assets/images/banner.jpg) no-repeat;
        background-size: cover;
    }
    .goldtoday>div div{
        text-align: left;
        padding:0.9rem 0 0.6rem 30%;
    }
    .goldtoday>div div p{
        color:#Fff;
        line-height: 0.5rem;
    }
    .goldtoday>div div span{
        font-size: 0.8rem;
        color:#Fff;
    }
    .list ul{
        display:flex;
        flex-direction:  column;
    }
    tr td,tr,th{
        flex:1;
        overflow: hidden;
    }
    .list li{
        flex:1;
         flex-direction:  column;

    }
    h2{
        text-align: left;
    }
    .list ul li table{
        width:100%;
    }
    tr{
        display:flex;
    }
    tr td,tr,th{
        justify-content: flex-start;
        flex:1;
    }
    td{
        font-size: 0.3rem;
    }
    .table table th{
        color:#333333;
        font-size: 0.2rem;
    }
    .table table td{
        color:#555555;
        font-size: 0.2rem;
        height: 0.4rem;
        line-height: 0.4rem;
    }
    .table table th:nth-child(1){
        color:#f5772d;
        font-size: 0.26rem;
    }
    .table table th:nth-child(2){
        color:#333333;
    }
    .table table th:nth-child(3){
        color:#296fe1;
    }
    .table table th:nth-child(4){
        color:#296fe1;
    }
    .table li{
        border-bottom: 0.01rem solid #e4e4e4;
        padding:0.3rem 0 0.3rem 0;
        display:flex;
    }
    .table li:last-child{ 
        border-bottom: none;
    }
    .table table th:nth-child(3),.table table td:nth-child(3){
        border-right: 0.01rem solid #dcdcdc;
        border-left: 0.01rem solid #dcdcdc;
    }
    .table table th:last-child,.table table td:last-child{ 
        border-right: none;
    }
</style>
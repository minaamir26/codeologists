<template>
<div>
   <div v-for="request in requests">



    <el-card style="width: 650px">
      <div slot="header" class="clearfix">
          <span style="float:left line-height: 36px; font-size: 18px; font-weight: bold;">
            You have a game request by {{ request.playerUsername}}
          </span>
          <br>
          <span style="font-weight: bold;">He left you a comment :</span>
          <br>
          <span>{{request.comment}}</span>
          <form action="/RejectRequest" method="POST" @submit.prevent="rej(request)">
             <button class="w3-red" style="float: right;" type="submit">Reject</button>
          </form>     
          <form action="/AcceptRequest" method="POST" @submit.prevent="acc(request)">
            <button class="w3-green" style="float: right;" type="submit">Accept</button>
          </form>
        </div>
    </el-card>


   <!--  <article class="message" >
        <div class="message-header">
            <p>Requet details: {{this.requests}}</p>
        </div>

        <h3>Player Username :{{request.playerUsername}}</h3>
        <h3>Comment :{{request.comment}} </h3>
        <form action="/AcceptRequest" method="POST" @submit.prevent="acc(request)">
           <button type="submit" class="button is-info" >Accept</button>
        </form>
        <form action="/RejectRequest" method="POST" @submit.prevent="rej(request)">
           <button type="submit" class="button is-info" >Reject</button>
        </form>
    </article> -->
</div>
</div>
</template>


<script>

  export default {
    data(){
        return{
          form: new Form({
            playerUsername:""
          }),
          requests:[],
          id:""
        };
   },

    mounted(){
        axios.get("/myrequests").then(res=>{
          for(var i=res.data.length-1 ;i>=0;i--){
          this.requests.push(res.data[i]);
        }
        }); 

        axios.get("/mygame").then(res=>{
        this.id=res.data
        }); 

  },
  methods:{
  acc(request){
    this.form.playerUsername=request.playerUsername;
     this.form.submit('post','/AcceptRequest/'+this.id).then(response => alert(response))
    .catch(errors => alert(errors));
     this.requests.splice(this.requests.indexOf(request),1);
  }
,rej(request){
  this.form.playerUsername=request.playerUsername;
     this.form.submit('post','/RejectRequest/'+this.id).then(response => alert(response))
    .catch(errors => alert(errors));
         this.requests.splice(this.requests.indexOf(request),1);
    }

   }
}
</script>
<template>
  <div class="wrapper">
    <h1>独り言App</h1>
    <p>つぶやく内容を入力してください</p>
    <input type="text" v-model="newTweet">
    <button @click="postTweet">つぶやく</button>
    <h2>つぶやき一覧</h2>
    <div class="tweets">
      <ul>
        <li v-for="tweet in tweets" :key="tweet.id">
          {{tweet.content}}
          <button @click="deleteTweet(tweet.id)">削除</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      newTweet:null,
      tweets:[],
    };
  },
  methods:{
    async getTweets(){
      const resData = await this.$axios.get("http://127.0.0.1:8000/api/tweets");
      this.tweets = resData.data.data;
    },
    async postTweet(){
      const sendData={
        content:this.newTweet
      };
      await this.$axios.post("http://127.0.0.1:8000/api/tweets", sendData);
      this.newTweet = "",
      this.getTweets();
    },
    async deleteTweet(id){
      await this.$axios.delete("http://127.0.0.1:8000/api/tweets/" + id);
      this.getTweets();
    }
  },
  created(){
    this.getTweets();
  },
}
</script>

<style>
.wrapper{
  width:800px;
  margin:0 auto;
  text-align: center;
}
.tweets{
  border:3px solid rgba(128, 128, 128, 0.644);
  margin:0 auto;
  width:50%;
}

.tweets ul li{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding:10px;

}

li{
  list-style: none;
}

li:before{
  content: "";
  background-color: red;
  width:50px;
  height:50px;
  border-radius: 50%;
}
</style>
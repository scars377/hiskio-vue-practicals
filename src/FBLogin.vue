<template>
  <div>
    <span>{{connected}}</span>
    <button v-if="!connected" @click="connect">
      Connect
    </button>
    <div v-else>
      <h4>{{username}}</h4>
      <img :src="picture"/>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      connected: false,
      username: '',
      picture: '',
    };
  },
  methods:{
    connect(){
      FB.login(this.statusGet);
    },
    statusGet(result){
      if(result.status === 'connected'){
        this.connected = true;
        FB.api('/me', 'GET', {
          fields:['picture', 'name'],
        }, rs => {
          this.userID = rs.id;
          this.username = rs.name;
          this.picture = rs.picture.data.url;
        });
      }
    },
    submit(){
    },
  },

  mounted(){
    FB.init({
      appId: 182706955649650,
      version: 'v2.12',
    });
    FB.getLoginStatus(this.statusGet);
  },
};
</script>
<template>
  <div>
    <div id="add" style="display:block">
      Title
      <input v-model="title" />
      <br/>Body
      <input v-model="body" />
      <button @click="postData()">Post Data</button>
    </div>
  </div>
</template>
<script>
import axios from "axios";
const url = "m/";
export default {
  props: ["post","addpost"],
  data() {
    return {
      id: 1,
      title: "",
      body: ""
    };
  },
  methods: {
    postData() {
      document.getElementById('add').style.display='none'
      axios
        .post(url, {
          userId: 1,
          title: this.title,
          body: this.body
        })
        .then(res => (this.post=res.data.data));
        this.$emit("get",this.addpost);
    },
  }
};
</script>
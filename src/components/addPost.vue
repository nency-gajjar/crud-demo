<template>
  <div style="display:block">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
    <div class="container">
    <center><h2>Update Your Data Form</h2></center>
    <br>
    <form>
    <div class="form-group row">
      <label for="title" class="col-sm-2 col-form-label">Title : </label>
      <div class="col-sm-10">
        <input type="text" class="form-control" id="title" placeholder="Enter Title"
        v-model="title">
        <!-- {{title}} -->
      </div>
    </div>
    <div class="form-group row">
      <label for="body" class="col-sm-2 col-form-label">Body : </label>
      <div class="col-sm-10">
        <input type="text" class="form-control" id="body" placeholder="Enter Body Text"
        v-model="body">
        <!-- {{body}} -->
      </div>
    </div>
        
    <div class="form-group row">
      <div class="col-sm-10">
      <button class="btn btn-success" @click="postData()"><i class="material-icons"></i> <span>Post Data</span></button>
      <button class="btn btn-warning" @click="editPost()"><i class="material-icons"></i><span>Edit Data</span></button>	
      <!-- <button @click="postData()" class="btn-floating  btn-primary">Post Data</button>
      <button @click="editPost()" class="btn-floating  btn-primary">Edit Data</button> -->
      </div>
    </div>
    
  </form>
  </div>

    <!-- ======== -->
    <!-- <div id="add" style="display:block">
      Title
      <input v-model="title" />
      <br/>Body
      <input v-model="body" />
      <button @click="postData()">Post Data</button>
      <button @click="editPost()">Edit Data</button>
    </div> -->


  </div>
</template>
<script>
import axios from "axios";
const url = "http://192.168.2.65:3030/posts/";
export default {
  props: ["post","addpost" ,"editKey"],
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
        }).then(res => (this.post=res.data.data));
        this.$emit("get",this.addpost);
      },

    editPost(){
      axios.patch(url+this.editKey,{
        title:this.title,
        body:this.body
      }).then(response => {
          this.post   = response.data.data
          this.$emit("get" , this.addpost)
       })
    }
  }
};
</script>

<style scoped>
button { 
  margin-top: 20px;
  padding: 10px;
}

button:nth-child(2) {
  margin-left: 20px;
  padding: 10px;
}
</style>
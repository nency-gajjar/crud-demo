<template>
    <div id="trainee-table">
        <div v-if="!addPost">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
        <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
        <div class="container">
        <div class="table-wrapper">
            <div class="table-title">
                <div class="row">
                    <div class="col-sm-6">
						<h2>Manage <b>Posts</b></h2>
					</div>
					<div class="col-sm-6">
                        <!-- <a href="#GetPostModal" class="btn btn-info" data-toggle="modal"><i class="glyphicon glyphicon">&#xE147;</i></a> -->
						<!-- <button class="btn btn-success"  ><i class="material-icons">&#xE147;</i> <span>Add New Post</span></button> -->
						<button class="btn btn-success" @click="addPost='true'" ><i class="material-icons">&#xE147;</i> <span>Add New Post</span></button>
                        <button class="btn btn-info" @click="reloadPage()"><i class="material-icons glyphicon glyphicon-refresh"></i></button>						
					</div>
                </div>
            </div>
            <table class="table table-striped table-hover">
                <thead>
                    <tr>
						<th>
							<span class="custom-checkbox">
								<input type="checkbox" id="selectAll">
								<label for="selectAll"></label>
							</span>
						</th>
                        <th>User Id</th>
                        <th>Title</th>
                        <th>Body</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    
                    <tr v-for="(post,index) in posts" :key="index">
                        <td>
							<span class="custom-checkbox">
								<input type="checkbox" id="selectAll">
								<label for="selectAll"></label>
							</span>
						</td>
                        <td>{{ post.userId}}</td>
                        <td>{{ post.title }}</td>
                        <td>{{ post.body }}</td>
                        <td>
                            <a href="#addPostModal" @click="editPost()" class="edit" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Edit">&#xE254;</i></a>
                            <a href="#deletePostModal" @click="deletePost(index,post._id)" class="delete" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Delete">&#xE872;</i></a>
                        </td>
                    </tr>
                    <!-- <tr>    
						<td>
							<span class="custom-checkbox">
								<input type="checkbox" id="checkbox1" name="options[]" value="1">
								<label for="checkbox1"></label>
							</span>
						</td>
                        <td>1</td>
                        <td>post 1</td>
                        <td>commodi nesciunt rem tenetur doloremqu</td>
                        <td>
                            <a href="#editPostModal" class="edit" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Edit">&#xE254;</i></a>
                            <a href="#deletePostModal" class="delete" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Delete">&#xE872;</i></a>
                        </td>
                    </tr>
                    
                    <tr>
						<td>
							<span class="custom-checkbox">
								<input type="checkbox" id="checkbox5" name="options[]" value="1">
								<label for="checkbox5"></label>
							</span>
						</td>
                        <td>1</td>
                        <td>post 2</td>
                        <td>ullam et saepe reiciendis voluptatem </td>
                        <td>
                            <a href="#editPostModal" class="edit" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Edit">&#xE254;</i></a>
                            <a href="#deletePostModal" class="delete" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Delete">&#xE872;</i></a>
                        </td>
                    </tr>  -->
                </tbody>
            </table>
        </div></div>
        
    </div>
    <div v-else>
        <addPost :post="post" :addPost="addPost" @get="get($event)"/>    
    </div>
    </div> 
</template>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
<script>
import axios from 'axios'
import addPost from '@/components/addPost.vue'
const API_URL = 'http://192.168.2.65:3030/posts/'
export default {
    name: 'trainee-table',
    components:{
        addPost
    },
    data(){
      return {
      posts: null,
      result: null,
      addPost: false
      }
    },

    async created(){
        this.posts = await this.getPost()
    },
   
    methods: {
        reloadPage(){
            window.location.reload()
        },

        async getPost(){
            const url  = API_URL 
            return (await axios.get(url)).data.data;
        },
        
        async deletePost(result, userId){
            const url  = API_URL + userId
            axios.delete(url).then( response => {
                this.result.splice(userId , 1)
            })
            console.log(this.result) 
            // return (await axios.delete(url)).data.data;
        },

        get(){
            this.addPost = false
        },

        deletePost(index, key){
            axios.delete(API_URL + key).then(responce =>{
                this.posts.splice(index ,1)
            })
        },

        editPost(){
            axios.patch('http://192.168.2.65:3030/posts?userId=1', { title : this.edittitle, body : this.editbody })
        .then(response => {console.log("here",this.posts)
          // this.posts[id] = response.data
          // this.newtitle = ''
          // this.newbody = ''
          console.log("here",this.posts)
          // handle success
        })            

            axios.patch('http://192.168.2.65:3030/posts?userId=1').then(response=>{

            })
        }   
        // patch("http://192.168.2.65:3030/posts/{id}")
    }
}
</script>

<style scoped>
#app{
    color: #566787;
	font-family: 'Varela Round', sans-serif;
	font-size: 13px;
}
.table-wrapper {
    background: #fff;
    padding: 20px 25px;
    margin: 25px 0;
	border-radius: 3px;
    box-shadow: 0 1px 1px rgba(0,0,0,.05);
}
.table-title {        
	padding-bottom: 15px;
	background: #435d7d;
	color: #fff;
	padding: 16px 30px;
	margin: -20px -25px 10px;
	border-radius: 3px 3px 0 0;
}
.table-title h2 {
	margin: 5px 0 0;
	font-size: 24px;
}
.table-title .btn-group {
	float: right;
}
.table-title .btn {
	color: #fff;
	float: right;
	font-size: 13px;
	border: none;
	min-width: 50px;
	border-radius: 2px;
	border: none;
	outline: none !important;
	margin-left: 10px;
}
.table-title .btn i {
	float: left;
	font-size: 21px;
	margin-right: 5px;
}
.table-title .btn span {
	float: left;
	margin-top: 2px;
}
table.table tr th, table.table tr td {
    border-color: #e9e9e9;
	padding: 12px 15px;
	vertical-align: middle;
}
table.table tr th:first-child {
	width: 60px;
}
table.table tr th:last-child {
	width: 100px;
}
table.table-striped tbody tr:nth-of-type(odd) {
	background-color: #fcfcfc;
}
table.table-striped.table-hover tbody tr:hover {
	background: #f5f5f5;
}
table.table th i {
    font-size: 13px;
    margin: 0 5px;
    cursor: pointer;
}	
table.table td:last-child i {
	opacity: 0.9;
	font-size: 22px;
    margin: 0 5px;
}
table.table td a {
	font-weight: bold;
	color: #566787;
	display: inline-block;
	text-decoration: none;
	outline: none !important;
}
table.table td a:hover {
	color: #2196F3;
}
table.table td a.edit {
    color: #FFC107;
}
table.table td a.delete {
    color: #F44336;
}
table.table td i {
    font-size: 19px;
}
table.table .avatar {
	border-radius: 50%;
	vertical-align: middle;
	margin-right: 10px;
}
</style>
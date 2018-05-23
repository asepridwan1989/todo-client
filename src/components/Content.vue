<template>
    <div class="inputtask">
    <h1>Add Task</h1>
        <form>
            <div class="form-group">    
                <input type="text" class="form-control" placeholder="Enter your task" v-model="task">
            </div>
            <button type="submit" class="btn btn-primary" @click="addtask">Save</button>
        </form>
        <br/>
        <div class="d-flex flex-row">
            <div class="p-3">
                <form class="form-inline my-2 my-lg-0">
                    <input class="form-control mr-sm-2" type="search" placeholder="Search task" aria-label="Search" v-model="query">
                    <button class="btn btn-outline-success my-2 my-sm-0" type="submit" @click="search">Search</button>
                </form>
            </div>
            <div class="p-3">
                <button class="btn btn-outline-success my-2 my-sm-0" type="submit" @click="getall">show all</button>
            </div>
            <div class="p-3">
                <button class="btn btn-outline-success my-2 my-sm-0" type="submit" @click="showCompleted">show complete</button>
            </div>
            <div class="p-3">
                <button class="btn btn-outline-success my-2 my-sm-0" type="submit" @click="showUncomplete">show uncomplete</button>
            </div>
        </div>
        <br>
        <div class="wrap">
            <button class="btn btn-outline-secondary" data-toggle="modal" data-target="#create">check football schedule</button>
            <div class="modal" tabindex="-1" role="dialog" id='create'>
                <div class="modal-dialog" role="document">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title">Football schedule</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <p>date to check:</p>
                            <input type="text" class="form-control"  placeholder="example: 2018-05-30" v-model="date">
                        </div>
                        <div>
                            <table class="table table-striped">
                                <thead>
                                    <tr>
                                    <th scope="col">no</th>
                                    <th scope="col">team</th>
                                    <th scope="col">time</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(match,index) in matches" v-bind:key="index">
                                        <td style="vertical-align: middle;">{{index+1}}</td>
                                        <td align="left" style="vertical-align: middle;" ><p>{{match.match_awayteam_name}} vs {{match.match_hometeam_name}}</p></td>
                                        <td>{{match.match_time}}</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-primary" @click="getSchedule">get schedule</button>
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <br/>
        <table class="table table-striped">
            <thead>
                <tr>
                <th scope="col">no</th>
                <th scope="col">task</th>
                <th scope="col">status</th>
                <th scope="col">created at</th>
                <th scope="col">last update</th>
                <th scope="col">action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(todo,index) in todos" v-bind:key="index">
                    <td style="vertical-align: middle;"><h4>{{index+1}}</h4></td>
                    <td align="left" style="vertical-align: middle;" ><h4>{{todo.task}}</h4></td>
                    <td style="vertical-align: middle;"><h4>{{todo.completeStat}}</h4></td>
                    <td style="vertical-align: middle;"><h4>{{todo.createdAt | moment("MMMM Do YYYY, h:mm:ss") }}</h4></td>
                    <td style="vertical-align: middle;"><h4>{{todo.updatedAt | moment("MMMM Do YYYY, h:mm:ss") }}</h4></td>
                    <td>
                        <div>
                            <button @click="updateStat" :data-id="todo._id" class="btn btn-outline-secondary">done</button>
                        </div>                    
                        <div class="wrap">
                            <button class="btn btn-outline-secondary" data-toggle="modal" data-target="#update">Change task name</button>
                            <div class="modal" tabindex="-1" role="dialog" id='update'>
                                <div class="modal-dialog" role="document">
                                    <div class="modal-content">
                                        <div class="modal-header">
                                            <h5 class="modal-title">Change task name</h5>
                                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                                <span aria-hidden="true">&times;</span>
                                            </button>
                                        </div>
                                        <div class="modal-body">
                                            <p>New task name:</p>
                                            <input type="text" class="form-control"  placeholder="enter your new task name" v-model="task">
                                        </div>
                                        <div class="modal-footer">
                                            <button type="button" class="btn btn-primary" data-dismiss="modal" :data-id="todo._id" @click="updateName">Update</button>
                                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div>
                            <button @click="deleteTask" :data-id="todo._id" class="btn btn-outline-secondary" >delete</button> 
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
        <h3>{{ nothing }}</h3>
    </div>
    
</template>

<script>
export default {
 name: "Content",
 data: function(){
     return {
         task:'',
         todos:'',
         stat:'',
         query:'',
         date:'',
         matches:'',
         nothing:''
     }
 },
 computed:{
     convert(){
        if(todo.completeStat == false){
            stat = 'uncomplete'
        }         
     }
 },
 created(){
    this.getall()
 },
 methods:{
     addtask(){
        let body = {
                task : this.task
            }
        let headers = {
                token : window.localStorage["todo-token"]
            }
        axios.post('https://powerful-sands-33447.herokuapp.com//todo', body, {headers:headers})
        .then( response => {
            window.location.reload(true);
        })
        .catch( err => {
            this.error = err.response.data.message
        })
     },
     updateName(){
        let id = event.srcElement.dataset.id
        let body = {
                task : this.task
            }
        let headers = {
                token : window.localStorage["todo-token"]
            }
        axios.put(`https://powerful-sands-33447.herokuapp.com/todo/${id}`, body, {headers:headers})
        .then( response => {
            window.location.reload(true);
        })
        .catch( err => {
            this.error = err.response.data.message
        })
     },
     updateStat(event){
        let id = event.srcElement.dataset.id
        let body = {
                completeStat : 'completed'
            }
        let headers = {
                token : window.localStorage["todo-token"]
            }
        axios.put(`https://powerful-sands-33447.herokuapp.com/todo/${id}`, body, {headers:headers})
        .then( response => {
            window.location.reload(true);
        })
        .catch( err => {
            this.error = err.response.data.message
        })
     },
     deleteTask(){
        let id = event.srcElement.dataset.id
        let headers = {
                token : window.localStorage["todo-token"]
            }
        axios.delete(`https://powerful-sands-33447.herokuapp.com/todo/${id}`, {headers:headers})
        .then( response => {
            window.location.reload(true);
        })
        .catch( err => {
            this.error = err.response.data.message
        })
     },
     search(){
        let headers = {
            token : window.localStorage["todo-token"]
        }
        axios.get(`https://powerful-sands-33447.herokuapp.com/todo/search?task=${this.query}`, {headers:headers})
        .then( response => {            
            this.todos = response.data.data
            if(response.data.message == 'nothing to show'){
                this.nothing = `${response.data.message} for keyword : '${this.query}'` 
            }else{
                this.nothing =''
            }            
        })
        .catch( err => {
        })
     },
     getall(){
        let headers = {
                token : window.localStorage["todo-token"]
            }
        axios.get('https://powerful-sands-33447.herokuapp.com/todo', {headers:headers})
        .then( response => {            
            this.todos = response.data.data
            if(response.data.message == 'you dont have any task'){
                this.nothing = response.data.message
            }else{
                this.nothing =''
            }
        })
        .catch( err => {
        })
     },
     showUncomplete(){
        let headers = {
                token : window.localStorage["todo-token"]
            }
        axios.get('https://powerful-sands-33447.herokuapp.com/todo/uncomplete', {headers:headers})
        .then( response => {            
            this.todos = response.data.data
            this.todos = response.data.data
            if(response.data.message == 'there is no uncomplete task'){
                this.nothing = response.data.message
            }else{
                this.nothing =''
            }
        })
        .catch( err => {
        })
     },
    showCompleted(){
        let headers = {
                token : window.localStorage["todo-token"]
            }
        console.log('complete function')
        axios.get('https://powerful-sands-33447.herokuapp.com/todo/complete', {headers:headers})
        .then( response => {            
            this.todos = response.data.data
            if(response.data.message == 'there is no completed task'){
                this.nothing = response.data.message
            }else{
                this.nothing =''
            }
        })
        .catch( err => {
            console.log(err)
        })
    },
    getSchedule(){
        axios.get(`https://apifootball.com/api/?action=get_events&from=${this.date}&to=${this.date}&country_id=222&APIkey=c1ee5b863b96422ccf9bdbfbe8c49ba34230a3b4a95daeebc920fe6718cbf3a6`)
        .then(football=>{
            console.log(football.data)
            this.matches = football.data
        })
    }      
 }
}
</script>

<style>
.inputtask{
    margin-left:150px;
    margin-right:150px;
}
</style>



<template >
    <div class="card">
        <h3>Login if you have account !</h3> 
        <p style="color:red">{{error}}</p>
        <div class="card-body">
            <form>                
                <div class="form-group">
                    <label for="exampleInputEmail1">Email address</label>
                    <input type="text" class="form-control" aria-describedby="emailHelp" placeholder="Enter email" v-model="email">
                </div>
                <div class="form-group">
                    <label for="exampleInputPassword1">Password</label>
                    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password" v-model="password">
                </div>
                <div class="form-check">
                </div>
                <button type="submit" class="btn btn-primary" @click="login">Login</button>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Login',
        data(){
            return{
                email:"",
                password:"",
                token:"",
                fail:false,
                error:""
            }
        },
        methods: {
            login() {
                let payload = {
                    email: this.email,
                    password: this.password
                }
                 axios.post('https://powerful-sands-33447.herokuapp.com/users/signin', payload)
                  .then( response => {
                      console.log(response);
                      this.token = response.data.token
                    //   alert("succesfuly registered")
                      // success = true
                      window.location.reload(true);
                      localStorage.setItem('todo-token', this.token);
                  })
                  .catch( err => {
                        this.error = err.response.data.message
                  })
            }
        } 
    }
</script>


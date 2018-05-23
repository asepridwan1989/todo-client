<template >
    <div class="card">
        <h3>Register first, if you don't have account !</h3>
        <div class="card-body">            
            <form>                
                <div class="form-group">
                    <label >Email address</label>
                    <p style="color:red">{{errorEmail}}</p>
                    <input type="text" class="form-control" v-model="email" aria-describedby="emailHelp" placeholder="Enter email">
                </div>
                <div class="form-group">
                    <label >Password</label>
                    <p style="color:red">{{errorPass}}</p>
                    <input type="password" class="form-control" v-model="password" placeholder="min 8 char, mix letter and number">
                </div>
                <div class="form-check">
                </div>
                <button type="submit" class="btn btn-primary" @click="register" >Register</button>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Register',
        data(){
            return{
                email:"",
                password:"",
                errorEmail:"",
                errorPass:"",
                fail:false,
                error:""
            }
        },
        methods: {
            register() {
                let payload = {
                    email: this.email,
                    password: this.password
                }

                console.log(payload)
                 axios.post('https://powerful-sands-33447.herokuapp.com/users/signup', payload)
                  .then( response => {
                      console.log(response);
                      alert("succesfuly registered")
                      // success = true
                       window.location.reload(true);
                      // localStorage.setItem('happy-token', this.token);
                  })
                  .catch( err => {
                        this.error = err.response.data.message
                        console.log(this.error.mail)
                        this.errorEmail = this.error.mail
                        this.errorPass = this.error.pass
                  })
            }
        } 
    }
</script>

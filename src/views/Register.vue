<template>
  <div class="container" >
    <div class="d-flex">
        <div class="container center">
            <div class="login">
                <h1 style="font-size: 30px;">Register Account</h1>
            </div>
            <div>
                <div class="d-flex">
                    <input style="margin-right: 20px" class="form1 form-control" id="firstName" type="text" placeholder="First Name" v-model="register.firstName">
                    <input class="form1 form-control" id="lastName" type="text" placeholder="Last Name" v-model="register.lastName" >
                </div>
                <input class="form form-control" id="user" type="email" placeholder="Email" v-model="register.email">
                <input class="form form-control" id="pass" type="password" placeholder="Password" v-model="register.password">
                <input class="form form-control" id="pass2" type="password" placeholder="Repeat Password" v-model="register.password2"  @click="signup()">
            </div>
            <div>
                <p class="text-danger">{{correct}}</p>
            </div>
            <div class="sign btn" @click="signup()" >
                SIGN UP
            </div>
            <div>
                <h2 style="font-size: 20px;">Already have an account? <a href="/login">Login</a></h2>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
import qs from 'qs'

export default {
    name: "login",
    data() {
        return {
            correct: '',
            register: {
                firstName: '',
                lastName: '',
                email: '',
                password: '',
                password2 : ''
            },
            data: []
        }
    },
    mounted(){
        if(localStorage.Token){
            this.$router.push('/')
        }
    },
    methods: {
        signup() {
            if (this.register.password !== this.register.password2) {
                return this.correct = 'Password not match'
            }
            if (this.register.email.length <= 1){
                return this.correct = 'Please add email'
            }
            if (this.register.password.length <= 1){
                return this.correct = 'Please add password'
            }
            const datas = { 
                'firstName': this.register.firstName,
                'lastName' : this.register.lastName,
                'email' : this.register.email,
                'password' : this.register.password
                }
            const options = {
            method: 'POST',
            url: process.env.VUE_APP_REGISTER,
            headers: { 'content-type': 'application/x-www-form-urlencoded' },
            data: qs.stringify(datas),
            };
            Axios(options)
            .then(res => {
                if(res.data.result.length > 0){
                    this.correct = res.data.result
                }else {
                    alert('Registration completed successfully')
                    this.$router.push('/login')
                }
                
            })
            .catch( err => {
                console.log(err)
            })
        },
        check(){
            console.log(this.register.email.length < 1)
        }
    }
}
</script>

<style scoped>
.center {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    width: 500px;
    height: 500px;
    border: 2px solid white;
    border-radius: 5px;
    box-shadow: 0 3px 20px 0px rgba(0, 0, 0, 0.1);
    background-color: white;
    margin-top: 7%;
    padding-top: 3%;
  }

.login {
    display: flex;
    justify-content: center;
    width: 380px;
}

.form {
    width: 400px;
    height: 40px;
    margin-bottom: 20px;
}

.form1 {
    width: 190px;
    height: 40px;
    margin-bottom: 20px;
    
}

.sign {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 400px;
    height: 60px;
    color: white;
    background-color: #4272d7;
}

</style>>


<template>
  <div class="container" >
    <div class="d-flex">
        <div class="container center">
            <div class="login">
                <h1 style="font-size: 30px;">Account Login</h1>
            </div>
            <div>
                <input class="form form-control" id="user" type="email" placeholder="Email" v-model="login.email" @keyup.enter="logon()">
                <input class="form form-control" id="pass" type="password" placeholder="Password" v-model="login.password" @keyup.enter="logon()">
            </div>
            <div>
                <p class="text-danger">{{checkin}}</p>
            </div>
            <div class="sign btn" @click="logon()">
                SIGN IN
            </div>
            <div>
                <h2 style="font-size: 20px;">Creat an account? <a href="/register">Sign up</a></h2>
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
            checkin: '',            
            login: {
                email: '',
                password: ''
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
        logon() {
            const datas = { 
                'email' : this.login.email,
                'password' : this.login.password
                }
            const options = {
            method: 'POST',
            url: process.env.VUE_APP_LOGIN,
            headers: { 'content-type': 'application/x-www-form-urlencoded' },
            data: qs.stringify(datas),
            };
            Axios(options)
            .then( res => {  
                if (res.data.result[0].token) {
                    localStorage.setItem('Token', res.data.result[0].token) 
                    this.$router.push('/')
                } else {
                    this.checkin = res.data.result
                    
                }
                
            })
            .catch( err => {
                console.log(err)
            })
        },
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
    width: 380px;
    height: 70px;
}

.sign {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 380px;
    height: 60px;
    color: white;
    background-color: #4272d7;
}

</style>>


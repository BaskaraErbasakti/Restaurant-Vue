<template>
    <div class="d-flex">
        <div class="d-flex col-1 flex-column align-items-center justify-content-start">
            <div v-if="(logon == 'true')" class="my-2 flex-column align-items-center btn" @click="logout">
                <img class="login" src="@/assets/logout.png" alt="login" >
                <p>Logout</p>
            </div>
            <div v-else class="my-2 flex-column align-items-center btn" @click="login">
                <img class="login" src="@/assets/login.png" alt="login" >
                <p>Login</p>
            </div>
            <div class="my-3 flex-column align-items-center">
                <img src="@/assets/fork.png" alt="fork">
                <p>Menu</p>
            </div>
            <div class="my-3 flex-column align-items-center">
                <img src="@/assets/clipboard.png" alt="clipboard">
                <p>History</p>
            </div>
            <div class="my-3 d-flex flex-column align-items-center btn" @click="edit">
                <img src="@/assets/add.png" alt="add">
                <p>Edit menu</p>
            </div>
            
            <!-- <div @click="coba()">
                <button>TOMBOL</button>
            </div> -->
        </div>
        <div class="col-7 d-flex flex-column bg-light">
            <div class="d-flex justify-content-center bg-light mt-3">
                <div class="d-flex">
                    <div class="d-flex align-items-center mx-2 px-3 py-0 border rounded-pill abu ">
                        <h4 class="p-0 m-0">Sort by</h4>
                    </div>
                    <div>
                        <div class="dropdown mx-2">
                            <button class="btn abu fontku rounded-pill dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                {{kondisi}}
                            </button>
                            <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                                <button class="dropdown-item" @click="sort(kondisi2)">{{kondisi2}}</button>
                                <button class="dropdown-item" @click="sort(kondisi3)">{{kondisi3}}</button>
                            </div>
                        </div>
                    </div>
                    <div>
                        <div class="dropdown mx-2">
                            <button class="btn abu fontku rounded-pill dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                {{condition}}
                            </button>
                            <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                                <button class="dropdown-item" @click="descend">{{condition2}}</button>
                            </div>
                        </div>
                    </div>
                </div> 
            </div>
            <div class="d-flex flex-wrap justify-content-center align-content-start p-2 bg-light">
                <div class="col keranjang" v-for="foods in filterprod" :key="foods.id" :id="foods.id">
                    <button class="btn klik" @click="(centang(foods.index), keranjang() )" >
                        <div :class="[foods.tick ? 'background tick' : 'background notick']" >
                            <img class="choice" src="@/assets/tick.png" alt="tick">
                        </div>       
                        <img :src="foods.images" :alt="foods.images">
                    </button>
                    <h3 class="m-0 p-0 light">{{foods.name}}</h3>
                    <h2 class="m-0 p-0 bold">Rp. {{foods.price}}</h2>   
                </div>
            </div>
        </div>

        <div class="col-4">
            <div class="col d-flex flex-column">
                <div :class="[empty === 'muncul' ? 'd-flex flex-column align-items-center justify-content-start' : 'd-none']">
                    <img src="@/assets/empty.png" alt="empty">
                    <h2 class="m-0 p-0">Your cart is empty</h2>
                    <p class="grey m-0 p-0">Please add some items from the menu</p>
                </div>
                <div :class="[item.right === 'hilang' ? 'd-none' : 'my-2 d-flex']" v-for="item in food" :key="item.id">
                    <div class="mr-4">
                        <img class="small-size" :src="item.images" :alt="item.name">
                    </div>
                    <div class="d-flex flex-column justify-content-between">
                        <div >
                            <h3 class="font-25 bold">{{item.name}}</h3>
                        </div>
                        <div class="d-flex">
                            <div class="d-flex mr-4">
                                <h2 class="added btn" @click="kurang(item.index)">-</h2>
                                <h2 class="numb d-flex justify-content-center align-items-center">{{item.count}}</h2>
                                <h2 class="added btn" @click="tambah(item.index)">+</h2>
                            </div>
                            <div class="d-flex justify-content-center align-items-end m-0">
                                <h1 class="font-25">Rp.</h1>
                                <h1 class="font-25 ml-2">{{ item.subprice }}</h1>
                            </div>     
                        </div>
                    </div>
                </div>
            </div>       
            <div :class="[bayar === 'hilang' ? 'd-none' : 'd-block margintop']">
                <div class="row d-flex ml-0 justify-content-between">
                    <div class="col p-0">
                        <h1>Total :</h1>
                    </div>
                    <div class="col text-right">
                        <h1 id="total-price1">Rp.{{jumlah}}*</h1>
                    </div>
                </div>
                <div class="row ml-0">
                    <h3 class="font-weight-light">*Belum termasuk ppn</h3>
                </div>
                <div class="row d-flex justify-content-center align-items-center py-2 my-2 mx-0 text-white btn bg-info">
                    <h1>Checkout</h1>
                </div>
                <div @click="cancel" class="pink row d-flex justify-content-center align-items-center py-2 my-2 mx-0 text-white btn">
                    <h1>Cancel</h1>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from "axios"

export default {
    name: "content",
    props: {
        total : Number,
        search : String,
        
    },
    data() {
        return {
            logon: 'false',
            searchs: this.search,
            condition: 'A-Z',
            condition2: 'Z-A',
            kondisi: 'id',
            kondisi2: 'name',
            kondisi3: 'price',
            jumlah: 0,
            empty: 'muncul',
            bayar: 'hilang',
            food: [],
        }
    },
    updated(){
        this.searchs = this.search
    },
    mounted(){
        axios.get(process.env.VUE_APP_PRODUK)
        .then(response => {
            this.food = response.data.result
            for (let i = 0; i < this.food.length; i++){
                this.food[i].index = i
                this.food[i].count = 1
                this.food[i].subprice = 0
                this.food[i].tick = false
                this.food[i].right = 'hilang'
            }
        })
        .catch( err => {
            console.log('gagal load')
            console.log(err)
            this.$router.go('/')
        })
        if(localStorage.Token){
            this.logon = 'true'
        } else {
            this.logon = 'false'
        }    
    },
    computed:{
        filterprod(){
            return this.food.filter(produk => {
                return produk.name.toLowerCase().match(this.searchs.toLowerCase())
            })
        }
    },
    methods: {
        logout(){
            this.logon = 'false'
            localStorage.removeItem("Token")
            this.$router.go('/')
        },
        login(){
            this.$router.push('/login')
        },
        edit(){
            if(localStorage.Token){
                this.$router.push('/edit')
            } else {
                alert('Please login first !')
            }     
        },
        centang(x) {
            if (this.food[x].tick == true){
                this.food[x].tick = false
                this.food[x].right = 'hilang'
                this.jumlah -= this.food[x].subprice
                this.$emit('tutup', this.food[x].count)
            } else {
                this.food[x].tick = true
                this.food[x].right = 'muncul'
                this.food[x].subprice = this.food[x].price
                this.food[x].count = 1
                this.jumlah += this.food[x].subprice
                this.$emit('nambah')
            }
        },
        keranjang(){
            let produk = this.food.map((nomor) => {return nomor.right})
            function chek(objeks) {
                return objeks === 'hilang'
            }
            if (produk.every(chek) == true){   
                this.empty = 'muncul'
                this.bayar = 'hilang'
                
            } else {   
                this.empty = 'hilang'
                this.bayar = 'muncul'
                
            }
               
        },
        kurang(y){
            if (this.food[y].count > 1) {
                this.food[y].count -= 1
                let a = this.food[y].count
                let b = this.food[y].price
                this.food[y].subprice = a * b   
                this.jumlah = this.jumlah - b
                this.$emit('kurang')
            }
            
        },
        tambah(y){
            this.food[y].count += 1
            let a = this.food[y].count
            let b = this.food[y].price
            this.food[y].subprice = a * b
            this.jumlah = this.jumlah + b
            this.$emit('nambah')
           
        },
        cancel(){
            let ok = confirm('Are you sure to cancel all items ?')
            if ( ok == true) {
                let semua = this.food
                semua.forEach(nomor => {
                    nomor.tick = false
                    nomor.right = 'hilang'  
                });
                this.empty = 'muncul'
                this.bayar = 'hilang'
                this.jumlah = 0
                this.$emit('reset')
            }
        },
        sort(x){
            if (this.condition == 'A-Z'){
                let a = this.food.sort((a, b) => (a[x] > b[x]) ? 1 : -1)
                this.filterprod = a
                this.kondisi = x
                if ( x == 'name') {
                    this.kondisi2 = 'price'
                    this.kondisi3 = 'id'
                } else if ( x == 'price') {
                    this.kondisi2 = 'id'
                    this.kondisi3 = 'name'
                }else {
                    this.kondisi2 = 'name'
                    this.kondisi3 = 'price'
                }
            } else {
                let a = this.food.sort((a, b) => (a.x > b.x) ? -1 : 1)
                this.filterprod = a
                this.kondisi = x
                if ( x == 'name') {
                    this.kondisi2 = 'price'
                    this.kondisi3 = 'id'
                } else if ( x == 'price') {
                    this.kondisi2 = 'id'
                    this.kondisi3 = 'name'
                }else {
                    this.kondisi2 = 'name'
                    this.kondisi3 = 'price'
                }
            }
        },

        descend() {
            if (this.kondisi == 'id'){
                if ( this.condition == 'A-Z'){
                    let a = this.food.sort((a, b) => (a.id > b.id) ? -1 : 1)
                    this.filterprod = a
                    this.condition = 'Z-A'
                    this.condition2 = 'A-Z'
                } else {
                    let a = this.food.sort((a, b) => (a.id > b.id) ? 1 : -1)
                    this.filterprod = a
                    this.condition = 'A-Z'
                    this.condition2 = 'Z-A'
                }
                
            } else if (this.kondisi == 'name') {
                if ( this.condition == 'A-Z'){
                    let a = this.food.sort((a, b) => (a.name > b.name) ? -1 : 1)
                    this.filterprod = a
                    this.condition = 'Z-A'
                    this.condition2 = 'A-Z'
                } else {
                    let a = this.food.sort((a, b) => (a.name > b.name) ? 1 : -1)
                    this.filterprod = a
                    this.condition = 'A-Z'
                    this.condition2 = 'Z-A'
                }
            } else {
                if ( this.condition == 'A-Z'){
                    let a = this.food.sort((a, b) => (a.price > b.price) ? -1 : 1)
                    this.filterprod = a
                    this.condition = 'Z-A'
                    this.condition2 = 'A-Z'
                } else {
                    let a = this.food.sort((a, b) => (a.price > b.price) ? 1 : -1)
                    this.filterprod = a
                    this.condition = 'A-Z'
                    this.condition2 = 'Z-A'
                }
            }
            
        },

        coba(){
            console.log(this.searchs)
        }
    },
}

</script>

<style scoped>
.fontku{
    font-size: 140%;
    font-weight: 500;
  
}
.abu {
    background-color: #e2e2e2;
}
.login{
    width: 50px;
    height: 50px;
}
.grey {
    color: #CECECE;
    font-size: 20px;
}
.added {
	background-color: rgba(130, 222, 58, 0.2);
	border: 1px solid #82DE3A;
	box-sizing: border-box;
	color: #82DE3A;
	font-size: 40px;
	width: 45px;
	text-align: center;
    font-weight: bold;
    margin: 0%;
}
.numb {
	background-color: #FFFFFF;
	color: #82DE3A;
	border: 1px solid #82DE3A;
	box-sizing: border-box;
	font-size: 25px;
	width: 45px;
	text-align: center;
    font-weight: bold;
    margin: 0%;
}
.small-size {
    width: 115px;
    height: 110px;
}
.pink {
    background-color: #f24f8a;
}
.background {
    position: absolute;
    width: 250px;
    height: 200px;
    background-color: rgba(0, 0, 0, 0.4);
}
.choice{
    position: absolute;
    top: 65px;
    left: 99px;
}
.keranjang {
    margin: 1.5% auto;
    width: 250px;
}
.klik {
    background-color: transparent;
    margin: 0%;
    padding: 0%;
    width: 250px;
    height: 200px;
}
.margintop{
    margin-top: 20%;
}

</style>
<template>
    <div class="container">


        <div class="background d-flex justify-content-between pr-2">
            <div class="d-flex align-items-center">
                <div @click="back" class="btn mr-4">
                    <img class="back" src="@/assets/back.png" alt="back">
                </div> 
                <div class="btn mx-4" @click="refresh">
                    <h2 class="text-white m-0 font-weight-bold">FOOD ITEMS DATABASE</h2>
                </div>
            </div>
            <div class="d-flex align-items-center">
                <div class="tinggi d-flex align-items-center border rounded-pill bg-white pl-2 mx-4">
                    <img class="cari" src="@/assets/cari.png" alt="search">
                    <input class="input border-0" type="text" placeholder="Search item" v-model="search">
                </div>
                <div class="hijau d-flex align-items-center border rounded-pill px-3 py-0 border-success btn" data-toggle="modal" data-target="#exampleModal">
                    <img class="plus" src="@/assets/plus.png" alt="plus">
                    <h3 class="text-white m-0">Add item</h3>
                </div>
            </div>  
        </div>

        
        
        <div class="biru">
            <table class="table">
            <thead>
                <tr>
                <th scope="col">Id</th>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
                <th scope="col">Images</th>
                <th scope="col">Category</th>
                <th scope="col">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="menus in filterprod" :key="menus.id">
                <th scope="row">{{menus.id}}</th>
                <td>{{menus.name}}</td>
                <td>{{menus.price}}</td>
                <td>{{menus.images}}</td>
                <td>{{menus.category}}</td>
                <td>
                    <img class="btn mx-2" src="@/assets/pencil.png" alt="pencil"> 
                    <img class="btn mx-2" src="@/assets/delete.png" @click="deleted(menus.name)" alt="delete">
                </td>
                </tr>
            </tbody>
            </table>
        </div>
        
        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">ADD ITEM</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        

                    <form>
                        <div class="form-group">
                            <label for="exampleInputName">Name</label>
                            <input type="text" class="form-control" id="exampleInputName" v-model="tambah.name">
                        </div>
                        <div class="form-group">
                            <label for="exampleInputPrice">Price</label>
                            <input type="text" class="form-control" id="exampleInputPrice" v-model="tambah.price">
                        </div>
                        <div class="form-group">
                            <label for="exampleInputImages">Images</label>
                            <input type="file" class="form-control" id="exampleInputImages" @change="fileselect">
                        </div>
                        <div class="form-group">
                            <label for="exampleInputCategory">Category</label>
                            <input type="text" class="form-control" id="exampleInputCategory" v-model="tambah.category">
                        </div>
                    </form>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary" data-dismiss="modal" @click="create()">Save changes</button>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import Axios from 'axios'
import qs from 'qs'
export default {
    name: 'edit',
    data(){
        return {
            search: '',
            condition: 'A-Z',
            condition2: 'Z-A',
            kondisi: 'id',
            kondisi2: 'name',
            kondisi3: 'price',
            menu: [],
            tambah: {
                name: '',
                price: 0,
                images: null,
                category: ''
            },
        }

    },
    computed:{
        filterprod(){
            return this.menu.filter(produk => {
                return produk.name.toLowerCase().match(this.search.toLowerCase())
            })
        },

    },
    mounted(){
        Axios.get(process.env.VUE_APP_PRODUK)
        .then(res => {
            if ( res.data.result == undefined){
                this.$router.go('/edit')
            } else {
                this.menu = res.data.result
            }
        })
        .catch( err => {
            console.log('GET AXIOS GAGAL')
            console.log(err)
        })
        if(!localStorage.Token){
            this.$router.push('/')
        }
    },
    methods: {
        fileselect(e){
            this.tambah.images = e.target.files[0]
        },
        create(){
            const data = new FormData()
            data.append('images', this.tambah.images)
            data.append('name', this.tambah.name)
            data.append('price', this.tambah.price)
            data.append('category', this.tambah.category)

            Axios.post(process.env.VUE_APP_PRODUK, data)
            .then(res => {
                setTimeout( () => {
                    alert(`${this.tambah.name} has been added`)
                    this.$router.go('/edit')}, 2500);
                return res
                
            })
            .catch( err => {
                alert('Data failed to add')
                console.log(err)
            })
        },
        deleted(x){
            let ok = confirm(`Are you sure to delete ${x} ?`)
            if (ok == true) {
                const datas = { 'name': x }
                const options = {
                method: 'DELETE',
                url: process.env.VUE_APP_PRODUK,
                headers: { 'content-type': 'application/x-www-form-urlencoded' },
                data: qs.stringify(datas),
                };
                Axios(options)
                .then(res => {
                    setTimeout( () => {
                        alert(`${x} has been deleted`)
                        this.$router.go('/edit')}, 2500);
                    return res
                })
                .catch( err => {
                    alert('Data failed to delete')
                    console.log(err)
                })
            }
        },
        refresh(){
            this.$router.go('/edit')
            
           
        },

        back(){
            this.$router.push('/')
        },
            
        
    }

}
</script>

<style scoped>
.background {
    background-color: #57cad5;
    margin-top: 5%;
    height: 70px;
}
.back {
    width: 50px;
    height: 50px;
}
.cari {
    width: 30px;
    height: 30px;
}
.input{
    margin-left: 2%;
    border-color: white;
    background: transparent;
    font-size: 20px;
    width: 180px;
}
.hijau {
    background-color: #1fb141;
}
.plus{
    width: 40px;
    height: 40px;
}
.tinggi {
    height: 40px;
}
.biru{
    background-color: #d9f8fc;
}
</style>
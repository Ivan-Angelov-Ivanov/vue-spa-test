<template>
    <div>
        <input type="text" class="text-field-filter" v-model="search" placeholder="Search...">
    </div>
    <div class="info-list">
        <span v-on:click="sorting('id')">ID</span>
        <span v-on:click="sorting('username')">Username</span>
        <span v-on:click="sorting('email')">Email</span>
        <span v-on:click="sorting('address')">Address</span>
        <span v-on:click="sorting('company')">Company</span>
    </div>
    <div v-bind:key="user.id" v-for="user in sortedAndFilteredData">
        <User v-bind:user="user" />
    </div>
    <div>
    </div>
    <div class="pagination-filter">
        <label>Items per page:</label>
        <input type="text" v-model="itemsPerPage">
    </div>
    <div>
        <button @click="prevPage">Prev</button> 
        |
        <button @click="nextPage">Next</button>
    </div>
</template>

<script>
import axios from 'axios';
import User from './User.vue';

export default {
    name: 'Stats',
    components: {
        User
    },
    data() {
        return {
            users: [],
            sort: {
                field: '',
                desc: true
            },
            search: '',
            itemsPerPage: 10,
            currentPage: 1
        }
    },
    methods: {
        sorting(field) {
            if(field === this.sort.field){
                this.sort.desc = !this.sort.desc;
            } else {
                this.sort.field = field;
                this.sort.desc = true;
            }
        },
        prevPage() {
            if(this.currentPage > 1){
                this.currentPage--;
            }
        },
        nextPage() {
            if(this.currentPage < this.totalPages){
                this.currentPage++;
            }
        }
    },
    computed: {
        totalPages(){
            return Math.ceil(this.users.length / this.itemsPerPage);
        },
        sortedAndFilteredData () {
            var users = this.users.slice((this.currentPage - 1)*this.itemsPerPage, this.currentPage*this.itemsPerPage).filter((user) => {
                return  user.id.toString().includes(this.search.toString()) ||
                        user.username.toLowerCase().includes(this.search.toLowerCase()) || 
                        user.email.toLowerCase().includes(this.search.toLowerCase()) ||
                        user.address.street.toLowerCase().includes(this.search.toLowerCase()) ||
                        user.company.name.toLowerCase().includes(this.search.toLowerCase())
            });

            if(!this.sort.field){
                return users;
            }

            return users.concat().sort((a,b)=>{
                if(this.sort.field === 'address'){
                    if(this.sort.desc){
                        return a[this.sort.field].street > b[this.sort.field].street ? -1:1        
                    }else{
                        return a[this.sort.field].street > b[this.sort.field].street ? 1:-1                  
                    }
                } else if(this.sort.field === 'company'){
                    if(this.sort.desc){
                        return a[this.sort.field].name > b[this.sort.field].name? -1:1        
                    }else{
                        return a[this.sort.field].name > b[this.sort.field].name ? 1:-1                  
                    }
                } else {
                    if(this.sort.desc){
                        return a[this.sort.field] > b[this.sort.field] ? -1:1        
                    }else{
                        return a[this.sort.field] > b[this.sort.field] ? 1:-1                  
                    }
                }
            })
        }
    },
    created() {
        axios.get('https://jsonplaceholder.typicode.com/users')
            .then(res => this.users = res.data)
            .catch(err => console.log(err));
    }
}
</script>

<style scoped>
    .info-list {
        display: grid;
        grid-gap: 10px;
        grid-template-columns: 30px repeat(4, 1fr);
        padding: 10px;
        margin-top: 5px;
        background-color: #a0a0a0;
        color: white;
        cursor: pointer;
    }

    .text-field-filter {
        width: 400px;
        margin: 5px;
        padding: 10px;
        border: 1px solid grey;
        border-radius: 10px;    
    }

    .pagination-filter {
        display: grid;
        grid-gap: 5px;
        grid-template-columns: 200px;
        justify-content: center;
        margin-top: 70px;
        padding: 5px;
    }

    .pagination-filter input[type=text] {
        width: 40px;
        margin: auto;
        padding: 10px;
        border: 1px solid grey;
        border-radius: 10px; 
        text-align: center;
    }

    input:focus, button:focus {
        outline: none;
    }

    button {
        width: 100px;
        margin: auto;
        padding: 10px;
        border: none;
        background-color: #333333;
        color: white;
        cursor: pointer;
    }

    button:hover {
        background-color: #a0a0a0;
    }



</style>
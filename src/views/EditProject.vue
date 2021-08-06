<template>
    <div>
<h1>Edit this project</h1>
<form @submit.prevent="handleSubmit">
            <label >Title</label>
            <input type="text" required v-model="title">
            <label for="">Details</label>
            <textarea required v-model="details"></textarea>
            <button>Add Project</button>
        </form>
    </div>
</template>

<script>
    export default {
        props:['id'],
        data() {
            return {
                title: '',
                details:'',
                uri:'http://localhost:3000/projects/'+this.id
            }
        },
        mounted () {
            fetch(this.uri)
            .then(res=>res.json())
            .then(data=>{
            this.title=data.title;
            this.details=data.details;
            })
        },
        methods: {
            handleSubmit() {
                fetch(this.uri,{
                    method:'PATCH',
                    headers:{'Content-Type':'application/json'},
                    body:JSON.stringify({title:this.title,deatils:this.details})
                })
                .then(()=>{
                    this.$router.push('/')
                })
                .catch(err=>console.log(err))
                
            }
        },
    }
</script>

<style lang="css" >
form{
        background: white;
        padding: 20px;
        border-radius: 10px;

    }
    label{
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0;
    }
    input{
        padding: 10px;
        border: 0;
        border-bottom: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
    }
    textarea{
        border: 1px solid #ddd;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        height: 100px;
    }
    form button{
        display: block;
        margin: 20px auto 0;
        background: #00ce89;
        color: white;
        padding: 10px;
        border: 0;
        border-radius: 6px;
        font-size: 16px;
        cursor: pointer;

    }
</style>
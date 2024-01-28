<template>
<div class="project" :class="{complete:project.complete}">
    <div class="flexing">
        <div>
            <h3 @click="showDetail=!showDetail">{{project.title}}</h3>
        </div>
        <div>
            <span class="material-symbols-outlined" @click="deleteProject">
            delete
            </span>
            <span class="material-symbols-outlined">
            edit
            </span>
            <span class="material-symbols-outlined" @click="completeProject">
            done
            </span>
        </div>
    </div>
    <p v-if="showDetail">{{project.detail}}</p>
</div>
</template>

<script>
export default {
 props:['project'],
 data(){
    return {
        showDetail: false,
        api: "http://localhost:3000/projects/"
    }
 },
 methods: {
    deleteProject() {
        let deleteRoute = this.api+this.project.id;
        fetch(deleteRoute,{method:"DELETE"})
        .then(()=>{
            this.$emit("delete",this.project.id)
        })
        .catch((err)=>{
            console.log(err.message())
        })
    },
    completeProject() {
        let updateRoute = this.api+this.project.id;
        fetch(updateRoute,{
            method:"PATCH",
            headers:{
                "Content-Type":"application/json"
            },
            body:JSON.stringify(
                {
                    complete:!this.project.complete
                }
            )
            })
        .then(()=>{
            this.$emit("complete",this.project.id)
        })
        .catch((err)=>{
            console.log(err)
        })
    }
 }
}
</script>

<style>
    .project {
        padding: 20px;
        margin: 10px;
        background-color: #f2f2f2; 
        border-left: 8px solid crimson;
        border-radius: 10px;
        }
    h3 {
        color: indigo;
        cursor: pointer;

    }
    .flexing {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    span {
        margin-left: 6px;
        cursor: pointer;
        padding: 8px 10px;
        margin-left: 10px;
        border-radius: 13px;
    }
    span:nth-child(1) {
        color: crimson;    
    }
    span:nth-child(2) {
        color: blue;    
    }
    span:nth-child(3) {
        color: green;    
    }
    .complete {
        border-left-color: green;
    }
</style>
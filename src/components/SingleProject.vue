<template>
    <div class="projects" :class="{complete : project.complete}">
        <div class="actions">
            <h3 v-on:click="toggleDetails">{{ project.title }}</h3>
            <div class="icons">
                <router-link :to="{ name: 'EditProject', params: { id: project.id } } ">
                    <span class="material-icons">edit</span>
                </router-link>
                <span @click="deleteProject" class="material-icons">delete</span>
                <span @click="toggleComplete" class="material-icons tick">done</span>
            </div>
        </div>
        <div v-if="showDetails" class="details">
            <p>{{ project.details }}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleProject',
    props: ['project'],
    data(){
        return{
            showDetails : false,
            url : 'https://62bd66d0c5ad14c110bdc696.mockapi.io/projects/' + this.project.id
        }
    },
    methods: {
        toggleDetails(){
            this.showDetails = !this.showDetails
        },
        deleteProject(){
            fetch(this.url, {method: 'DELETE'})
            .then(() => this.$emit('delete', this.project.id))
            .catch(err => console.log(err.message))
        },
        toggleComplete(){
            fetch(this.url, {
                method: 'PUT',
                headers: { 'Content-Type' : 'application/json'},
                body: JSON.stringify({ complete : !this.project.complete })
            }).then(() => this.$emit('complete', this.project.id))
            .catch(err => console.log(err.message))
        }
    }
}
</script>

<style>
    .projects{
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.1);
        border-left: 4px solid #e90074;
    }
    h3{
        cursor: pointer;
    }
    .actions{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .material-icons{
        font-size: 24px;
        cursor: pointer;
        margin-left: 10px;
        color: #bbb;
    }
    .material-icons:hover{
        color: #777;
    }
    .projects.complete{
        border-left: 4px solid #00ce89;
    }
    .projects.complete .tick{
        color: #00ce89;
    }
</style>
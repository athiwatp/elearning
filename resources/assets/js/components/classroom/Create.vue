<template>
    <div class="container">
        <button type="button" @click="back" class="btn btn-default btn-back">Back</button>
        <div class="card">
        <div class="form-group">
            <label for="name">Classroom name</label>
            <input type="text" class="form-control" id="name" placeholder="Classroom name" v-model="classroom.name">
            <p class="text-danger"
                v-for="error in classroom_errors.name"
                v-if="classroom_errors.name"
            >{{ error }}</p>
        </div>
        <div class="form-group">
            <label for="description">description</label>
            <textarea class="form-control" id="description" rows="3" v-model="classroom.description"></textarea>
        </div>
        <button type="submit" class="btn btn-yel pull-right" @click="create">Submit</button>
        <div class="clearfix"></div>
    </div>
</div>
</template>

<script>
    export default {
        data(){
            return {
                classroom : {
                    name : '',
                    description: ''
                },
                classroom_errors: []
            }
        },
        methods : {
            create(){
                var token = this.$auth.getToken()
                var data = {
                    'name': this.classroom.name,
                    'description': this.classroom.description
                }

                axios.post('api/classroom', data, {
                    headers:{
                        Authorization: 'Bearer ' + token
                    }
                })
                .then(response=>{
                    var classroom_id = response.data.id
                    this.$router.push('/classroom/' + classroom_id)
                })
                .catch(error => {
                    this.classroom_errors = error.response.data.errors
                })
            },back(){
                this.$router.go(-1)
            }
        }
    }
</script>

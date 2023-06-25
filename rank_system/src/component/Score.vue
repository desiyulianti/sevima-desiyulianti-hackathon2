<template>
        <div>
                 <div class="container-fluid">

                        <!-- Page Heading -->
                                <h1 class="h3 mb-1 font-weight-bold text-black">Data Score SMK Bina Indonesia</h1>
                                <p class="mb-8">The following is data about SMK Bina Indonesia.</p>
                                

                        <div class="card shadow mb-4">
                        <div class="card-header py-3">
                             <h5 class="m-0 font-weight-bold text-pink">Data Score</h5>
                           <!-- <a class="btn btn-primary float-right" href="/#/AddGrade" ><i class="bi bi-clipboard2-plus-fill"></i></a> -->
                        </div>
                        <div class="card-body">
                            <div class="table-responsive">
                                <!-- <button @click="Add()" class="btn btn-primary btn-icon-split" data-bs-toggle="modal" data-bs-target="#score_modal"><span class="icon text-white-50"><i class="fas fa-plus" data-bs-dismiss="modal"></i></span> <span class="text">Add Score</span></button> -->
                                  <hr> 
                                <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                                    <thead class="text-center">
                                        <tr>
                                            <th>NO</th>
                                            <th>LESSON NAME</th>
                                            <th>SCORE AMOUNT</th>
                                            <th>ACTION</th>
                                        </tr>
                                    </thead>
                                    <tbody class="text-center">
                                         <tr v-for="(la, index) in scores" :key="index">
                                <td>{{ index+1 }}</td>
                                <td>{{ la.lesson_name }}</td>
                                <td>{{ la.score_amount}}</td>
                                <td>
                                        <!-- <a class="btn btn-success" :href="'/#/UpdateGrade/' + grade.class_id" ><i class="fas fa-pencil-alt fa-fw"></i></a>
                         -->
                                        <button class="btn  btn-circle btn-success" @click="Edit(la)" data-bs-toggle="modal" data-bs-target="#grade_modal"><i class="fas fa-pencil-alt fa-fw"></i></button>
                                        |
                                        <button class="btn  btn-circle btn-danger" @click="Delete(la.score_id)"><i class="fas fa-trash-alt fa-fw"></i></button>
                                </td>  
                             </tr> 
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                 </div>

                 <!-- Modal Grade-->
                        <div class="modal fade" id="grade_modal" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                                <div class="modal-dialog">
                                        <div class="modal-content">
                                        <div class="modal-header">
                                                <h5 class="modal-title text-black" id="staticBackdropLabel">Data Grade</h5>
                                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                                        </div>
                                        <div class="modal-body">
                                        <div class="mb-3">
                                                <label for="lesson_name" class="form-label">Lesson Name</label>
                                                <input type="text" class="form-control" id="class_name" v-model="lesson_name">
                                        </div> 

                                        <div class="mb-3">
                                                <label for="score_amount" class="form-label">Major</label>
                                                <input type="text" class="form-control" id="score_amount" v-model="score_amount">
                                        </div>
                                        </div>
                                        <div class="modal-footer">
                                                <button type="button" class="btn btn-danger" data-bs-dismiss="modal">Cancel</button>
                                                <button type="button" class="btn btn-primary" @click="Save()" data-bs-dismiss="modal">Submit</button>
                                        </div>
                                        </div>
                                </div>

                        </div> 
        </div>
</template>
<script>
module.exports = {
        //state
        data : function(){
                return {
                         score_id: "",
                         lesson_name: "",
                         score_amount: "",
                         action: "",
                         scores: [],
                         
                              }
        },
        methods: {
                getData: function(){
                        //token
               let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }
                axios.get(api_url + "/Score", config)
                .then( response => {
                  this.scores = response.data;   
                })
                
                },
                Add: function(){
                    this.score_id = ""
                    this.lesson_name = ""
                    this.score_amount = ""
                    this.action = "insert"
                
                },
                Edit: function(la){
                    this.score_id = la.score_id
                    this.lesson_name = la.lesson_name
                    this.score_amount = la.score_amount
                    this.action = "update"

                },
                
                Save: function(){
                //maping token
                let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }

                //maping data 
                let form = {
                        //harus sama dengan backend
                         'lesson_name': this.lesson_name,
                         'score_amount': this.score_amount,


                }

                if(this.action === 'insert'){ //POST

                        axios.post(api_url + '/Score', form, config)
                        .then( response => {
                        Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'OK'
                    })

                                 this.getData()
                        })

                } else { //PUT

                        axios.put(api_url + '/Score/' + this.score_id, form, config)
                        .then( response => {
                        Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'OK'
                    })
                        })

                }
               
               this.getData()
                },
                Delete: function(class_id){
                //maping token 
                    let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }

                        Swal.fire({
                                title: 'Delete Data Score',
                                text: 'Do you want to delete this data?',
                                icon: 'warning',
                                showDenyButton: true,
                                showCancelButton: false,
                                confirmButtonText: 'Yes',
                                denyButtonText: `No`,
                                }).then((result) => {
                        if (result.isConfirmed) {
                                axios.delete(api_url + '/Score/' + score_id, config)
                        .then( response => {

                                Swal.fire({
                                title: 'Success!',
                                text: response.data.message,
                                icon: 'success',
                                confirmButtonText: 'OK'
                        })

                                this.getData()
                        })
     
                        } else  {
                                Swal.fire({
                                title: 'Cancel !',
                                text: 'Data is not deleted',
                                icon: 'error',
                                confirmButtonText: 'OK'
                        })
                        }
                })

            }    
        },
        mounted(){
                this.getData()
        },
}
</script>
<template>
     <div>
           <div class="container-fluid">

                 <!-- Page Heading -->
                    <h1 class="h3 mb-1 font-weight-bold text-black">Data Student SMK Bina Indonesia</h1>
                    <p class="mb-8">The following is data about SMK Bina Indonesia student.</p>
                    

                    <!-- DataTales Example -->
                    <div class="card shadow mb-4">
                        <div class="card-header py-3">
                            <h5 class="m-0 font-weight-bold text-pink">Data Student</h5>
                           <!-- <a class="btn btn-primary float-right" href="/#/AddStudent" ><i class="bi bi-person-plus-fill"></i></a> -->
                        </div>
                        <div class="card-body">
                            <div class="table-responsive">
                                 <button @click="Add()" class="btn btn-primary btn-icon-split" data-bs-toggle="modal" data-bs-target="#student_modal"><span class="icon text-white-50"><i class="fas fa-plus" data-bs-dismiss="modal"></i></span> <span class="text">Add Student</span></button>
                                  <hr> 
                                <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                                    <thead class="text-center">
                                        <tr>
                                        <th>NO</th>
                                        <th>STUDENT ID</th>
                                        <th>STUDENT NAME</th>
                                        <th>GENDER</th>
                                         <th>ADDRESS</th>
                                        <th>CLASS</th>
                                        <th>ACTION</th>
                                        </tr>
                                </thead>
                                <tbody class="text-center ">
                                    <tr v-for="(li, index) in student_list" :key="index"> 
                                       <td>{{ index+1 }}</td>
                                       <td>{{li.student_id}}</td>
                                       <td>{{li.student_name}}</td>
                                         <td>
                                        <span class="badge bg-info" v-if="li.gender === 'L'">Laki-Laki</span>
                                        <span class="badge bg-warning" v-if="li.gender === 'P'">Perempuan</span>
                                        </td>
                                         <td>{{li.address}}</td>
                                       <td>{{ li.class_name }}</td>
                                       <td>

                                                <!-- <a class="btn btn-success" :href="'/#/UpdateStudent/' + li.nisn" ><i class="fas fa-pencil-alt fa-fw"></i></a>
-->
                                             <button class="btn btn-circle btn-success" @click="Edit(li)" data-bs-toggle="modal" data-bs-target="#student_modal"><i class="fas fa-pencil-alt fa-fw"></i></button>
                                              |
                                             <button class="btn btn-circle btn-danger" @click="Delete(li.student_id)"><i class="fas fa-trash-alt fa-fw"></i></button>
                                       </td>  
                                    </tr> 
                              </tbody>
                              </table>
                            </div>
                        </div>
                    </div>

                </div>

               <!-- Modal Student-->
        <div class="modal fade" id="student_modal" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                <div class="modal-dialog">
                        <div class="modal-content">
                        <div class="modal-header">
                                <h5 class="modal-title text-black" id="staticBackdropLabel">Data Student</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                                <form class="row g-3">
                                <div class="col-md-20">
                                        <label for="student_name" class="form-label">Student Name</label>
                                        <input type="text" class="form-control" id="student_name" v-model="student_name">
                                </div>
                                <div class="mb-3">
                                   <label for="gender" class="form-label">Gender</label>
                                   <select class="form-control" v-model="gender">
                                        <option value="" disabled>-- Choose Gender --</option>
                                        <option value="L">Laki-Laki</option>
                                        <option value="P">Perempuan</option>
                                   </select>
                                </div>
                                <div class="col-md-20">
                                        <label for="address" class="form-label">Address</label>
                                        <input type="text" class="form-control" id="address" v-model="address">
                                </div>
                                 <div class="col-md-20">
                                 <label for="class" class="form-label">Class</label>
                                        <select class="form-control" v-model="class_id">
                                                <option value="" disabled>-- Choose Class --</option>
                                                <option v-for="a in list_class" :key="a" v-bind:value="a.class_id">{{ a.class_name }} - {{ a.major }}</option>
                                        </select>
                                 </div>
                                </form>
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
                        student_id: "",
                        student_name: "",
                        gender: "",
                        address: "",
                        action: "",
                        class_id: "",
                        student_list: [],
                        list_class: []
                }
        },
        methods: {
                getData: function(){
                        //token
               let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }
                axios.get(api_url + "/Student", config)
                .then( response => {
                  this.student_list = response.data;   
                })
                

                },
                getClass: function(){
                        //token
               let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }
                axios.get(api_url + "/Grade", config)
                .then( response => {
                  this.list_class = response.data;   
                })

                },
                Add: function(){
                    this.student_id = ""
                    this.student_name = ""
                    this.gender = ""
                    this.address = ""
                    this.class_id = ""
                    this.action = "insert"

                    this.getClass()
                
                },
                 Edit: function(li){
                      this.student_id = li.student_id
                      this.student_name = li.student_name
                      this.gender= li.gender
                      this.address = li.address
                      this.class_id = li.class_id
                      this.action = li.action = "update" 

                      this.getClass()

                },
                Save: function(){
                //maping token
                let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }

                //maping data 
                let form = {
                        //harus sama dengan backend
                        
                        'student_name': this.student_name,
                        'gender': this.gender,
                        'address': this.address,
                        'class_id': this.class_id,

                }

                if(this.action === 'insert'){ //POST

                        axios.post(api_url + '/Student', form, config)
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

                        axios.put(api_url + '/Student/' + this.student_id, form, config)
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

                   Delete: function(student_id){
                //maping token 
                    let config = {
                    headers: { "Authorization": "Bearer " + this.$cookies.get("Authorization")}
                }

                        Swal.fire({
                                title: 'Delete Data Student',
                                text: 'Do you want to delete this data?',
                                icon: 'warning',
                                showDenyButton: true,
                                showCancelButton: false,
                                confirmButtonText: 'Yes',
                                denyButtonText: `No`,
                                }).then((result) => {
                        if (result.isConfirmed) {
                                axios.delete(api_url + '/Student/' + student_id, config)
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
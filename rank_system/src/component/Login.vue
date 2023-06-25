<template>
  <div class="content-center">
    <div class="container">
        <div class="card o-hidden border-0 shadow-lg my-5">
            <div class="card-body p-0">
                <!-- Nested Row within Card Body -->
                <div class="row">
                    <div class="col-lg-5 d-none d-lg-block bg-register-image"></div>
                    <div class="col-lg-7">
                        <div class="p-5">
                            <div class="text-center">
                                <h1 class="text- text-bold">Login</h1>
                            </div>
                             <div class="p-1">
                            <p>{{ message }}</p>
                            <br>
                            </div>
                            <form class="user">
                                <div class="form-group">
                                    <input type="Email" class="form-control form-control-user" v-model="email" id="exampleInputEmail"
                                        placeholder="Email...">
                                </div>
                                <br>
                                <div class="form-group">
                                        <input type="password" class="form-control form-control-user" v-model="kataSandi"
                                            id="exampleInputPassword" placeholder="Password">
                                  
                                </div>
                                <br>
                                <a class="btn btn-primary btn-user btn-block" href="/#/home" @click="Login">
                                    Login
                                </a>
                                <br>
                                <!-- <hr> -->
                                <!-- <a href="#" class="btn btn-google btn-user btn-block">
                                    <i class="fab fa-google fa-fw"></i> Register with Google
                                </a>
                                <a href="#" class="btn btn-facebook btn-user btn-block">
                                    <i class="fab fa-facebook-f fa-fw"></i> Register with Facebook
                                </a> -->
                            </form>
                            <hr>
                            <br>
                            <div class="text-center">
                                <a class="small" href="#">Forgot Password?</a>
                            </div>
                            <div class="text-center">
                                <a class="small" href="#">Already have an account? Login!</a>
                            </div>
                            <br>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
module.exports = {
  //state
  data: function () {
    return {
      email: "",
      kataSandi: "",
      message: "",
    };
  },
  methods: {
    Login: function () {
      //
      this.message = "Please waiting ...";
      //mapping data post
      let data_login = {
        //sesuai API    //sesuai state
        "email":        this.email,
        "password":     this.kataSandi
      }

      //post data
      axios.post(api_url + "/login", data_login).then((response) => {
        //kondisi status login
        this.message = response.data.message;

        if (response.data.status === 1) {
          //jika berhasil login menyimpan token ke dalam cookies
          //cek dulu apakah ada data token sebelumnya
          if (this.$cookies.isKey("Authorization")) {
            //jika ada, maka di hapus
            this.$cookies.remove("Authorization");
          }

          this.$cookies.set("Authorization", response.data.token);
          this.mycomponent = "apps";
          location.reload();
        } else {
          this.mycomponent = "login";
        }
      });
    },
  },
};
</script>
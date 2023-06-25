<template>
  <div>
    <!-- Page Wrapper -->
    <div id="wrapper">
      <!-- Sidebar -->
     <ul class="navbar-nav bg-gradient-purple sidebar sidebar-dark accordion" id="accordionSidebar">
        <!-- Sidebar - Brand -->
        <a class="sidebar-brand d-flex align-items-center justify-content-center">
          <div class="sidebar-brand-icon rotate-n-15">
          <i class="bi bi-mortarboard-fill"></i>
          </div>
          <div class="sidebar-brand-text mx-3">MyRank</div>
        </a>

        <!-- Divider -->
        <hr class="sidebar-divider my-0" />

        <!-- Nav Item - Dashboard -->
        <li class="nav-item active">
          <a class="nav-link" href="/#/home">
            <i class="bi bi-house-door-fill"></i>
            <span>Dashboard</span>
          </a>
        </li>

        <!-- Divider -->
        <hr class="sidebar-divider" />

        <!-- Heading -->
        <div class="sidebar-heading">DATA USER</div>

        <!-- Nav Item - Student Collapse Menu -->
        <li class="nav-item">
            <router-link to="/student" class="nav-link">
            <i class="bi bi-person-fill"></i>
            <span>Student</span>
            </router-link>
        </li>

        <!-- Nav Item - Grade Collapse Menu -->
        <li class="nav-item">
            <router-link to="/grade" class="nav-link">
            <i class="bi bi-mortarboard-fill"></i>
            <span>Grade</span>
          </router-link>
        </li>

        <!-- Nav Item -Spp Menu -->
       <li class="nav-item">
          <router-link to="/score" class="nav-link">
            <i class="bi bi-layout-text-sidebar"></i>
            <span>Score</span>
          </router-link>
        </li>

         <!-- Divider -->
        <hr class="sidebar-divider d-none d-md-block" />

        <!-- Heading -->
        <div class="sidebar-heading">OTHERS</div>

        <!-- Nav Item - Logout -->
        <li class="nav-item">
           <router-link to="/logout" class="nav-link">
            <i class="bi bi-box-arrow-right"></i>
            <span>Logout</span>
        </li>
      </ul>
      <!-- End of Sidebar -->

      <!-- Content Wrapper -->
      <div id="content-wrapper" class="d-flex flex-column">
        <!-- Main Content -->
        <div id="content">
          <!-- Topbar -->
          <nav class=" navbar navbar-expand navbar-light bg-white topbar mb-4 static-top shadow" >
            <!-- Sidebar Toggle (Topbar) -->
            <button
              id="sidebarToggleTop"
              class="btn btn-link d-md-none rounded-circle mr-3">
              <i class="fa fa-bars"></i>
            </button>

            <!-- Topbar Navbar -->
            <ul class="navbar-nav ml-auto">
              <!-- Nav Item - Search Dropdown (Visible Only XS) -->
              <li class="nav-item dropdown no-arrow d-sm-none">
                <a
                  class="nav-link dropdown-toggle"
                  href="#"
                  id="searchDropdown"
                  role="button"
                  data-toggle="dropdown"
                  aria-haspopup="true"
                  aria-expanded="false" >
                  <i class="fas fa-search fa-fw"></i>
                </a>
              </li>
              <div class="topbar-divider d-none d-sm-block"></div>
              <!-- Nav Item - User Information -->
              <li class="nav-item dropdown no-arrow">
                <a
                  class="nav-link dropdown-toggle"
                  href="#"
                  id="userDropdown"
                  role="button"
                  data-toggle="dropdown"
                  aria-haspopup="true"
                  aria-expanded="false" >
                  <span class="mr-2 d-none d-lg-inline text-gray-600 small">Admin</span>
                  <img class="img-profile rounded-circle" src="img/undraw_profile.svg" />
                </a>
              </li>
            </ul>
          </nav>
           <main>
          <!-- Ini nanti Dinamis -->
          <router-view></router-view>
        </main>
          </div>
          <!-- End of Topbar --> 
        <!-- Footer -->
        <footer class="sticky-footer bg-white">
          <div class="container my-auto">
            <div class="copyright text-center my-auto">
              <span>Copyright &copy; Your Website 2021</span>
            </div>
          </div>
        </footer>
        <!-- End of Footer -->
      </div>
      <!-- End of Content Wrapper -->
    </div>
    <!-- End of Page Wrapper -->

    <!-- Scroll to Top Button-->
    <a class="scroll-to-top rounded" href="#page-top">
      <i class="fas fa-angle-up"></i>
    </a>

   
  </div>
</template>

<script>
module.exports = {
  methods: {
    Logout: function () {
      //menghapus cookies token
      this.$cookies.remove("Authorization");
      location.reload();
    },
  },
};
</script>
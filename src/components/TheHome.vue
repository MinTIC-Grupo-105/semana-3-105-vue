<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container">
        <a class="navbar-brand" href="#">
          <img
            src="bootstrap-solid.svg"
            width="30"
            height="30"
            class="d-inline-block align-top"
            alt=""
          />
          Marketing Digital
        </a>
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav ml-auto">
            <li class="nav-item">
              <!-- <a class="nav-link" data-toggle="modal" data-target="#loginModal" href="#">Ingreso</a> -->
              <button
                class="btn btn-outline-success my-2 my-sm-0"
                type="submit"
                @click.prevent="logOut"
              >
                Salir
              </button>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="container">
      <div class="span3 well">
        <center>
          <a href="#aboutModal" data-toggle="modal" data-target="#myModal"
            ><img
              src="https://encrypted-tbn2.gstatic.com/images?q=tbn:ANd9GcRbezqZpEuwGSvitKy3wrwnth5kysKdRqBW54cAszm_wiutku3R"
              name="aboutme"
              width="140"
              height="140"
              class="rounded-circle"
          /></a>
          <h3>{{ user.nombre }}</h3>
          <em>{{ user.email }}</em>
        </center>
      </div>
    </div>
  </div>
</template>

<script>
$(document).ready(function () {
  $("#loginModal").modal("hide");
});

export default {
  data() {
    return {
      user: {},
    };
  },

  methods: {
    getUserDetails() {
      let user = localStorage.getItem("user");
      let token = localStorage.getItem("jwt");

      if (token) {
        this.user = JSON.parse(user);
      }
    },
    logOut() {
      localStorage.removeItem("jwt");
      localStorage.removeItem("user");
      this.$router.push("/");
    },
  },
  created() {
    this.getUserDetails();
  },
};
</script>
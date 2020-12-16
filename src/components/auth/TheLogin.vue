<template>
  <div class="" mt="5">
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
      <a class="navbar-brand" href="#">
        <img src="bootstrap-solid.svg" width="30" height="30" class="d-inline-block align-top" alt="">
        Marketing Digital
      </a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <!-- <a class="nav-link" data-toggle="modal" data-target="#loginModal" href="#">Ingreso</a> -->
            <button
            type="button"
            class="btn btn-info btn-round"
            data-toggle="modal"
            data-target="#loginModal"
          >
            Login
          </button>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Registro</a>
          </li>
        </ul>

      </div>
    </div>
  </nav>






    <div
      class="modal fade"
      id="loginModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header border-bottom-0">
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">×</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="form-title text-center">
              <h4>Ingreso</h4>
            </div>
            <div class="d-flex flex-column text-center">
              <form>
                <div class="form-group">
                  <input
                    type="email"
                    class="form-control"
                    placeholder="Your email address..."
                    id="email_address"
                    name="email-address"
                    v-model="login.email"
                    aria-describedby="emailHelp"
                    required
                    autofocus
                  />
                </div>
                <div class="form-group">
                  <input
                    type="password"
                    class="form-control"
                    placeholder="Your password..."
                    id="password"
                    name="password"
                    required
                    v-model="login.password"
                  />
                </div>
                <button
                  class="btn btn-info btn-block btn-round"
                  @click.prevent="loginUser"
                  type="submit"
                >
                  Entrar
                </button>
              </form>
              <div class="text-center text-muted delimiter">
                o usa una red social
              </div>
              <div class="d-flex justify-content-center social-buttons">
                <button
                  type="button"
                  class="btn btn-secondary btn-round"
                  data-toggle="tooltip"
                  data-placement="top"
                  title="Twitter"
                >
                  <i class="fab fa-twitter"></i>
                </button>
                <button
                  type="button"
                  class="btn btn-secondary btn-round"
                  data-toggle="tooltip"
                  data-placement="top"
                  title="Facebook"
                >
                  <i class="fab fa-facebook"></i>
                </button>
                <button
                  type="button"
                  class="btn btn-secondary btn-round"
                  data-toggle="tooltip"
                  data-placement="top"
                  title="Linkedin"
                >
                  <i class="fab fa-linkedin"></i>
                </button>
              </div>
            </div>
          </div>
          <div class="modal-footer d-flex justify-content-center">
            <div class="signup-section">
              ¿No eres usuario todavía?
              <a href="#a" class="text-info"> Registrate</a>.
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import swal from "sweetalert";

$(document).ready(function () {
  $("#loginModal").modal("show");
  console.log("Activar modal");
});

export default {
  name: "TheLogin",
  data() {
    return {
      login: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
    async loginUser() {
      try {
        let response = await this.$http.post("/api/usuario/login", this.login);
        // console.log(response.data)
        let token = response.data.tokenReturn;
        let user = response.data.user;

        localStorage.setItem("jwt", token);
        localStorage.setItem("user", JSON.stringify(user));
        if (token) {
          swal("Exito!!!", "Logi correcto!", "success");
          $("#loginModal").modal("hide");
          this.$router.push("/home");
        }
      } catch {
        swal("Oops!", "Datos incorrectos", "error");

        // console.log(err.response)
      }
    },
  },
};
</script>
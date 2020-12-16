<template>
  <div class="container" mt="5">
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
  console.log("Hola");
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
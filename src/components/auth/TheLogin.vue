<template>
  <div class="container" mt="5">
    <div class="row">
      <div class="col"></div>
      <div class="col"></div>
    </div>
    <main class="login-form">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-md-8">
            <div class="card">
              <div class="card-header">Ingreso</div>
              <div class="card-body">
                <form action="" method="">
                  <div class="form-group row">
                    <label
                      for="email_address"
                      class="col-md-4 col-form-label text-md-right"
                      >Direccion de email</label
                    >
                    <div class="col-md-6">
                      <input
                        id="email_address"
                        name="email-address"
                        v-model="login.email"
                        type="email"
                        class="form-control"
                        aria-describedby="emailHelp"
                        required
                        autofocus
                      />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label
                      for="password"
                      class="col-md-4 col-form-label text-md-right"
                      >Contraseña</label
                    >
                    <div class="col-md-6">
                      <input
                        type="password"
                        id="password"
                        class="form-control"
                        name="password"
                        required
                        v-model="login.password"
                      />
                    </div>
                  </div>

                  <div class="form-group row">
                    <div class="col-md-6 offset-md-4">
                      <div class="checkbox">
                        <label>
                          <input type="checkbox" name="remember" /> Recordarme
                        </label>
                      </div>
                    </div>
                  </div>
                  <div class="col-md-6 offset-md-4">
                    <button
                      @click.prevent="loginUser"
                      type="submit"
                      class="btn btn-primary"
                    >
                      Ingresar
                    </button>
                    <a href="#" class="btn btn-link"> Olvido su cntraseña? </a>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import swal from "sweetalert";

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
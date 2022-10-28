<template>
  <q-page class="flex">
    <div class="q-pa-md head">
      <div class="q-pa-md row justify-between">
        <div
          class="
            col-8
            text-caveat text-h1 text-weight-bold text-left text-primario
            spacio_logo
          "
          style="margin-top: -20px"
        >
          Logo
        </div>
        <div class="col-4">
          <div class="row justify-center q-mt-md q-ml-md">
            <q-btn
              size="1.3rem"
              outline
              round
              style="color: #0096d9"
              icon="fa-brands fa-facebook-f"
            />
            <q-btn
              size="1.3rem"
              outline
              round
              style="margin-left: 20px; color: #b72c2c"
              icon="fa-brands fa-instagram"
            />
            <q-btn
              size="1.3rem"
              outline
              round
              style="margin-left: 20px; color: #d8ad3d"
              icon="fa-brands fa-youtube"
            />
          </div>
        </div>
      </div>
      <div
        class="
          text-caveat text-h1 text-weight-bold text-left text-primario
          spacio
        "
        style="width: 50%"
      >
        El secreto<br />
        de tu cocina
      </div>
      <div class="contenedor">
        <!-- <q-img
      class="foot"
          src="BRUSH-3 2.png"
          contain
          
        /> -->
        <img src="BRUSH-3 2.png" />
      </div>
    </div>
    <div class="row justify-center" style="width: 100%; margin-top: 60px">
      <div class="col-12">
        <div
          class="text-caveat text-h2 text-weight-bold text-center text-primario"
        >
          Nuestros articulos
        </div>
        <img
          src="BRUSH-3 2.png"
          style="
            width: 40%;
            height: 20%;
            position: relative;
            left: 30%;
            top: -20px;
            opacity: 0.3;
          "
        />
      </div>
    </div>
    <div class="q-pa-md q-ml-md row justify-center" style="width: 100%">
      <div class="col-4" style="width: 30%">
        <q-list style="margin-right: 10%" class="text-opensans">
          <q-item clickable v-ripple>
            <q-item-section style="color: #d8ad3d">TODOS</q-item-section>
            <q-item-section side>
              <q-icon name="fa-solid fa-arrow-right" style="color: #d8ad3d" />
            </q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section>PRODUCTOS</q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section>RECETAS</q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section>CONSEJOS</q-item-section>
          </q-item>
        </q-list>
      </div>
      <div class="col-8" style="width: 70%">
        <div class="q-pa-md row items-start q-gutter-md">
          <q-card
            class="tarjeta text-opensans q-ma-md titulo-hover"
            v-for="(dato, index) in datos"
            :key="index"
            @mouseleave="hover = null"
            @mouseover="hover = dato.id"
          >
            <img :src="dato.image" />

            <q-card-section>
              <div class="text-h6">{{ dato.title }}</div>
            </q-card-section>

            <q-card-section class="q-pt-none cuerpo-hover">
              {{ dato.content }}
            </q-card-section>
            <q-card-actions
              style="position: absolute; bottom: 5px"
              v-if="hover === dato.id"
            >
              <p class="text-weight-bold">Ver más</p>
            </q-card-actions>
          </q-card>
        </div>
      </div>
    </div>
    <div class="row justify-center" style="width: 100%; margin-top: 60px">
      <div class="col-12">
        <div
          class="text-caveat text-h2 text-weight-bold text-center text-primario"
        >
          Contáctanos
        </div>
        <img src="BRUSH-3 2.png" class="broch-conta" />
      </div>
    </div>

    <div class="row justify-center" style="width: 100%; margin-top: -15px">
      <div class="col-12">
        <q-form
          style="position: relative; left: 20%; width: 50%"
          class="row q-col-gutter-md"
          ref="FormUserEditar"
        >
          <q-input
            square
            outlined
            class="col-xs-12 col-sm-12 col-md-6"
            :value="nombre"
            label="NOMBRE"
            lazy-rules
            :rules="[(val) => !!val || 'Nombre es requerido']"
          />

          <q-input
            square
            outlined
            class="col-xs-12 col-sm-12 col-md-6"
            v-model="apellido"
            label="APELLIDO"
            lazy-rules
            :rules="[(val) => !!val || 'Apellido es requerido']"
          />

          <q-input
            square
            outlined
            class="col-xs-12 col-sm-12 col-md-6"
            v-model="telefono"
            label="TELÉFONO"
            lazy-rules
            :rules="[(val) => !!val || 'Teléfono es requerido']"
          />

          <q-input
            square
            outlined
            class="col-xs-12 col-sm-12 col-md-6"
            v-model="email"
            label="Correo Electrónico"
            lazy-rules
            :rules="[(val) => !!val || 'Correo Electrónico es requerido']"
          />
          <div class="col-md-12">
            <div class="row justify-end">
              <q-btn
                rounded
                type="submit"
                :loading="submitting"
                label="ENVIAR"
                class="q-mt-md"
                color="primary"
                style="width: 150px; height: 60px; color: #ffffff"
              >
                <template v-slot:loading>
                  <q-spinner-facebook />
                </template>
              </q-btn>
            </div>
          </div>
        </q-form>
      </div>
    </div>
  </q-page>
</template>

<script>
import { api } from "boot/axios";
export default {
  name: "PageIndex",
  mounted() {
    this.obtener();
  },
  data() {
    return {
      datos: [],
      nombre: null,
      apellido: null,
      telefono: null,
      email: null,
      submitting: false,
      hover: null,
    };
  },
  methods: {
    obtener() {
      api
        .get("/api/articles")
        .then((response) => {
          this.datos = response.data;
          console.log(response.data);
        })
        .catch(() => {
          this.$q.notify({
            color: "negative",
            position: "top",
            message: "Loading failed",
            icon: "report_problem",
          });
        });
    },
    simulateSubmit() {
      this.submitting = true;

      // Simulating a delay here.
      // When we are done, we reset "submitting"
      // Boolean to false to restore the
      // initial state.
      setTimeout(() => {
        // delay simulated, we are done,
        // now restoring submit to its initial state
        this.submitting = false;
      }, 3000);
    },
  },
};
</script>

<style lang="scss" scoped>
.head {
  position: relative;
  background-image: url("/hero.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  object-fit: fill;
  height: 805px;
  width: 100%;
}

.spacio_logo {
  position: relative;
  top: 40px;
  left: 200px;
  font-size: 8em;
  z-index: 1;
  margin-top: -2px;
  word-wrap: break-word;
}

img {
  height: 100%;
  width: 100%;
  /*object-fit: contain; */
}

.contenedor {
  position: relative;
  top: 88px;
  left: -400px;
  height: 200px;
  width: 1300px;
}

.spacio {
  position: relative;
  top: 190px;
  left: 200px;
  font-size: 8em;
  z-index: 1;
  margin-top: -2px;
  word-wrap: break-word;
}

.broch-conta {
  width: 40%;
  height: 20%;
  position: relative;
  left: 30%;
  top: -20px;
  opacity: 0.3;
}

.q-btn {
  border-style: solid !important;
  border-width: medium !important;
}

.tarjeta {
  width: 100%;
  min-height: 424px;
  max-width: 250px;
}

.titulo-hover:hover {
  color: #d8ad3d;
}

.cuerpo-hover {
  color: #3f454a !important;
}

.q-btn:hover {
  background-color: #0096d9 !important;
}
</style>

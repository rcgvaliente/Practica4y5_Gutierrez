<template>
  <div class="q-pa-md flex flex-center container">
    <div class="row items-center q-gutter-sm">
      <q-form @submit="submitForm" class="row items-center q-gutter-sm">
        <q-input
          v-model="fechaInicio"
          label="Fecha Inicio"
          dense
          placeholder="yyyy-mm-dd"
          ref="fechaInicioInput"
        >
          <template v-slot:append>
            <q-icon
              name="event"
              class="cursor-pointer"
              @click="openPopup('inicio')"
            />
          </template>
          <q-popup-proxy
            ref="inicioPopup"
            transition-show="scale"
            transition-hide="scale"
          >
            <q-date v-model="fechaInicio" mask="YYYY-MM-DD" />
          </q-popup-proxy>
        </q-input>

        <q-input
          v-model="fechaFin"
          label="Fecha Fin"
          dense
          placeholder="yyyy-mm-dd"
          ref="fechaFinInput"
        >
          <template v-slot:append>
            <q-icon
              name="event"
              class="cursor-pointer"
              @click="openPopup('fin')"
            />
          </template>
          <q-popup-proxy
            ref="finPopup"
            transition-show="scale"
            transition-hide="scale"
          >
            <q-date v-model="fechaFin" mask="YYYY-MM-DD" />
          </q-popup-proxy>
        </q-input>

        <q-btn type="submit" label="Buscar" color="primary" icon="search" />
      </q-form>
    </div>

    <div class="row justify-center">
      <div
        v-for="informacion in informaciones"
        :key="informacion.date"
        class="card-container"
      >
        <q-card class="my-card" style="width: 350px">
          <q-img
            :src="informacion.url"
            style="max-height: 200px; object-fit: cover"
          >
            <div class="absolute-bottom text-h6">{{ informacion.title }}</div>
          </q-img>

          <q-card-section class="card-content">
            {{ informacion.explanation }}
          </q-card-section>
        </q-card>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      informaciones: [],
      fechaInicio: "",
      fechaFin: "",
    };
  },
  methods: {
    async submitForm() {
      if (this.fechaInicio === "" && this.fechaFin === "") {
        // Manejo del caso cuando las fechas están vacías
        return;
      }

      try {
        var url =
          "https://api.nasa.gov/planetary/apod?api_key=tt57WXWq8Pt5tJNf3TVb1dzRO5DPQm3tN1JLF9qS&start_date=" +
          this.fechaInicio +
          "&end_date=" +
          this.fechaFin;
        const response = await axios.get(url);
        this.informaciones = response.data;
        console.log("respuesta exitosa:", response.data);
      } catch (error) {
        console.error(error);
      }
    },
    openPopup(popup) {
      if (popup === "inicio") {
        this.$refs.inicioPopup.show();
      } else if (popup === "fin") {
        this.$refs.finPopup.show();
      }
    },
  },
};
</script>

<style>
.card-container {
  padding: 20px;
}

.my-card {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.card-content {
  flex: 1;
  overflow-y: auto;
}
</style>

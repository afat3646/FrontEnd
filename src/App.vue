<script>
import axios from 'axios';

export default {
  data() {
    return {
      routeName: "", // Input del usuario para buscar por nombre
      foundRouteOrder: null, // Resultado de la búsqueda
      error: null, // Para manejar errores
    };
  },
  methods: {
    async searchRouteOrder() {
      try {
        const response = await axios.get(
          `http://127.0.0.1:8000/api/routeorders/search/${this.routeName}`
        );
        this.foundRouteOrder = response.data;
        this.error = null; // Limpia el error si la búsqueda fue exitosa
      } catch (err) {
        this.error = "Route order not found. Please check the name and try again.";
        this.foundRouteOrder = null; // Limpia el resultado anterior
        console.error(err);
      }
    },
  },
};
</script>

<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">Search Route Orders by Name</h1>

    <!-- Buscar por nombre de ruta -->
    <div class="row justify-content-center">
      <div class="col-12 col-md-6">
        <label for="routeName" class="form-label">Route Name</label>
        <input
          type="text"
          id="routeName"
          class="form-control mb-3"
          v-model="routeName"
          placeholder="Enter Route Name"
        />
        <button class="btn btn-primary mb-3" @click="searchRouteOrder">Search</button>
      </div>
    </div>

    <!-- Mostrar errores -->
    <div v-if="error" class="alert alert-danger text-center">
      {{ error }}
    </div>

    <!-- Mostrar resultados -->
    <div v-if="foundRouteOrder" class="row justify-content-center">
      <div class="col-12 col-md-8">
        <div class="card">
          <div class="card-header">
            <h3>Route Order Details</h3>
          </div>
          <div class="card-body">
            <p><strong>Route Name:</strong> {{ foundRouteOrder.route_name }}</p>
            <p><strong>Route Status:</strong> {{ foundRouteOrder.route_status }}</p>
            <p><strong>Order ID:</strong> {{ foundRouteOrder.order_id || "N/A" }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  margin-top: 20px;
}
</style>
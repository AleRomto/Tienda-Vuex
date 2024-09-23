<template>
  <div class="contenedor">
    <h1 class="titulo">Tienda 32Bits - Videojuegos</h1>
    <table>
      <thead>
        <tr>
          <th>Código</th>
          <th>Nombre</th>
          <th>Stock</th>
          <th>Precio</th>
          <th>Acciones</th> 
        </tr>
      </thead>
      <tbody>
        <tr v-for="juego in juegos" :key="juego.codigo">
          <td>{{ juego.codigo }}</td>
          <td>{{ juego.nombre }}</td>
          <td>{{ juego.stock }}</td>
          <td>${{ juego.precio }}</td>
          <td>
            <!-- ingresar cantidad de juegos para "vender" -->
            <input 
              v-model.number="cantidadVenta[juego.codigo]" 
              type="number" 
              min="1" 
              :max="juego.stock" 
              placeholder="cantidad" 
              class="input-venta"
            />
            <!-- botoncito de "venta" -->
            <button 
              @click="reducirStock(juego.codigo, cantidadVenta[juego.codigo])" 
              class="btn">
              Vender
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
  data() {
    return {
      cantidadVenta: {}
    };
  },
  computed: {
    ...mapState({
      juegos: state => state.juegos
    })
  },
  methods: {
    // "vender" los jueguitos
    reducirStock(codigo, cantidad) {
      if (cantidad > 0) {
        this.$store.dispatch('modificarStock', { codigo, cantidad: -cantidad });
        this.cantidadVenta[codigo] = 1; 
      } else {
        alert("Por favor ingresa una cantidad válida.");
      }
    }
  },
  mounted() {
    // valor por defecto
    this.juegos.forEach(juego => {
      this.$set(this.cantidadVenta, juego.codigo, 1); 
    });
  }
};
</script>

<style scoped>
.contenedor {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.titulo {
  font-size: 2em;
  color: #333;
  margin-bottom: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 12px;
  text-align: center;
  color: #333;
}

th {
  background-color: #f2f2f2;
  font-weight: bold;
}

td {
  background-color: #fff;
}

.btn {
  padding: 5px 10px;
  font-size: 0.9em;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.btn:hover {
  background-color: #0056b3;
}

.input-venta {
  width: 70px;
  margin-right: 10px;
  padding: 5px;
  text-align: center;
}
</style>

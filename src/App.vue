<template>
  <div id="app">
    <h1 class="title">
      <span class="cine">Taqui</span><span class="bol">lla</span><span class="ivia">Bol</span>
    </h1>
    <br>
    <div class="movie-carousel" :class="{ 'rotating': isRotating }">
      <img v-for="(movie, index) in movieList" :key="index" :src="movie.url" :alt="movie.title" class="movie" />
    </div>
    <br>
    <button @click="shiftMovies" class="toggle-button">Avanzar</button>
    <br>
    <!-- Información adicional -->
    <div v-for="(movie, index) in movieList" :key="index" class="movie-info">
      <div class="movie-details">
        <div class="additional-container">
          <div class="additional-image-container">
            <img :src="`/public/images/peli${index + 1}.jpg`" :alt="movie.title" class="additional-image" />
          </div>
          <div class="additional-text">
            <p class="additional-title">{{ movie.title }}</p>
            <p class="additional-info">Duración: 2h</p>
            <p class="additional-info">Edad: 12 para adelante</p>
            <table class="showtimes">
              <tr v-for="day in ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado']" :key="day">
                <td v-for="hour in ['17:00', '19:00', '21:00', '23:00']" :key="hour" @click="showDetails(`${day} ${hour}`)" class="showtime">{{ `${day} ${hour}` }}</td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </div>

    <!-- Ventana emergente de compra con formulario -->
    <div v-if="showPurchaseDetails" class="purchase-details">
      <div class="purchase-content">
        <form @submit.prevent="submitForm">
          <label for="name">Nombre:</label>
          <input type="text" id="name" v-model="purchaseDetails.name" required><br>
          
          <label for="paymentMethod">Método de Pago:</label>
          <select id="paymentMethod" v-model="purchaseDetails.paymentMethod" required>
            <option value="tarjeta">Tarjeta de Crédito</option>
            <option value="efectivo">Efectivo</option>
            <!-- Agrega otros métodos de pago según sea necesario -->
          </select><br>

          <label for="ticketQuantity">Número de Boletos:</label>
          <input type="number" id="ticketQuantity" v-model="purchaseDetails.ticketQuantity" required>
          <br>
          <label for="seatNumber">Seleccionar Asiento:</label>
          <div>
            <label>Fila:</label>
            <input type="text" id="seatRow" v-model="purchaseDetails.seatRow" required width="20px">
          </div>
          <div>
            <label>Número:</label>
            <input type="number" id="seatNumber" v-model="purchaseDetails.seatNumber" min="1" max="40" required>
          </div>

          <button type="submit">Comprar</button>
        </form>
        <button @click="closePurchaseDetails">Cancelar</button>
      </div>
    </div>

    <!-- Nueva pantalla de "Pago Realizado" -->
    <div v-if="paymentSuccessful" class="payment-success">
      <p>Pago Realizado</p>
      <button @click="closePaymentSuccess">Cerrar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      movieList: [
        { title: 'Movie 1', url: '/public/images/peli1.jpg' },
        { title: 'Movie 2', url: '/public/images/peli2.jpg' },
        { title: 'Movie 3', url: '/public/images/peli3.jpg' },
        { title: 'Movie 4', url: '/public/images/peli4.jpg' },
        { title: 'Movie 5', url: '/public/images/peli5.jpg' },
        { title: 'Movie 6', url: '/public/images/peli6.jpg' },
      ],
      isRotating: false,
      showPurchaseDetails: false,
      paymentSuccessful: false,
      purchaseDetails: {
        name: '',
        paymentMethod: 'tarjeta',
        ticketQuantity: 1,
        seatRow: 'A',
        seatNumber: 1,
      },
    };
  },
  methods: {
    toggleRotation() {
      this.isRotating = !this.isRotating;
    },
    shiftMovies() {
      if (this.isRotating) return;

      const firstMovie = this.movieList.shift();
      this.movieList.push(firstMovie);
    },
    showDetails(time) {
      // Implementa la lógica para mostrar la ventana emergente aquí
      this.purchaseDetails = {
        name: '',
        paymentMethod: 'tarjeta',
        ticketQuantity: 1,
        seatRow: 'A',
        seatNumber: 1,
      };
      this.showPurchaseDetails = true;
    },
    closePurchaseDetails() {
      this.showPurchaseDetails = false;
    },
    submitForm() {
      // Aquí puedes agregar lógica para procesar la compra con los datos del formulario
      // Por ejemplo, enviar una solicitud al servidor para procesar el pago, etc.
      // Simulamos un proceso de pago exitoso después de un breve retraso (puedes cambiar esto)
      setTimeout(() => {
        console.log('Compra realizada:', this.purchaseDetails);
        // Reinicia los detalles de la compra y cierra la ventana emergente
        this.purchaseDetails = {
          name: '',
          paymentMethod: 'tarjeta',
          ticketQuantity: 1,
          seatRow: 'A',
          seatNumber: 1,
        };
        this.showPurchaseDetails = false;

        // Muestra la pantalla de "Pago Realizado"
        this.paymentSuccessful = true;
      }, 1000);
    },
    closePaymentSuccess() {
      // Cierra la pantalla de "Pago Realizado"
      this.paymentSuccessful = false;
    },
  },
};
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
  display: flex;
  width: 100%;
  height: 100%;
  align-content: center;
  background-color: black;
  overflow-y: auto; /* Habilita el desplazamiento vertical */
  overflow-x: hidden;
  

}

#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  background-color: black;
  align-content: center;
  width: 100%;
  height: 100%;
  display: block;
}

.title {
  color: white;
}

.cine {
  color: cyan;
}

.bol {
  color: magenta;
}

.ivia {
  color: white;
}

.movie-carousel {
  display: flex;
  white-space: nowrap;
  transition: transform 0.5s ease;
}

.movie-carousel.rotating {
  animation: none;
}

.movie-carousel img {
  width: 100%;
  height: 392px;
  margin-right: 30px;
  border: 3px solid white;
  border-image: linear-gradient(45deg, cyan, magenta);
  border-image-slice: 1;
}

.toggle-button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
}

.additional-container {
  display: flex;
  border: 3px solid white;
  border-image: linear-gradient(45deg, cyan, magenta);
  border-image-slice: 1;
  margin-top: 20px;
  background-color: #333;
}

.additional-image-container {
  flex: 1;
  padding: 10px;
}

.additional-image {
  width: 100%;
  height: auto;
}

.additional-text {
  flex: 2;
  padding: 10px;
  text-align: left;
}

.additional-title {
  color: cyan;
  font-size: 20px;
  margin: 0;
}

.additional-info {
  color: white;
  margin: 5px 0;
}

.showtimes {
  width: 100%;
  margin-top: 10px;
}

.showtimes tr {
  background-color: #333;
}

.showtime {
  padding: 10px;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.showtime:hover {
  background-color: #555;
}

.purchase-details {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.purchase-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}

.purchase-content button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
}

/* Estilos para la pantalla de "Pago Realizado" */
.payment-success {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 128, 0, 0.7); /* Verde para indicar éxito */
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
  color: white;
  font-size: 24px;
  text-align: center;
}

.payment-success button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
}
</style>

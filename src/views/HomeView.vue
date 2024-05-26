<template>
  <div class="container">
    <header class="header">
      <div class="logo">
        <img :src="require(`../assets/logo.png`)" alt="Logo" />
      </div>
      <div class="name">
        <img :src="require(`../assets/moto.png`)" alt="moto" />
      </div>
      <div class="link">
        <a href="/booking">Solicitar Reserva</a>
      </div>
    </header>

    <main>
      <div class="ciudades">
        <div class="tarjeta">
          <img src="../assets/cartagena.png" alt="Cartagena de Indias" />
        </div>

        <div class="tarjeta">
          <img src="../assets/SanAndres_Colombia.jpg" alt="San Andrés Islas" />
        </div>

        <div class="tarjeta">
          <img src="../assets/Minca.jpg" alt="Minca" />
        </div>

        <div class="tarjeta">
          <img src="../assets/salento.jpg" alt="Salento" />
        </div>

        <div class="tarjeta">
          <img src="../assets/mompox.png" alt="Mompox" />
        </div>
      </div>
    </main>

    <footer>
      <p>Términos y condiciones | Contactanos</p>
    </footer>
  </div>
</template>

<script>
export default {
  methods: {
    goToBooking() {
      this.$router.push("/booking");
    },
  },
};
</script>

<style>
body {
  padding: 0px;
  margin: 0px;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: rgb(203, 232, 186);
  width: 99%;
}

.logo {
  width: 100px;
  height: auto;
}

.name {
  text-align: center;
}
.link {
  padding: 20px;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: calc(100vw - 20px);
}
.main {
  display: flex;
  flex-grow: 1;
  flex-direction: column;
  align-items: center;
}

.ing {
  display: flex;
  align-items: center;
  height: 5px;
}

button:hover {
  background-color: #5f9ea0;
}
header {
  width: 100%;
  text-align: center;
  padding: 5px;
  background-color: rgb(203, 232, 186);
}

h1 {
  font-size: 1.5rem;
}

.ciudades {
  display: flex;
  flex-wrap: wrap;
  width: 70%;
  justify-content: center;
  margin: 0 auto;
}

.tarjeta {
  display: flex;
  flex-grow: 1;
  width: 30%;
  background-color: #fff;
  border-radius: 1px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  text-align: center;
  margin: 5px;
}

.tarjeta img {
  display: flex;
  flex-grow: 1;
  width: 100%;
  object-fit: cover;
  border-radius: 10px 10px 0 0;
}

.tarjeta h2 {
  font-size: 20px;
  margin-bottom: 5px;
}

.tarjeta p {
  font-size: 1px;
  line-height: 1.5;
}

.tarjeta a {
  display: block;
  background-color: #007bff;
  color: #fff;
  border-radius: 5px;
  text-decoration: none;
  margin-top: 10px;
}

footer {
  width: 100%;
  text-align: right;
  background-color: rgb(203, 232, 186);
  margin: 10px;
}

@media (max-width: 480px) {
  .tarjeta {
    width: 100px;
  }
}
</style>

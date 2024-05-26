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
        <a href="/">Inicio</a>
      </div>
    </header>

    <div class="form-container">
      <div class="form-item">
        <label for="tipoFactura">Tipo de factura:</label>
        <select v-model="selectedHotel">
          <option v-for="hotel in hotels" :key="hotel.id" :value="hotel">
            {{ hotel.name }} - ${{ hotel.pricePerNight }}/noche
          </option>
        </select>
      </div>

      <div class="form-item">
        <label for="fechaInicio">Fecha inicio:</label>
        <input type="date" id="startDate" v-model="startDate" />

        <label for="fechaFin">Fecha fin:</label>
        <input type="date" id="endDate" v-model="endDate" />

        <input
          type="number"
          v-model="peopleCount"
          placeholder="Cantidad de personas"
        />

        <div v-for="index in parseInt(peopleCount)" :key="index">
          <input
            type="text"
            v-model="personIds[index - 1]"
            placeholder="ID de la persona"
          />
        </div>
      </div>
      <button
        @click="generateInvoice"
        :disabled="
          !selectedHotel ||
          !startDate ||
          !endDate ||
          peopleCount === '' ||
          peopleCount <= 0
        "
      >
        Generar factura
      </button>
    </div>

    <footer>
      <p>Términos y condiciones | Contactanos</p>
    </footer>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";

export default {
  data() {
    return {
      hotels: [
        { id: 1, name: "Hotel Plaza", pricePerNight: 120 },
        { id: 2, name: "Marina Bay", pricePerNight: 150 },
        { id: 3, name: "Sunset Resort", pricePerNight: 180 },
      ],
      selectedHotel: null,
      startDate: "",
      endDate: "",
      peopleCount: 1,
      personIds: [],
      bookings: [],
    };
  },
  methods: {
    generateInvoice() {
      if (
        (!this.selectedHotel ||
          !this.startDate ||
          !this.endDate ||
          this.peopleCount === "",
        this.peopleCount < 1)
      ) {
        alert(
          "Por favor, completa todos los campos. El número de personas debe ser mayor a cero."
        );
        return;
      }
      const days =
        (new Date(this.endDate) - new Date(this.startDate)) /
        (1000 * 3600 * 24);
      if (days <= 0) {
        alert("La fecha de fin debe ser mayor que la fecha de inicio.");
        return;
      }
      const totalCost = this.selectedHotel.pricePerNight * days;
      const perPerson = totalCost / this.peopleCount;
      const invoice = {
        hotel: this.selectedHotel.name,
        startDate: this.startDate,
        endDate: this.endDate,
        totalCost: totalCost,
        splitCost: perPerson,
        personIds: this.personIds,
      };
      this.bookings.push(invoice);
      localStorage.setItem("bookings", JSON.stringify(this.bookings));
      this.printPDF(invoice);
    },
    printPDF(invoice) {
      const doc = new jsPDF();
      doc.text(`Factura de Hotel: ${invoice.hotel}`, 10, 10);
      doc.text(`Fecha de inicio: ${invoice.startDate}`, 10, 20);
      doc.text(`Fecha de fin: ${invoice.endDate}`, 10, 30);
      doc.text(`Costo total: $${invoice.totalCost}`, 10, 40);
      doc.text(`Costo por persona: $${invoice.splitCost}`, 10, 50);
      doc.text(`IDs de personas: ${invoice.personIds.join(", ")}`, 10, 60);
      doc.save("factura.pdf");
    },
  },
  watch: {
    peopleCount(newValue) {
      if (newValue === "") {
        // Check for empty value
        alert("El campo 'Número de personas' no puede estar vacío.");
        this.peopleCount = 1;

        return false;
      }
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
header {
  width: 100%;
  text-align: center;
  padding: 5px;
  background-color: #8fbc8f;
}

.form-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 80%;
  max-width: 600px;
  margin: 10px;
  padding: 10px;
  border: 1px solid #000;
  border-radius: 15px;
}

.form-item {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
  width: 48%;
}

label {
  margin-bottom: 5px;
}

input,
select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

footer {
  width: 100%;
  text-align: right;
  background-color: rgb(203, 232, 186);
  margin: 10px;
}

@media (max-width: 768px) {
  .form-item {
    width: 100%;
  }
}
</style>

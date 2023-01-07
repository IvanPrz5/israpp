<template>
  <v-container>
    <h3>Neto a Bruto</h3>
    <v-combobox
      v-model="añoData"
      :items="años"
      label="Año"
      outlined
      dense
    ></v-combobox>
    <v-combobox
      v-model="periodoData"
      :items="periodo"
      label="Periodo"
      outlined
      dense
    ></v-combobox>
    <v-text-field
      v-model="netoData"
      label="Neto"
      aria-required="true"
      outlined
      dense
    ></v-text-field>
    <div class="btn-container">
      <v-btn depressed color="primary" @click="calculaBruto()">
        Calcular
      </v-btn>
    </div>
    <v-text-field
      v-model="brutoData"
      label="Bruto"
      outlined
      dense
      readonly
    ></v-text-field>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "FormNetoBruto",
  data() {
    return {
      añoData: "",
      periodoData: "",
      netoData: "",
      brutoData: "",
      //Items
      años: [
        "2015",
        "2016",
        "2017",
        "2018",
        "2019",
        "2020",
        "2021",
        "2022",
        "2023",
      ],
      periodo: ["Mensual", "Quincenal", "Semanal", "Diario"],
    };
  },
  methods: {
    calculaBruto() {
      axios
        .get(
          "http://localhost:8081/Isr/año/" +
            this.añoData +
            "/" +
            this.periodoData
        )
        .then((response) => {
          this.result = response.data;
          for (let i = 0; i < this.result.length; i++) {
            let netoFloat = parseFloat(this.netoData);
            let netoFloatProducto = this.netoData * 2;
            let limInf = response.data[i].limInf;
            let porcentaje = response.data[i].porcentaje;
            let cuota = response.data[i].cuota;
            console.log(
              response.data[i].limInf +
                "  -------  " +
                netoFloatProducto +
                "  ,   " +
                response.data[i].limSup
            );
            if (
              response.data[i].limInf < netoFloatProducto &&
              response.data[i].limSup > netoFloatProducto
            ) {
              let base = netoFloatProducto - limInf;
              let impuestoMarginal = base * porcentaje;
              let isrData = impuestoMarginal + cuota;
              let netoFloatTemporal = netoFloat + isrData;
              //console.log(response.data[i].limInf + "  +++++++  " + netoFloatProducto + "  ,   " + response.data[i].limSup);
              //console.log(netoFloatTemporal.toFixed(2));
              //console.log(response.data[i].limInf + "  +++++++  " + netoFloatProducto.toFixed(2) + "  ,   " + response.data[i].limSup);
              for (let j = netoFloat; j < netoFloatTemporal; j++) {
                let vari = netoFloatTemporal
                console.log(netoFloatTemporal.toFixed(2));
                if (netoFloat < netoFloatTemporal) {
                  if (
                    response.data[i].limInf < vari &&
                    response.data[i].limSup > vari
                  ) {
                    let base = netoFloatProducto - limInf;
                    let impuestoMarginal = base * porcentaje;
                    let isrData = impuestoMarginal + cuota;
                    netoFloatTemporal = netoFloat + isrData;
                    this.brutoData = netoFloatTemporal;
                    this.brutoData = this.brutoData.toFixed(2);
                  }
                }
              }
              /* for (let j = netoFloat; j < netoFloatTemporal; j++) {
                console.log(netoFloatTemporal);
                if (netoFloat < netoFloatTemporal) {
                  netoFloatProducto = netoFloatTemporal;
                  //console.log(netoFloatTemporal);
                  if (
                    response.data[i].limInf < netoFloatProducto &&
                    response.data[i].limSup > netoFloatProducto
                  ) {
                    let base = netoFloatProducto - limInf;
                    let impuestoMarginal = base * porcentaje;
                    let isrData = impuestoMarginal + cuota;
                    netoFloatTemporal = netoFloat + isrData;
                    this.brutoData = netoFloatTemporal;
                    this.brutoData = this.brutoData.toFixed(2);
                  }
                }
              } */
            }
          }
        });
    },
  },
};
</script>

<style></style>

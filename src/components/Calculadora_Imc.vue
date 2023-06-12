<template>
  <div :class="{ noturno: modoNoturno }" class="raiz">

    <div class="div_modo">
      <button class="botao_noturno" @click="ligarModoNoturno">
        <img class="imagem_modo" :src="modoNoturno ? imagemNoturna : imagemDiurna" alt="Botão de alternar modo">
      </button>
    </div>

    <div class="div_titulo">
      <h1 class="titulo_calculadora">Calculadora de IMC</h1>
    </div>

    <img class="img_capa" src="../assets/calculo_imc.png" alt="Diferentes tipos de IMC">

    <form @submit.prevent="calcular">

      <div>
        <label class="label_calculo" for="peso">Peso (kg): </label>
        <input type="number" id="peso" placeholder="70" v-model="peso" step="0.5" required>
      </div>

      <div>
        <label class="label_calculo" for="altura">Altura (m): </label>
        <input type="number" id="altura" placeholder="1.75" v-model="altura" step="0.01" required>
      </div>

      <div class="aviso">
        <p>É importante levar em consideração que o IMC por conta própria não é o suficiente para caracterizar uma pessoa como obesa ou definir algo quanto à sua saúde, já que o calculo não leva em consideração sua idade, gênero, percentual de gordura ou a prática de atividades físicas. Se você desejar uma avaliação mais completa do seu corpo, procure um profissional capacitado.</p>
      </div>

      <div>
        <button type="submit" class="botao_calcular">Calcular</button>
      </div>

      <div>
        <label class="resultado" v-if="mensagem">{{ mensagem }}</label>
        <br>
        <img :src= imagem :class="{ 'imagem-redimensionada': imagem }"  v-if="imagem">
      </div>

    </form>

  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
  name: "CalculadoraImc",

  data() {
    return {
      peso: null as number | null,
      altura: null as number | null,
      imc: null as number | null,
      mensagem: "" as string,
      imagem:"" ,
      modoNoturno: false
    };
  },

  mounted() {
    const preferenciaModo = localStorage.getItem("modoNoturno");
    if (preferenciaModo) {
      this.modoNoturno = JSON.parse(preferenciaModo);
      if (this.modoNoturno) {
        document.documentElement.classList.add("noturno");
      }
    }
  },

  computed: {
    imagemDiurna() {
      return require('@/assets/dark_mode.png');
    },
    imagemNoturna() {
      return require('@/assets/light_mode.png');
    }
  },

  methods: {
    calcular() {
      if (this.peso !== null && this.altura !== null) {

        const peso = parseFloat(this.peso.toString());

        const altura = parseFloat(this.altura.toString());

        const imc = peso / (altura * altura);
        
        this.imc = parseFloat(imc.toFixed(2));

        if (imc < 18.5 ) {
          this.mensagem = `Possui um IMC de ${this.imc}: Abaixo do Peso.`;
          this.imagem = require('@/assets/abaixo_do_peso.png');
        } 
        
        else if (imc > 18.5 && imc < 25) {
          this.mensagem = `Você possui um IMC de ${this.imc}: Peso Normal.`;
          this.imagem = require('@/assets/peso_normal.png');
        }

        else if (imc >= 25 && imc < 30) {
          this.mensagem = `Você possui um IMC de ${this.imc}: Acima do Peso.`;
          this.imagem = require('@/assets/acima_do_peso.png');
        } 
        
        else if (imc >= 30 && imc < 40) {
          this.mensagem = `Você possui um IMC de ${this.imc}: Obesidade Grau I/II.`;
          this.imagem = require('@/assets/obesidade_grau_1.png');
        } 
        
        else if (imc >= 40) {
          this.mensagem = `Você possui um IMC de ${this.imc}: Obesidade Grau III.`;
          this.imagem = require('@/assets/obesidade_grau_3.png');
        }
      }
    },

    ligarModoNoturno() {
      this.modoNoturno = !this.modoNoturno;
      document.documentElement.classList.toggle("noturno", this.modoNoturno);
      localStorage.setItem("modoNoturno", JSON.stringify(this.modoNoturno));
    }
  }
});
</script>
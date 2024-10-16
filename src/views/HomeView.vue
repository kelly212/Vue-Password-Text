<template>
  <div>
    <v-container>
      <br>
      <div style="text-align: center">
        <h1>Vue Password Text</h1>
        <br>
      </div>
      <v-row justify="center">
        <v-col cols="6">
          Mostra a força da senha e a lista
          <v-password-text :password="senha" :autocompletOff="true"
                           @update="(v)=>{senha=v}"
                           label="Senha"
                           :show_forca="true"
                           :show_list="true"
                           :items="teste"
                           id="senha">
          </v-password-text>
        </v-col>
        <v-col cols="6">
          Mostra a força da senha e esconde lista
          <v-password-text :password="senha1" :autocompletOff="false"
                           @update="(v)=>{senha1=v}"
                           label="Senha"
                           :show_forca="true"
                           :show_list="false"
                           id="senha2">
          </v-password-text>
        </v-col>

        <v-col cols="6">
          Mostra  a lista  e esconde a força da senha
          <v-password-text :password="senha2"
                           @update="(v)=>{senha2=v}"
                           label="Senha"
                           :show_forca="false"
                           :show_list="true"
                           id="senha3">
          </v-password-text>
        </v-col>
        <v-col cols="6">
          Esconde a força da senha e a lista
          <v-password-text :password="senha3"
                           @update="(v)=>{senha3=v}"
                           label="Senha"
                           :show_forca="false"
                           :show_list="false"
                           id="senha4">
          </v-password-text>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import { defineComponent } from 'vue';

// Components
import VPasswordText from '../components/VPasswordText.vue';

export default defineComponent({
  name: 'HomeView',
  data:()=>({
    senha: null,
    senha1: null,
    senha2: null,
    senha3: null,
    teste: [
      {cor: '#a7a7a7', descricao: 'Pelo menos uma letra maiuscula (A-Z)', key: 'maiuscula'},
      {cor: '#a7a7a7', descricao: 'Pelo menos uma letra minuscula (a-Z)', key: 'minuscula'},
      {cor: '#a7a7a7', descricao: 'Pelo menos um número (0-9)', key: 'numero'},
      {cor: '#a7a7a7', descricao: 'Pelo menos um caracter especial (#%!_@)', key: 'especial'},
      {cor: '#a7a7a7', descricao: 'Minimo 7 caractéres', key: 'minimo'},
    ]
  
  }),

  components: {
    VPasswordText,
  },
  mounted() {
  
  }
});
</script>

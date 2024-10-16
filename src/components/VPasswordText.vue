<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
			<div>
						<v-text-field :id="id"
																				v-model="passwordAux"
																				:class="classe"
																				:label="label"
																				:disabled="disabled"
																				:variant="variant"
																				:density="density"
																				hide-details
																				:prepend-inner-icon="icon"
																				:type="showSenha ? 'text' : 'password'"
																				@click:append-inner="showSenha = !showSenha"
																				:append-inner-icon="showSenha ? 'mdi-eye-outline' : 'mdi-eye-closed'" ></v-text-field>
						
						<v-menu  :activator="'#'+id" style="width: 200px!important" v-if="show_forca || show_list">
									<v-list>
												<v-list-item v-if="show_forca">
															<span style="color: gray;font-size: 13px">Força {{validarCampo(txtforca) ? '('+txtforca+')' : ''}} </span>
															<v-progress-linear rounded :disabled="disabled" v-model="forca" bg-color="gray-lighten-3" buffer-value="0"
																																		height="10" :color="color" ></v-progress-linear>
												</v-list-item>
												
												<v-divider v-if="show_list"  style="margin-top: 5px; margin-bottom: 5px;"></v-divider>
												
												<v-list-item v-if="show_list">
															<ul style="font-size: 13px">
																		<li :style="'color: ' +(temMaiuscula ? 'green' : '#a7a7a7')">Pelo menos uma letra maiuscula (A-Z)</li>
																		<li :style="'color: ' +(temMinuscula ? 'green' : '#a7a7a7')">Pelo menos uma letra minuscula (a-Z)</li>
																		<li :style="'color: ' +(temNumero ? 'green' : '#a7a7a7')">Pelo menos um número (0-9)</li>
																		<li :style="'color: ' +(temEspecial ? 'green' : '#a7a7a7')">Pelo menos um caracter especial (#%!_@)</li>
																		<li :style="'color: ' +(maior8 ? 'green' : '#a7a7a7')">Minimo 7 caractéres</li>
															</ul>
												</v-list-item>
									</v-list>
						</v-menu>
			</div>
</template>

<script>
   /* eslint-disable */
   export default {
      name: 'VPasswordText',
      components: {},
      props: {
         show_list: {default: true},
         autocompletOff: {default: true},
         disabled: {default: false},
         show_forca: {default: true},
         variant: {default: 'outlined'},
         icon: {default: 'mdi-lock'},
         density: {default: 'compact'},
         classe: {default: ''},
         label: {default: 'Label'},
         password: {default: ''},
         id: {default: 'menu-activator'},
      },
      data: () => ({
         forca: 0,
         txtforca: '',
         showSenha: true,
         temMaiuscula: true,
         maior8: true,
         temMinuscula: true,
         temNumero: true,
         temEspecial: true,

         // disabled: true,
         senha: null,
         color: 'gray',
         select: ['Vuetify', 'Programming'],
         items: [
            'Programming',
            'Design',
            'Vue',
            'Vuetify',
         ],
      }),
      methods: {
         validarCampo(campo) {
            return (campo !== undefined && campo !== null && campo !== '')
         },
         getForca() {
            let senha = this.passwordAux;

            this.temMaiuscula = /[A-Z]/.test(senha);
            this.temMinuscula = /[a-z]/.test(senha);
            this.temNumero = /[0-9]/.test(senha);
            this.temEspecial = /[^a-zA-Z0-9]/.test(senha);
            this.maior8 = senha.length >= 7

            if (this.temMaiuscula && this.temMinuscula && this.temNumero && this.temEspecial && this.maior8) {
               this.color = 'green'
               this.txtforca = 'Forte'
               this.forca = 100
            } else {
               let max =this.temMaiuscula ? 25: 0
               let num =this.temMinuscula ? 25: 0
               let min =this.temNumero ? 25: 0
               let esp =this.temEspecial ? 25: 0

               this.forca =  max + num + min + esp
               this.color = this.forca <= 50 ? (this.forca <= 25 ? 'red' : 'orange') : 'yellow'
               this.txtforca =this.forca < 50 ? (this.forca > 0 ? 'Fraca' : '') : 'Média'
            }


         }
      },
      created() {
      },
      mounted() {
         this.temMaiuscula = false
         this.temMinuscula = false
         this.temNumero = false
         this.temEspecial = false
         this.maior8 = false

 									if (this.autocompletOff){
												document.getElementById(this.id).addEventListener('focusin', function(event) {
                  if (event.target.tagName === 'INPUT' || event.target.tagName === 'TEXTAREA' || event.target.tagName === 'SELECT') {
                     event.target.setAttribute('autocomplete', 'off');
                  }
               });
									}
    
         
      },
      computed: {
         passwordAux: {
            get() {
               return this.password
            },
            set(v) {
               this.$emit('update', v)
               return this.passwordAux
            }
         },
      },
      watch: {
         passwordAux: function() {

            this.getForca()
         },

         senha: function() {
            this.getForca()
         }
      }
   }
</script>

<style scoped>

</style>

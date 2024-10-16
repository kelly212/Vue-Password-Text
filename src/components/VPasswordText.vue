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
																				:append-inner-icon="showSenha ? 'mdi-eye-outline' : 'mdi-eye-closed'"></v-text-field>
						
						<v-menu :activator="'#'+id" style="width: 200px!important" v-if="show_forca || show_list">
									<v-list>
												<v-list-item v-if="show_forca">
															<span style="color: gray;font-size: 13px">Força {{validarCampo(txtforca) ? '('+txtforca+')' : ''}} </span>
															<v-progress-linear rounded :disabled="disabled" v-model="forca" bg-color="gray-lighten-3" buffer-value="0"
																																		height="10" :color="color"></v-progress-linear>
												</v-list-item>
												
												<v-divider v-if="show_list" style="margin-top: 5px; margin-bottom: 5px;"></v-divider>
												<!--lista de força -->
												<v-list-item v-if="show_list">
															<ul style="font-size: 13px">
																						<li v-for="(item, index) in lista" :style="'color: ' +(item.cor)">{{item.descricao}}</li>
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
         items: {
            default: [
               {cor: '#a7a7a7', descricao: 'Pelo menos uma letra maiuscula (A-Z)', key: 'maiuscula'},
               {cor: '#a7a7a7', descricao: 'Pelo menos uma letra minuscula (a-Z)', key: 'minuscula'},
               {cor: '#a7a7a7', descricao: 'Pelo menos um número (0-9)', key: 'numero'},
               {cor: '#a7a7a7', descricao: 'Pelo menos um caracter especial (#%!_@)', key: 'especial'},
               {cor: '#a7a7a7', descricao: 'Minimo 7 caractéres', key: 'minimo'},
            ]
         }
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
         senha: null,
         lista: [],
         color: 'gray',
      }),
      methods: {
         validarCampo(campo) {
            return (campo !== undefined && campo !== null && campo !== '')
         },

         getForca() {
            let senha = this.passwordAux;
            var max = 0
            var num = 0
            var minu = 0
            var min = 0
            var esp = 0
            this.lista = []
            var _this = this
            var array = this.items
												var valor =  array.length > 0 ? ( Math.ceil(100 / array.length)) : 20
												var total =  array.length > 0 ? ( array.length*valor) : 100

            // console.log(valor);
            for (var i = 0; i < array.length; i++) {
               switch (array[i].key) {
                  case 'maiuscula':
                     array[i].cor = /[A-Z]/.test(senha) ? 'green' : '#a7a7a7';
                     max = /[A-Z]/.test(senha) ? valor : 0;
                     break;
                  case 'minuscula':
                     array[i].cor = /[a-z]/.test(senha) ? 'green' : '#a7a7a7';
                     minu = /[a-z]/.test(senha) ? valor : 0;
                     break;
                  case 'numero':
                     array[i].cor = /[0-9]/.test(senha) ? 'green' : '#a7a7a7';
                     num = /[0-9]/.test(senha) ? valor : 0;
                     break;
                  case 'especial':
                     array[i].cor = /[^a-zA-Z0-9]/.test(senha) ? 'green' : '#a7a7a7';
                     esp = /[^a-zA-Z0-9]/.test(senha) ? valor : 0;
                     break;
                  case 'minimo':
                     if (senha != null) {
                        array[i].cor = senha.length >= 7 ? 'green' : '#a7a7a7';
                     } else {
                        array[i].cor = '#a7a7a7'
                     }
                     min = senha.length >= 7 ? valor : 0;
                     break;
               }
               this.forca = max + num + min + esp + minu
															var _60 = parseInt(total*0.6)
															var _40 = parseInt(total*0.4)
															var _80 = parseInt(total*0.8)
               if (this.forca <= _60) {
                  this.color = this.forca <= _40 ? 'red' : 'orange'
                  this.txtforca = this.forca > 0 && this.forca <= _40 ? 'Fraca' : 'Media'
               } else if (this.forca > _60 && this.forca <= _80) {
                  this.color = this.forca <= _80 ? 'yellow' : 'green'
                  this.txtforca = this.forca <= _80 ? 'Mádia' : 'Forte'
               } else {
                  this.color = 'green'
                  this.txtforca = 'Forte'
               }

               this.lista = array
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
         this.lista = this.items

         if (this.autocompletOff) {
            document.getElementById(this.id).addEventListener('focusin', function (event) {
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
         passwordAux: function () {

            this.getForca()
         },

         senha: function () {
            this.getForca()
         }
      }
   }
</script>

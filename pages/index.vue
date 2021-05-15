<template>
  <div class="index">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-6">
          <div class="title">
            <h3 class="h3 text-center">
              Cotizador de envíos
            </h3>
          </div>
          <template v-if="optionsOrder.length > 0">
            <div class="text-center detail">
              <h6 class="h6 text-center">
                Código postal: <strong>"{{ postalCode }}"</strong>
              </h6>
              <button
                class="btn btn-primary"
                type="button"
                @click="init()"
              >
                Nueva cotización
              </button>
            </div>
          </template>
          <template v-else>
            <form
              class="text-center form"
              @submit.prevent="send()"
            >
              <div class="form-group">
                <input
                  id="postalCode"
                  name="postalCode"
                  class="form-control text-center"
                  type="text"
                  v-validate.disable="'required|min:4|max:4'"
                  v-model="postalCode"
                  placeholder="Ingresá el código postal"  v-validate="'required|email'"
                  data-vv-as="código postal"
                >
                <span
                  v-if="errors.has('postalCode')"
                  class="error"
                >
                  {{ errors.first('postalCode') }}
                </span>
              </div>
              <button
                class="btn btn-primary"
                type="submit"
              >
                {{ txtSubmit }}
              </button>
            </form>
          </template>
          <div class="options">
            <quote-option
              v-for="(option, index) in optionsOrder"
              :key="`option-${index}`"
              :value="option"
            /> 
          </div>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>

const CP_HOST = 1229;
const ID_ARTICLE = 431103;

import QuoteOption from '~/components/QuoteOption.vue'
import '@fortawesome/fontawesome-free/css/all.css'
import '@fortawesome/fontawesome-free/js/all.js'
//Instale awesome para poder usar iconos en el componente QuoteOption

export default {

  components: {
    QuoteOption,
  },

  data(){
    return {
      options: [],
      postalCode: '',
      loading: false,
      component: {}
    }
  },

  computed:{
    //Ordene los datos usando la funcion nativa de javaScript "Sort()", pueden ver los resultados en el console.log, para ordenar de mayor a menor cambiar total0 por totaln
    optionsOrder(){
      let data = this.options;
      if(data.length>0){
        console.log("Datos sin ordenar : ");
        console.log(data);
        data.sort((total0, totaln) => total0.Total - totaln.Total)
        console.log("Datos ordenados : ");
        console.log(data);
      }
      return data
    },
    txtSubmit(){
      return this.loading ? 'Cotizando...' : 'Cotizar'
    },
  },

  async mounted(){
    await this.init()
  },

  methods: {
    
    async init(){
      this.options = []
      this.postalCode = ''
    },

    async send(){

      this.$validator.validateAll().then(async (result) => {

        if (!result){
          return
        } else if(this.loading){
          return
        } else {
          this.loading = true
          // En este paso agregue la URL de la api libreOpcion, previamente estaba apuntando a localHost
          await fetch(`https://envios.libreopcion.com/item/${ID_ARTICLE}/cp/${this.postalCode}/cp_host/${CP_HOST}`)
          .then((response) => {
            return response.json()
          })
          .then((data) => {
            this.options =  data
          }).catch(function(e){
            console.log(e);
          });
          this.loading = false

        }
      })
    }

  },
  
  head () {
    return {
      title: 'Cotizador de envíos',
    }
  }

}
</script>

<style lang="scss" scoped>
.index{
  padding: 60px 0;
  .title{
    margin-bottom: 30px;
  }
  .detail{
    padding: 15px;
    h6{
      margin-bottom: 30px;
    }
  }
  .form{
    padding: 15px;
    .error{
      display: block;
      font-size: 12px;
      margin-top: 7.5px;
      color: red;
    }
  }
  .list{
    padding: 15px;
  }
  //Styles agregados.
  .options{
    display:flex;
		flex-direction: row;
		width: 100%;
		justify-content: start;
		align-items: center;
    margin-top:40px;
    text-align: center;
    flex-wrap:wrap;
    @media screen and (max-width: 992px){
            flex-direction: column;
            justify-content: center;
    }
  }
}
</style>

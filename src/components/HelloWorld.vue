<template>
  <v-card>
    <v-toolbar color="primary">

      <v-toolbar-title style="padding-left: 3%; padding-top: 3%;"><img style="width: 280px; margin-bottom: 30px;" src="../assets/ecoweb.png" alt=""></v-toolbar-title>

      <template v-slot:extension>
        <v-tabs
          v-model="form.type_interest"
          align-tabs="title"
        >
          <v-tab
            v-for="item in items"
            :key="item"
            :value="item"
            v-model="form.type_interest"
          >
            {{ item }}
          </v-tab>
        </v-tabs>
      </template>
    </v-toolbar>

    <v-window v-model="form.type_interest">
      <v-window-item
        v-for="item in items"
        :key="item"
        :value="item"
      >
      <!--INICIO INTERES SIMPLE-->
        <v-card flat v-if="form.type_interest == 'INTERES SIMPLE'" style="display: flex;">
          <div style="padding-top: 3%; width: 50%; margin-left: 10%;">
            <div style="width: 100%;">
              <img class="imgs" v-show="form.type_interest == 'INTERES SIMPLE' && form.type_calculate == null" src="../assets/interes_simple.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES SIMPLE' && form.type_calculate == 'CAPITAL'" src="../assets/capital_simple.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES SIMPLE' && form.type_calculate == 'VALOR FUTURO'" src="../assets/valor_futuro_simple.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES SIMPLE' && form.type_calculate == 'TASA DE INTERES'" src="../assets/tasa_de_interes_simple.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES SIMPLE' && form.type_calculate == 'INTERES PRODUCIDO'" src="../assets/interes__producido_simple.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES SIMPLE' && form.type_calculate == 'TIEMPO INVERTIDO'" src="../assets/tiempo_invertido_simple.jpg" alt="">
            </div>
          </div>
          <!--INICIO FORMULARIO-->
          <v-sheet width="350" class="mx-auto" style="padding-top: 30px;">

            <v-select
              v-model="form.type_calculate"
              :items="options_simple"
              label="Seleccione una opcion"
              required
            ></v-select>
  
            <v-form ref="form" >
              <v-text-field
                v-if="form.type_calculate != 'CAPITAL' && form.type_calculate != null"
                v-model="form.capital"
                :rules="[v => !!v || 'Capital debe ser numerico y superior a 0', validateNumeric]"
                label="CAPITAL"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>

                <v-text-field
                  v-if="form.type_calculate != 'TASA DE INTERES' && form.type_calculate != null"
                  v-model="form.interestRate"
                  :rules="[v => !!v || 'Tasa de interes debe ser numerico y superior a 0', validateNumeric]"
                  label="TASA DE INTERES"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-text-field
                  v-if="form.type_calculate != 'INTERES PRODUCIDO' && form.type_calculate != 'VALOR FUTURO' && form.type_calculate != null"
                  v-model="form.interestEarned"
                  :rules="[v => !!v || 'Interes producido debe ser numerico y superior a 0', validateNumeric]"
                  label="INTERES PRODUCIDO"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <div style="display: flex;" v-if="form.type_calculate != 'TIEMPO INVERTIDO' && form.type_calculate != null">
                  <v-text-field
                    style="width: 30%;"
                    v-model="anos"
                    :rules="[validateNumeric]"
                    label="AÑOS"
                    v-on:input="validateNumericInput"
                    required
                  ></v-text-field>

                  <v-text-field
                    style="width: 30%;"
                    v-model="meses"
                    :rules="[validateNumeric]"
                    label="MESES"
                    v-on:input="validateNumericInput"
                    required
                  ></v-text-field>

                  <v-text-field
                    style="width: 30%;"
                    v-model="dias"
                    :rules="[validateNumeric]"
                    label="DIAS"
                    v-on:input="validateNumericInput"
                    required
                  ></v-text-field>
                </div>
                <template v-if="error_date == true">
                  <v-alert
                    density="compact"
                    text="Un campo del tiempo debe ser superior a 0"
                    title="Error"
                    type="error"
                  ></v-alert>
                </template>

                <v-text-field
                    v-if="answer == true"
                    v-model="result"
                    disabled
                    :class="{ 'font-weight-bold': true }"
                    style="color: black;"
                  ></v-text-field>
        
              <div class="d-flex flex-column">
                <v-btn
                  color="#1867c0"
                  class="mt-4"
                  block
                  @click="validate()"
                >
                  Calcular
                </v-btn>
        
                <v-btn
                  color="error"
                  class="mt-4"
                  block
                  @click="reset"
                >
                  Limpiar
                </v-btn>
        
              </div>
            </v-form>
          </v-sheet>
          <!--FIN FORMULARIO-->
        </v-card>
      <!--FIN INTERES SIMPLE-->

      <!--INICIO INTERES COMPUESTO-->
        <v-card flat v-if="form.type_interest == 'INTERES COMPUESTO'" style="display: flex;">
          <div style="padding-top: 3%; width: 50%; margin-left: 10%;">
            <div style="width: 100%;">
              <img class="imgs" v-show="form.type_interest == 'INTERES COMPUESTO' && form.type_calculate == null" src="../assets/interés_compuesto.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES COMPUESTO' && form.type_calculate == 'CAPITAL'" src="../assets/capital compuesto.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES COMPUESTO' && form.type_calculate == 'TASA DE INTERES'" src="../assets/tasa de interés compuesto.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES COMPUESTO' && form.type_calculate == 'MONTO COMPUESTO'" src="../assets/monto compuesto.jpg" alt="">
              <img class="imgs" v-show="form.type_interest == 'INTERES COMPUESTO' && form.type_calculate == 'TIEMPO INVERTIDO'" src="../assets/tiempo compuesto.jpg" alt="">
            </div>
          </div>
          <!--INICIO FORMULARIO-->
          <v-sheet width="350" class="mx-auto" style="padding-top: 30px;">

            <v-select
              v-model="form.type_calculate"
              :items="options_compound"
              label="Seleccione una opcion"
              required
            ></v-select>
  
            <v-form ref="form" >
              <v-text-field
                v-if="form.type_calculate != 'CAPITAL' && form.type_calculate != null"
                v-model="form.capital"
                :rules="[v => !!v || 'Capital debe ser numerico y superior a 0', validateNumeric]"
                label="CAPITAL"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>

                <v-text-field
                  v-if="form.type_calculate != 'TASA DE INTERES' && form.type_calculate != null"
                  v-model="form.interestRate"
                  :rules="[v => !!v || 'Tasa de interes debe ser numerico y superior a 0', validateNumeric]"
                  label="TASA DE INTERES"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-text-field
                  v-if="form.type_calculate != 'MONTO COMPUESTO' && form.type_calculate != null"
                  v-model="form.compoundAmount"
                  :rules="[v => !!v || 'Monto compuesto debe ser numerico y superior a 0', validateNumeric]"
                  label="MONTO COMPUESTO"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-select v-if="form.type_calculate == 'TIEMPO INVERTIDO'"
                  v-model="form.showTime"
                  :items="options_time_compound"
                  label="Seleccione una opcion"
                  required
                ></v-select>

                <div style="display: flex;" v-if="form.type_calculate != 'TIEMPO INVERTIDO' && form.type_calculate != null">
                  <v-text-field
                    style="width: 30%;"
                    v-model="anos"
                    :rules="[validateNumeric]"
                    label="TIEMPO"
                    v-on:input="validateNumericInput"
                    required
                  ></v-text-field>
                  
                  <!--<v-text-field
                    style="width: 30%;"
                    v-model="meses"
                    :rules="[validateNumeric]"
                    label="MESES"
                    v-on:input="validateNumericInput"
                    required
                  ></v-text-field>

                  <v-text-field
                    style="width: 30%;"
                    v-model="dias"
                    :rules="[validateNumeric]"
                    label="DIAS"
                    v-on:input="validateNumericInput"
                    required
                  ></v-text-field>
                  -->
                </div>
                <template v-if="error_date == true">
                  <v-alert
                    density="compact"
                    text="Un campo del tiempo debe ser superior a 0"
                    title="Error"
                    type="error"
                  ></v-alert>
                </template>

                <v-text-field
                    v-if="answer == true"
                    v-model="result"
                    disabled
                  ></v-text-field>
        
              <div class="d-flex flex-column">
                <v-btn
                  color="#1867c0"
                  class="mt-4"
                  block
                  @click="validate()"
                >
                  Calcular
                </v-btn>
        
                <v-btn
                  color="error"
                  class="mt-4"
                  block
                  @click="reset"
                >
                  Limpiar
                </v-btn>
        
              </div>
            </v-form>
          </v-sheet>
          <!--FIN FORMULARIO-->
        </v-card>
      <!--FIN INTERES COMPUESTO-->

      <!--INICIO TASA DE INTERES-->>
      <v-card flat v-if="form.type_interest == 'TASA DE INTERES'" style="display: flex;">
        <div style="padding-top: 3%; width: 50%; margin-left: 10%;">
          <div style="width: 100%;">
            <img class="imgs" v-show="form.type_interest == 'TASA DE INTERES' && form.type_calculate == null" src="../assets/tasa de interes.jpg" alt="">
            <img class="imgs" v-show="form.type_interest == 'TASA DE INTERES' && form.type_calculate == 'INTERES SIMPLE'" src="../assets/tasa_de_interes_simple.jpg" alt="">
            <img class="imgs" v-show="form.type_interest == 'TASA DE INTERES' && form.type_calculate == 'INTERES COMPUESTO'" src="../assets/tasa de interés compuesto.jpg" alt="">
          </div>
        </div>

        <!--INICIO FORMULARIO-->
        <v-sheet width="350" class="mx-auto" style="padding-top: 30px;">

          <v-select
            v-model="form.type_calculate"
            :items="options_Rate"
            label="Seleccione una opcion"
            required
          ></v-select>

          <v-form ref="form" >
            <v-text-field
              v-if="form.type_calculate != 'CAPITAL' && form.type_calculate != null"
              v-model="form.capital"
              :rules="[v => !!v || 'Capital debe ser numerico y superior a 0', validateNumeric]"
              label="CAPITAL"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

              <v-text-field
                v-if="form.type_calculate != 'INTERES COMPUESTO' && form.type_calculate != null"
                v-model="form.interestEarned"
                :rules="[v => !!v || 'Interes producido debe ser numerico y superior a 0', validateNumeric]"
                label="INTERES PRODUCIDO"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>

              <v-text-field
                  v-if="form.type_calculate != 'INTERES SIMPLE' && form.type_calculate != null"
                  v-model="form.compoundAmount"
                  :rules="[v => !!v || 'Monto compuesto debe ser numerico y superior a 0', validateNumeric]"
                  label="MONTO COMPUESTO"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

              <div style="display: flex;" v-if="form.type_calculate != 'TIEMPO INVERTIDO' && form.type_calculate != null">
                <v-text-field
                  style="width: 30%;"
                  v-model="anos"
                  :rules="[validateNumeric]"
                  label="AÑOS"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-text-field
                  style="width: 30%;"
                  v-model="meses"
                  :rules="[validateNumeric]"
                  label="MESES"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-text-field
                  style="width: 30%;"
                  v-model="dias"
                  :rules="[validateNumeric]"
                  label="DIAS"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>
              </div>
              <template v-if="error_date == true">
                <v-alert
                  density="compact"
                  text="Un campo del tiempo debe ser superior a 0"
                  title="Error"
                  type="error"
                ></v-alert>
              </template>

              <v-text-field
                  v-if="answer == true"
                  v-model="result"
                  disabled
                ></v-text-field>
      
            <div class="d-flex flex-column">
              <v-btn
                color="#1867c0"
                class="mt-4"
                block
                @click="validate()"
              >
                Calcular
              </v-btn>
      
              <v-btn
                color="error"
                class="mt-4"
                block
                @click="reset"
              >
                Limpiar
              </v-btn>
      
            </div>
          </v-form>
        </v-sheet>
        <!--FIN FORMULARIO-->
      </v-card>
      <!--FIN TASA DE INTERES-->>
      </v-window-item>
    </v-window>
  </v-card>
</template>

<script>
  import axios from "axios";
  import $ from 'jquery';
  import moment from 'moment';

  export default {
    data () {
      return {
        form: {
          capital: 0,
          interestRate: 0,
          interestEarned: 0,
          compoundAmount: 0,
          type_calculate: null,
          type_interest: null,
          time: 0,
          showTime: null,
        },
        anos: 0,
        meses: 0,
        dias: 0,
        result: '',
        error_date: false,
        answer: false,
        items: [    //ITEMS DE LA BARRA DE NAVEFACION
          'INTERES SIMPLE', 'INTERES COMPUESTO', 'TASA DE INTERES',
        ],
        options_simple: [   //ITEMS DE LA BARRA DE NAVEFACION
          'CAPITAL',
          'VALOR FUTURO',
          'TASA DE INTERES',
          'INTERES PRODUCIDO',
          'TIEMPO INVERTIDO',
        ],
        options_compound:[
          'CAPITAL',
          'TASA DE INTERES',
          'MONTO COMPUESTO',
          'TIEMPO INVERTIDO',
        ],
        options_time_compound:[
          'AÑOS',
          'MESES',
          'DIAS',
          'SEMESTRE',
          'CUATRIMESTRE',
          'TRIMESTRE',
          'BIMESTRE',
          'QUINCENA',
          'SEMANA'
        ],
        options_Rate:[
          'INTERES SIMPLE',
          'INTERES COMPUESTO',
        ],
      }
    },
    watch: {
      'form.type_interest'(newTypeInterest, oldTypeInterest) {
        this.reset();
      },
      'form.type_calculate'(newOption, oldOption){
        this.reset(true);
      },
    },
    methods: {
      async validate() {
        let validate_date = await this.calculateTime();
        console.log(validate_date)

        if(validate_date){
          this.form.capital = parseInt(this.form.capital);
          this.form.interestRate = parseFloat(this.form.interestRate);
          this.form.interestEarned = parseInt(this.form.interestEarned);
          this.form.compoundAmount = parseInt(this.form.compoundAmount);
          axios.post("http://127.0.0.1:8000/api/calculate?", this.form)
          .then(res=> {
            this.answer = true;
            this.result = 'RESPUESTA: ' + res.data.result;
          })
          
        }
        console.log({FORMULARIO: this.form});
        
      },
      reset (typeCalculate = false) {
        this.form.capital        = 0;
        this.form.interestRate   = 0;
        this.form.interestEarned = 0;
        this.form.compoundAmount = 0;
        this.anos           = 0,
        this.meses          = 0,
        this.dias           = 0;
        this.error_date     = false,
        this.answer         = false,
        this.form.showTime       = null;
        if(!typeCalculate)this.form.type_calculate = null;
      },
      validateNumeric(value) {
        const numericRegex = /^[0-9.]+$/;
        return numericRegex.test(value) || 'Solo se permiten números';
      },
      validateNumericInput() {
        this.form.capital = String(this.form.capital).replace(/[^0-9.]/g, '');
        this.form.interestRate = String(this.form.interestRate).replace(/[^0-9.]/g, '');
        this.form.interestEarned = String(this.form.interestEarned).replace(/[^0-9.]/g, '');
        this.form.compoundAmount = String(this.form.compoundAmount).replace(/[^0-9.]/g, '');
        this.anos = String(this.anos).replace(/[^0-9.]/g, '');
        this.meses = String(this.meses).replace(/[^0-9.]/g, '');
        this.dias = String(this.dias).replace(/[^0-9.]/g, '');
      },
      calculateTime() {
        if(this.form.type_calculate == "TIEMPO INVERTIDO"){
          this.error_date = false;
          return true;
        }else if((this.anos == null || this.meses == null || this.dias == null)){
          this.error_date = true;
          this.answer = false;
          return false;
        }else if((this.anos > 0 || this.meses > 0 || this.dias > 0) && this.form.type_calculate !== "TIEMPO INVERTIDO"){
          this.form.time = parseFloat(this.anos) + parseFloat(this.meses / 12) + parseFloat(this.dias / 360);
          this.error_date = false;
          return true;
        } else {
          this.error_date = true;
          this.answer = false;
          return false;
        }
      },
    },
    
  }
</script>

<style>
  .imgs{
    width: 100%;
    height: 100%;
  }
</style>
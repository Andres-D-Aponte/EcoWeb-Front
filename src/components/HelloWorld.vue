<template>
  <v-card>
    <v-toolbar color="primary">

      <v-toolbar-title style="padding-left: 3%;">LOGO</v-toolbar-title>

      <template v-slot:extension>
        <v-tabs
          v-model="tab"
          align-tabs="title"
        >
          <v-tab
            v-for="item in items"
            :key="item"
            :value="item"
          >
            {{ item }}
          </v-tab>
        </v-tabs>
      </template>
    </v-toolbar>

    <v-window v-model="tab">
      <v-window-item
        v-for="item in items"
        :key="item"
        :value="item"
      >
      <!--INICIO INTERES SIMPLE-->
        <v-card flat v-if="tab == 'INTERES SIMPLE'">
          <!--INICIO FORMULARIO-->
          <v-sheet width="300" class="mx-auto" style="padding-top: 30px;">

            <v-select
              v-model="type_calculate"
              :items="options_simple"
              label="Seleccione una opcion"
              required
            ></v-select>
  
            <v-form ref="form" >
              <v-text-field
                v-if="type_calculate != 'CAPITAL' && type_calculate != null"
                v-model="capital"
                :rules="[v => !!v || 'Capital debe ser numerico', validateNumeric]"
                label="CAPITAL"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>

                <v-text-field
                  v-if="type_calculate != 'TASA DE INTERES' && type_calculate != null"
                  v-model="interestRate"
                  :rules="[v => !!v || 'Tasa de interes debe ser numerico', validateNumeric]"
                  label="TASA DE INTERES"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-text-field
                  v-if="type_calculate != 'INTERES PRODUCIDO' && type_calculate != 'VALOR FUTURO' && type_calculate != null"
                  v-model="interestEarned"
                  :rules="[v => !!v || 'Interes producido debe ser numerico', validateNumeric]"
                  label="INTERES PRODUCIDO"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <div style="display: flex;" v-if="type_calculate != 'TIEMPO INVERTIDO' && type_calculate != null">
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
      <!--FIN INTERES SIMPLE-->

      <!--INICIO INTERES COMPUESTO-->
        <v-card flat v-if="tab == 'INTERES COMPUESTO'">
          <!--INICIO FORMULARIO-->
          <v-sheet width="300" class="mx-auto" style="padding-top: 30px;">

            <v-select
              v-model="type_calculate"
              :items="options_compound"
              label="Seleccione una opcion"
              required
            ></v-select>
  
            <v-form ref="form" >
              <v-text-field
                v-if="type_calculate != 'CAPITAL' && type_calculate != null"
                v-model="capital"
                :rules="[v => !!v || 'Capital debe ser numerico', validateNumeric]"
                label="CAPITAL"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>

                <v-text-field
                  v-if="type_calculate != 'TASA DE INTERES' && type_calculate != null"
                  v-model="interestRate"
                  :rules="[v => !!v || 'Tasa de interes debe ser numerico', validateNumeric]"
                  label="TASA DE INTERES"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-text-field
                  v-if="type_calculate != 'MONTO COMPUESTO' && type_calculate != null"
                  v-model="compoundAmount"
                  :rules="[v => !!v || 'Monto compuesto debe ser numerico', validateNumeric]"
                  label="INTERES PRODUCIDO"
                  v-on:input="validateNumericInput"
                  required
                ></v-text-field>

                <v-select v-if="type_calculate == 'TIEMPO INVERTIDO'"
                  v-model="showTime"
                  :items="options_time_compound"
                  label="Seleccione una opcion"
                  required
                ></v-select>

                <div style="display: flex;" v-if="type_calculate != 'TIEMPO INVERTIDO' && type_calculate != null">
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
                  @click="validate"
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

      </v-window-item>
    </v-window>
  </v-card>
</template>

<script>
  
  import $ from 'jquery';
  import moment from 'moment';

  export default {
    data () {
      return {
        capital: 0,
        interestRate: 0,
        interestEarned: 0,
        compoundAmount: 0,
        anos: 0,
        meses: 0,
        dias: 0,
        time: 0,
        result: '',
        type_calculate: null,
        tab: null,
        showTime: null,
        error_date: false,
        answer: false,
        items: [    //ITEMS DE LA BARRA DE NAVEFACION
          'INTERES SIMPLE', 'INTERES COMPUESTO', 'ANUALIDAD',
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
          'DIAS',
          'MESES',
          'AÑOS',
        ],
        filtro: moment().format('MM/DD/YYYY')+' - '+moment().format('MM/DD/YYYY'),
      }
    },
    watch: {
      tab(newTab, oldTab) {
        this.reset();
      },
    },
    methods: {
      async validate () {
        let validate_date = await this.calculateTime();
        if(validate_date){
          this.answer = true;
          this.result = 18;
        }
        console.log(this.capital , this.interestRate , this.interestEarned, this.compoundAmount, this.anos , this.meses , this.dias );
        
      },
      reset () {
        this.capital        = 0;
        this.interestRate   = 0;
        this.interestEarned = 0;
        this.compoundAmount = 0;
        this.anos           = 0,
        this.meses          = 0,
        this.dias           = 0;
        this.error_date     = false,
        this.answer         = false,
        this.showTime       = null;
        this.type_calculate = null;
      },
      validateNumeric(value) {
        const numericRegex = /^[0-9.]+$/;
        return numericRegex.test(value) || 'Solo se permiten números';
      },
      validateNumericInput() {
        this.capital = String(this.capital).replace(/[^0-9.]/g, '');
        this.interestRate = String(this.interestRate).replace(/[^0-9.]/g, '');
        this.interestEarned = String(this.interestEarned).replace(/[^0-9.]/g, '');
        this.compoundAmount = String(this.compoundAmount).replace(/[^0-9.]/g, '');
        this.anos = String(this.anos).replace(/[^0-9.]/g, '');
        this.meses = String(this.meses).replace(/[^0-9.]/g, '');
        this.dias = String(this.dias).replace(/[^0-9.]/g, '');
      },
      calculateTime() {
        if (this.anos > 0 || this.meses > 0 || this.dias > 0) {
          this.time = (this.anos * 360) + (this.meses * 30) + (this.dias);
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
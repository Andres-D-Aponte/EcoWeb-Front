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

                <vs-input  name="daterange" id="daterange" v-model="filtro" class="w-full" autocomplete="off" readonly/>
        
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
        type_calculate: null,
        tab: null,
        showTime: null,
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
      validate () {
        console.log(this.tab);
      },
      reset () {
        this.capital        = 0;
        this.interestRate   = 0;
        this.interestEarned = 0;
        this.compoundAmount = 0;
        this.showTime       = null;
        this.type_calculate = null;
      },
      validateNumeric(value) {
        const numericRegex = /^[0-9.]+$/;
        return numericRegex.test(value) || 'Solo se permiten números';
      },
      validateNumericInput() {
        this.capital = this.capital.replace(/[^0-9.]/g, '');
      },
      activateDateRange(){
		    var today = new Date()
        $('#daterange').daterangepicker({
          opens: 'right',  
          autoApply: true,
          "locale": {
          "format": "MM/DD/YYYY",
          "separator": " - ",
          "applyLabel": "Aplicar",
          "cancelLabel": "Cancelar",
          "fromLabel": "From",
          "toLabel": "To",
          "customRangeLabel": "Custom",
          "startDate": moment().format('MM/DD/YYYY'),
          "endDate": moment().format('MM/DD/YYYY'),        
          "daysOfWeek": [
              "Do",
              "Lu",
              "Ma",
              "Mi",
              "Ju",
              "Vi",
              "Sa"
          ],
          "monthNames": [
              "Enero",
              "Febrero",
              "Marzo",
              "Abril",
              "Mayo",
              "Junio",
              "Julio",
              "Agosto",
              "Septiembre",
              "Octubre",
              "Noviembre",
              "Diciembre"
          ],
          "firstDay": 1,
          }      
        }, function(start, end, label) {});
      },
    },
    mounted(){
      //this.activateDateRange();
    }
  }
</script>
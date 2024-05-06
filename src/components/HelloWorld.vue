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

      <!--INICIO TASA DE INTERES-->
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
      <!--FIN TASA DE INTERES-->

      <!--INICIO AMORTIZACION-->
      <v-card flat v-if="form.type_interest == 'AMORTIZACION'" style="display: flex;">
        <div style="padding-top: 3%; width: 50%; margin-left: 10%;">
          <div style="width: 100%;">
            <img class="imgs" v-show="form.type_interest == 'AMORTIZACION' && form.type_calculate == null" src="../assets/amortizacion.png" alt="">
            <img class="imgs" v-show="form.type_interest == 'AMORTIZACION' && form.type_calculate == 'AMORTIZACION'" src="../assets/amortizacion1.png" alt="">
            <img class="imgs" v-show="form.type_interest == 'AMORTIZACION' && form.type_calculate == 'CAPITALIZACION'" src="../assets/capitalizacion.png" alt="">
          </div>
        </div>

        <!--INICIO FORMULARIO-->
        <v-sheet width="350" class="mx-auto" style="padding-top: 30px;">

          <v-select
            v-model="form.type_calculate"
            :items="options_Amortization"
            label="Seleccione una opcion"
            required
          ></v-select>

          <v-form ref="form" >
            <v-text-field
              v-if="form.type_calculate != 'CAPITALIZACION' && form.type_calculate != null"
              v-model="form.active_cost"
              :rules="[v => !!v || 'Costo activo debe ser numerico y superior a 0', validateNumeric]"
              label="COSTO ACTIVO"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>
            
            <v-text-field
              v-if="form.type_calculate != 'AMORTIZACION' && form.type_calculate != null"
              v-model="form.present_value"
              :rules="[v => !!v || 'Valor presente debe ser numerico y superior a 0', validateNumeric]"
              label="VALOR PRESENTE"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            <v-text-field
              v-if="form.type_calculate != 'AMORTIZACION' && form.type_calculate != null"
              v-model="form.interestRate"
              :rules="[v => !!v || 'Tasa de interes debe ser numerico y superior a 0', validateNumeric]"
              label="TASA DE INTERES"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            <v-text-field
              v-if="form.type_calculate != null"
              v-model="form.useful_life"
              :rules="[v => !!v || 'Vida útil debe ser numerico y superior a 0', validateNumeric]"
              label="VIDA UTIL"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

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
      <!--FIN AMORTIZACION-->

      <!--INICIO TASA INTERNA DE RETORNO-->
      <v-card flat v-if="form.type_interest == 'TASA INTERNA DE RETORNO'" style="display: flex;">
        <div style="padding-top: 3%; width: 50%; margin-left: 10%;">
          <div style="width: 100%;">
            <img class="imgs" v-show="form.type_interest == 'TASA INTERNA DE RETORNO' && form.type_calculate == null" src="../assets/TIR.png" alt="">
            <img class="imgs" v-show="form.type_interest == 'TASA INTERNA DE RETORNO' && form.type_calculate == 'TIR'" src="../assets/TIR1.png" alt="">
          </div>
        </div>

        <!--INICIO FORMULARIO-->
        <v-sheet width="350" class="mx-auto" style="padding-top: 30px;">

          <v-select
            v-model="form.type_calculate"
            :items="options_TIR"
            label="Seleccione una opcion"
            required
          ></v-select>

          <v-form ref="form" >

            <v-text-field
              v-if="form.type_calculate != null && form.type_calculate == 'TIR'"
              v-model="form.num_tir"
              :rules="[v => !!v || 'El número de flujos debe ser numérico y superior a 0', validateNumeric]"
              label="Número de flujos de efectivo"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            <v-text-field
              v-if="form.type_calculate != null && form.num_tir > 0 && form.type_calculate == 'TIR'"
              v-for="index in parseInt(form.num_tir) + 1"
              :key="index"
              v-model="form.flujos[index - 1]" 
              :rules="[v => !!v || 'El flujo de dinero debe ser numérico y superior a 0', validateNumeric]"
              :label="'Año ' + (index - 1)"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

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
      <!--FIN TASA INTERNA DE RETORNO-->

      <!--INICIO GRADIENTES-->
      <v-card flat v-if="form.type_interest == 'GRADIENTES'" style="display: flex;">
        <div style="padding-top: 3%; width: 50%; margin-left: 10%;">
          <div style="width: 100%;">
            <img class="imgs" v-show="form.type_interest == 'GRADIENTES' && form.type_calculate == null" src="../assets/gradientes.png" alt="">
            <img class="imgs" v-show="form.type_interest == 'GRADIENTES' && form.type_calculate == 'GRADIENTE ARITMETICO'" src="../assets/gradiente_aritmetico.png" alt="">
            <img class="imgs" v-show="form.type_interest == 'GRADIENTES' && form.type_calculate == 'GRADIENTE GEOMETRICO'" src="../assets/gradiente_geometrico.png" alt="">
          </div>
        </div>

        <!--INICIO FORMULARIO-->
        <v-sheet width="350" class="mx-auto" style="padding-top: 30px;">

          <v-select
            v-model="form.type_calculate"
            :items="options_gradients"
            label="Seleccione una opcion"
            required
          ></v-select>

          <v-form ref="form" >

            <v-text-field
              v-if="form.type_calculate != null"
              v-model="form.interestRate"
              :rules="[v => !!v || 'Tasa de interes debe ser numerico y superior a 0', validateNumeric]"
              label="TASA DE INTERES"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            <v-text-field
              v-if="form.type_calculate != null"
              v-model="form.num_gradients"
              :rules="[v => !!v || 'El número de periodos debe ser numérico y superior a 0', validateNumeric]"
              label="Número de periodos a pagar"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            <v-text-field
              v-if="form.type_calculate != null && form.num_gradients > 0 && form.num_gradients != 'GRADIENTE ARITMETICO'"
              v-model="form.consignaciones.monto[0]"
              :rules="[v => !!v || 'Debe ser numérico y superior a 0', validateNumeric]"
              label="CAPITAL"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            <v-text-field
              v-if="form.type_calculate != null && form.num_gradients > 0 && form.num_gradients != 'GRADIENTE ARITMETICO'"
              v-model="form.incremento"
              :rules="[v => !!v || 'Tasa de crecimiento debe ser numerico y superior a 0', validateNumeric]"
              label="TASA DE CRECIMIENTO POR PERIODO"
              v-on:input="validateNumericInput"
              required
            ></v-text-field>

            
            <div v-for="index in parseInt(form.num_gradients)" :key="index" style="display: flex; justify-content: space-between;" v-if="form.type_calculate != null && form.num_gradients > 0 && form.type_calculate != 'GRADIENTE GEOMETRICO'">
              <v-text-field
                style="width: 45%;"
                v-model="form.consignaciones.monto[index - 1]"
                :rules="[v => !!v || 'Debe ser numérico y superior a 0', validateNumeric]"
                label="CAPITAL"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>

              <v-text-field
                style="width: 45%;"
                v-model="form.consignaciones.meses[index - 1]"
                :rules="[v => !!v || 'Debe ser numérico y superior a 0', validateNumeric]"
                label="MES"
                v-on:input="validateNumericInput"
                required
              ></v-text-field>
            </div>
            

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
      <!--FIN GRADIENTES-->

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
          active_cost: 0,
          useful_life: 0,
          present_value: 0,
          num_tir: 1,
          num_gradients: 1,
          time: 0,
          incremento: 0,
          type_calculate: null,
          type_interest: null,
          showTime: null,
          flujos: [],
          consignaciones: {
            monto: {},
            meses: {}
          },
        },
        anos: 0,
        meses: 0,
        dias: 0,
        result: '',
        error_date: false,
        answer: false,
        items: [    //ITEMS DE LA BARRA DE NAVEFACION
          'INTERES SIMPLE', 'INTERES COMPUESTO', 'TASA DE INTERES', 'AMORTIZACION', 'TASA INTERNA DE RETORNO', 'GRADIENTES'
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
        options_Amortization:[
          'AMORTIZACION',
          'CAPITALIZACION',
        ],
        options_TIR:[
          'TIR',
        ],
        options_gradients:[
          'GRADIENTE ARITMETICO',
          'GRADIENTE GEOMETRICO',
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
          this.form.active_cost = parseInt(this.form.active_cost);
          this.form.useful_life = parseInt(this.form.useful_life);
          this.form.flujos = this.form.flujos;
          this.form.consignaciones = this.form.consignaciones;
          this.form.present_value = parseInt(this.form.present_value);
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
        this.form.active_cost    = 0;
        this.form.useful_life    = 0;
        this.form.present_value  = 0;
        this.form.num_tir        = 1;
        this.form.num_gradients  = 1;
        this.anos                = 0;
        this.meses               = 0;
        this.dias                = 0;
        this.form.flujos         = [];
        this.form.consignaciones = { monto: {}, meses: {}};
        this.error_date          = false;
        this.answer              = false;
        this.form.showTime       = null;
        if(!typeCalculate)this.form.type_calculate = null;
      },
      validateNumeric(value) {
        const numericRegex = /^-?[0-9.]+$/;
        return numericRegex.test(value) || 'Solo se permiten números';
      },
      validateNumericInput() {
        this.form.capital = String(this.form.capital).replace(/[^0-9.]/g, '');
        this.form.interestRate = String(this.form.interestRate).replace(/[^0-9.]/g, '');
        this.form.interestEarned = String(this.form.interestEarned).replace(/[^0-9.]/g, '');
        this.form.compoundAmount = String(this.form.compoundAmount).replace(/[^0-9.]/g, '');
        this.form.active_cost = String(this.form.active_cost).replace(/[^0-9.]/g, '');
        this.form.useful_life = String(this.form.useful_life).replace(/[^0-9.]/g, '');
        this.form.present_value = String(this.form.present_value).replace(/[^0-9.]/g, '');
        this.anos = String(this.anos).replace(/[^0-9.]/g, '');
        this.meses = String(this.meses).replace(/[^0-9.]/g, '');
        this.dias = String(this.dias).replace(/[^0-9.]/g, '');
      },
      calculateTime() {
        if(this.form.type_calculate == "TIEMPO INVERTIDO" || this.form.type_calculate == "AMORTIZACION" || this.form.type_calculate == "CAPITALIZACION" || this.form.type_calculate == "TIR" || this.form.type_interest == "GRADIENTES"){
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
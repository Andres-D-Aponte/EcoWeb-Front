<template>
  <v-card>
    <v-toolbar color="primary">
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Your Dashboard</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>

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
        <v-card flat>
          <v-sheet width="300" class="mx-auto">
  
            <v-form ref="form">
              <v-text-field
                v-model="name"
                :counter="10"
                :rules="nameRules"
                label="Name"
                required
              ></v-text-field>
        
              <v-select
                v-model="select"
                :items="items2"
                :rules="[v => !!v || 'Item is required']"
                label="Item"
                required
              ></v-select>
        
              <v-checkbox
                v-model="checkbox"
                :rules="[v => !!v || 'You must agree to continue!']"
                label="Do you agree?"
                required
              ></v-checkbox>
        
              <div class="d-flex flex-column">
                <v-btn
                  color="success"
                  class="mt-4"
                  block
                  @click="validate"
                >
                  Validate
                </v-btn>
        
                <v-btn
                  color="error"
                  class="mt-4"
                  block
                  @click="reset"
                >
                  Reset Form
                </v-btn>
        
                <v-btn
                  color="warning"
                  class="mt-4"
                  block
                  @click="resetValidation"
                >
                  Reset Validation
                </v-btn>
              </div>
            </v-form>
          </v-sheet>
        </v-card>
      </v-window-item>
    </v-window>
  </v-card>
</template>

<script>
  export default {
    data () {
      return {
        tab: null,
        items: [    //ITEMS DE LA BARRA DE NAVEFACION
          'INTERES SIMPLE', 'INTERES COMPUESTO', 'ANUALIDAD',
        ],
        text: '',
        name: '',
        nameRules: [
          v => !!v || 'Name is required',
          v => (v && v.length <= 10) || 'Name must be less than 10 characters',
        ],
        select: null,
        items2: [
          'Item 1',
          'Item 2',
          'Item 3',
          'Item 4',
        ],
        checkbox: false,
      }
    },
    methods: {
        async validate () {
          const { valid } = await this.$refs.form.validate()
  
          if (valid) alert('Form is valid')
        },
        reset () {
          this.$refs.form.reset()
        },
        resetValidation () {
          this.$refs.form.resetValidation()
        },
      },
  }
</script>
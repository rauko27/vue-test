<template>
  <div class="wrap" data-app>
    <div class="users-data">
      <v-btn
        class="users-data__btn"
        elevation="2"
        rounded
        @click="toggleModal"
      >
        Добавить
      </v-btn>
      <v-btn
        class="users-data__btn"
        elevation="2"
        rounded
        @click="cleanData"
      >
        Очистить
      </v-btn>
      <v-data-table
        :headers="headers"
        :items="items"
        items-key="name"
        elevation="1"
        class="user-data__table"
        hide-default-footer
      >
        <template v-slot:no-data>
          <v-alert :value="true" color="grey lighten-2 blue-grey--text text--darken-4" icon="warning">
            Данные отсутствуют
          </v-alert>
        </template>
      </v-data-table>
    </div>
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <v-card>
        <v-card-title>
          <span class="text-h5">Добавление пользователя</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  v-model="name"
                  label="Имя"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="phone"
                  label="Телефон"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-select
                  v-model="master"
                  :items="items"
                  item-text="name"
                  item-value="name"
                  label="Начальник"
                ></v-select>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Отмена
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="addUser"
          >
            Сохранить
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      dialog: false,
      name: '',
      phone: '',
      master: '',
      masters: [],
      headers: [
        {
          text: 'Имя',
          value: 'name',
          sortable: true
        },
        {
          text: 'Телефон',
          value: 'phone',
          sortable: true
        }
      ],
      items: []
    }
  },
  mounted: function () {
    this.items = JSON.parse(localStorage.getItem('items')) || []
    window.addEventListener('beforeunload', () => {
      localStorage.setItem('items', JSON.stringify(this.items))
    })
  },
  methods: {
    toggleModal () {
      this.dialog = true
    },
    cleanData () {
      localStorage.clear()
      this.items = []
      this.masters = []
    },
    addUser () {
      if (this.name || this.phone) {
        this.items.push({name: this.name, phone: this.phone, master: this.master})
      }
      if (this.master) {
        this.masters.push(this.mater)
      }
      this.name = ''
      this.phone = ''
      this.master = ''
      this.dialog = false
    }
  }
}
</script>

<style scoped>
.wrap {
  display: flex;
  justify-content: center;
}
.users-data__btn {
  margin: 20px
}
.users-data {
  width: 600px
}
</style>

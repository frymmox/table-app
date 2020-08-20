<template>
  <v-container>
    <v-row>
      <v-col>
        <v-btn color="primary" dark @click="fetchUsers" class="mb-4">Показать пользователей</v-btn>

        <the-table
          v-if="showTable"
          :titles="titles"
          :values="users"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import TheTable from '@/components/TheTable'
  export default {
    name: 'Home',
    data() {
      return {
        titles: [
          {text: 'Name', value: 'fullname'},
          {text: 'User Name', value: 'uname'},
          {text: 'Email', value: 'email'},
          {text: 'Company', value: 'company'}
        ],
        users: [],
        showTable: false,
      }
    },
    components: {
      TheTable
    },
    methods: {
      fetchUsers() {
        fetch('http://www.filltext.com/?rows=1000&id=%7Bindex%7D&fullname=%7BfirstName%7D~%7BlastName%7D&company=Bbusiness&email=%7Bemail%7D&uname=%7Busername%7D&address=%7BaddressObject%7D')
        .then(response => response.json())
        .then(json => {
          this.users = json
          this.showTable = true
        })
      }
    }
  }
</script>

<template>
  <div>
    <v-text-field
      v-model="search"
      label="Search" />

    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th
              v-for="(item, index) in titles"
              :key="index"
              style="cursor: pointer;"
              @click="onSort(item.value)">
                {{ item.text }}
                  <v-icon v-if="sort">mdi-arrow-down</v-icon>
                  <v-icon v-else>mdi-arrow-up</v-icon>
            </th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="item in filteredSearch"
            :key="item.id"
            @click="info(item.id)">
            <td>{{ item.fullname }}</td>
            <td>{{ item.uname }}</td>
            <td>{{ item.email }}</td>
            <td>{{ item.company }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

    <v-pagination
      v-model="page"
      class="my-4"
      :length="pageCount"
      @input="setupPage"/>

    <v-dialog
      v-if="dialog"
      v-model="dialog"
      max-width="500">
      <v-card>
        <v-card-title class="headline">
          {{ currentUser.fullname }}
        </v-card-title>

        <v-card-text>
          <p>Street: {{ currentUser.address.streetAddress }}</p>

          <p>City: {{ currentUser.address.city }}</p>

          <p>State: {{ currentUser.address.state }}</p>
        </v-card-text>

        <v-icon
          style="position: absolute; top: 16px; right: 16px;"
          @click="dialog = false">
          mdi-close
        </v-icon>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  import _ from 'lodash'

  export default {
    props: {
      titles: {
        type: Array,
        required: true
      },
      values: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        sort: false,
        page: 1,
        pageSize: 10,
        pageCount: 0,
        dialog: false,
        currentUser: {},
        search: ''
      }
    },
    computed: {
      filteredSearch() {
        return this.items.filter(value => {
          return value.fullname.match(this.search) || value.uname.match(this.search) || value.email.match(this.search) || value.company.match(this.search)
        })
      },
      allItems() {
        return _.chunk(this.values, this.pageSize)
      },
      items() {
        return this.allItems[this.page - 1] || this.allItems[0]
      }
    },
    methods: {
      onSort(value) {
        switch(value) {
          case 'fullname':
            this.sort = !this.sort
            if (this.sort) {
              this.values.sort((a, b) => {
                if (a.fullname > b.fullname) {
                  return 1;
                }
                if (a.fullname < b.fullname) {
                  return -1;
                }
                return 0;
              })
            } else {
              this.values.sort((a, b) => {
                if (a.fullname < b.fullname) {
                  return 1;
                }
                if (a.fullname > b.fullname) {
                  return -1;
                }
                return 0;
              })
            }
            break

          case 'uname':
            this.sort = !this.sort
            if (this.sort) {
              this.values.sort((a, b) => {
                if (a.uname > b.uname) {
                  return 1;
                }
                if (a.uname < b.uname) {
                  return -1;
                }
                return 0;
              })
            } else {
              this.values.sort((a, b) => {
                if (a.uname < b.uname) {
                  return 1;
                }
                if (a.uname > b.uname) {
                  return -1;
                }
                return 0;
              })
            }
            break

          case 'email':
            this.sort = !this.sort
            if (this.sort) {
              this.values.sort((a, b) => {
                if (a.email > b.email) {
                  return 1;
                }
                if (a.email < b.email) {
                  return -1;
                }
                return 0;
              })
            } else {
              this.values.sort((a, b) => {
                if (a.email < b.email) {
                  return 1;
                }
                if (a.email > b.email) {
                  return -1;
                }
                return 0;
              })
            }
            break

          default:
            this.sort = !this.sort
            if (this.sort) {
              this.values.sort((a, b) => {
                if (a.company > b.company) {
                  return 1;
                }
                if (a.company < b.company) {
                  return -1;
                }
                return 0;
              })
            } else {
              this.values.sort((a, b) => {
                if (a.company < b.company) {
                  return 1;
                }
                if (a.company > b.company) {
                  return -1;
                }
                return 0;
              })
            }
        }
      },
      setupPage(page) {
        this.items = this.allItems[page - 1] || this.allItems[0]
      },
      info(val) {
        this.dialog = true
        this.currentUser = this.values.find(user => user.id === val)
      }
    },
    mounted() {
      this.pageCount = this.allItems.length
    }
  }
</script>

<style scoped>
  .v-icon {
    transition: opacity .4s ease-in-out;
  }

  th .v-icon {
    opacity: 0;
  }

  th:hover .v-icon {
    opacity: 1;
  }
</style>

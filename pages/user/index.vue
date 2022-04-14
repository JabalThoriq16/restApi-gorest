<template>
    <b-container fluid>
        <b-card title="" header-tag="header" footer-tag="footer">
            <template #header>
                <h1 class="mb-0">Daftar Pengguna</h1>
            </template>
                <b-row>
                    <b-col cols="4">
                      <b-form-group
                        >
                          <b-input-group size="sm">
                            <b-form-input
                              id="filter-input"
                              v-model="filter"
                              type="search"
                              placeholder="Search"
                            ></b-form-input>

                            <b-input-group-append>
                              <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
                            </b-input-group-append>
                          </b-input-group>
                        </b-form-group>
                    </b-col>
                    <b-col cols="6"></b-col>
                    <b-col cols="2">
                        <NuxtLink to="/user/form" class="btn btn-primary text-light">Tambah Pengguna</NuxtLink>
                    </b-col>
                </b-row>
                <b-row>
                    <b-col>
                        <b-table
                            class="mt-3 table table-bordered"
                            :items="items"
                            :fields="fields"
                            :current-page="currentPage"
                            :per-page="perPage"
                            :filter="filter"
                            :filter-included-fields="filterOn"
                            :sort-by.sync="sortBy"
                            :sort-desc.sync="sortDesc"
                            :sort-direction="sortDirection"
                            stacked="md"
                            show-empty
                            small
                            @filtered="onFiltered"
                            >
                  
                            <template #cell(name)=row>
                                {{ row.value }}
                            </template>

                            <template #cell(actions)="row">
                                <button  @click="$router.push(`/user/${row.item.id}`)" class="btn btn-success text-light">View</button>
                                <button  @click="$router.push(`/user/edit/${row.item.id}`)" class="btn btn-primary text-light">Update</button>
                                <button  @click="onDelete(`${row.item.id}`)" class="btn btn-danger text-light">Delete</button>
                            </template>
                        </b-table>
                    </b-col>
                </b-row>
            <template #footer>
                <b-col sm="5" md="3" class="my-1">
                    <b-pagination
                    v-model="currentPage"
                    :total-rows="totalRows"
                    :per-page="perPage"
                    align="fill"
                    size="sm"
                    class="my-0"
                    ></b-pagination>
                </b-col>
            </template>
        </b-card>
    </b-container>
  
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ $axios }) {
    const items = await $axios.$get('https://gorest.co.in//public/v2/users?access-token=0359472d637566d3ebfe75360ab86cf27a9fd1661e6a425d5d04ac8f8592f2d6')
    return { items }
  },
    data() {
      return {
        fields: [
          { key: 'id', label: 'Id', sortable: true, sortDirection: 'asc' },
          { key: 'name', label: 'Name', sortable: true, class: 'text-center' },
          { key: 'email', label: 'email', sortable: true, class: 'text-center' },
          { key: 'gender', label: 'Gender', sortable: true, class: 'text-center' },
          { key: 'status', label: 'Status', sortable: true, class: 'text-center' },
          { key: 'actions', label: 'Actions', class: 'text-center col-md-2' }
        ],
        totalRows: 1,
        currentPage: 1,
        perPage: 5,
        sortBy: '',
        sortDesc: false,
        sortDirection: 'asc',
        filter: null,
        filterOn: [],
      }
    },
    computed: {
      sortOptions() {
        // Create an options list from our fields
        return this.fields
          .filter(f => f.sortable)
          .map(f => {
            return { text: f.label, value: f.key }
          })
      }
    },
    mounted() {
      // Set the initial number of items
      this.totalRows = this.items.length
    },
    methods: {
      onDelete(id) {
        axios.delete('https://gorest.co.in//public/v2/users/'+id+'?access-token=0359472d637566d3ebfe75360ab86cf27a9fd1661e6a425d5d04ac8f8592f2d6', this.items)
        .then(function (response) {
          window.location.href='/user'
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });
        // 
        // alert(JSON.stringify(this.form))
        // console.log(form)
      },
      info(item, index, button) {
        this.infoModal.title = `Row index: ${index}`
        this.infoModal.content = JSON.stringify(item, null, 2)
        this.$root.$emit('bv::show::modal', this.infoModal.id, button)
      },
      resetInfoModal() {
        this.infoModal.title = ''
        this.infoModal.content = ''
      },
      onFiltered(filteredItems) {
        // Trigger pagination to update the number of buttons/pages due to filtering
        this.totalRows = filteredItems.length
        this.currentPage = 1
      }
    }
};
</script>

<style></style>

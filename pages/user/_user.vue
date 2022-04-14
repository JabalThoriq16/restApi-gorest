<template>
  <div class="row justify-content-md-center mt-5">
    <b-card title="" header-tag="header" footer-tag="footer" class="col-10">
      <template #header>
        <h1 class="mb-0 text-center">Lihat Pengguna</h1>
      </template>
      <b-row class="justify-content-md-center">
        <b-col>
          <!-- {{$route.params.user}} -->
          <p>Id: {{ datas.id }}</p>
          <p>Nama: {{ datas.name }}</p>
          <p>Gender: {{ datas.gender }}</p>
          <p>Email: {{ datas.email }}</p>

          
          <b-row>
            <b-col cols="10"></b-col>
            <b-col cols="2">
              <b-button variant="success" v-b-modal.modal-prevent-closing>tambah Post</b-button>
              <b-modal id="modal-prevent-closing" ref="modal" title="Create Post" @ok="onSubmit(datas.id)" >
                  <b-form-group id="input-group-2" label="Your Title:" label-for="input-2">
                    <b-form-input id="input-2" v-model="forms.title" placeholder="Enter Title" required></b-form-input>
                  </b-form-group>
                  <b-form-group id="input-group-1" label="Your Body:" label-for="input-2">
                    <b-form-input id="input-1" v-model="forms.body" placeholder="Enter Body" required></b-form-input>
                  </b-form-group>
              </b-modal>  
            </b-col>
          </b-row>
          <b-table class="mt-3 table table-bordered"
            :items="form"
            :fields="fields"
            :current-page="currentPage"
            :per-page="perPage"
            :sort-by.sync="sortBy"
            :sort-desc.sync="sortDesc"
            :sort-direction="sortDirection"
            stacked="md"
            show-empty
            small
          >
            <template #cell(name)="row">
              {{ row.value.first }} {{ row.value.last }}
            </template>

            <template #cell(actions)="">
              <NuxtLink to="/admin/form" class="btn btn-danger text-light"
                >Delete</NuxtLink
              >
            </template>

            <template #row-details="row">
              <b-card>
                <ul>
                  <li v-for="(value, key) in row.item" :key="key">
                    {{ key }}: {{ value }}
                  </li>
                </ul>
              </b-card>
            </template>
          </b-table>
        </b-col>
      </b-row>
      <template #footer> </template>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ $axios, route }) {
    const datas = await $axios.$get(
      '/'+route.params.user+'.json'
    )
    const form = await $axios.$get(
      '/'+route.params.user+'/posts.json'
    )
    return { form , datas }
  },
  data() {
    return {
      forms:{
        title:'', 
        body:''
      },
      nameState: null,
      submittedNames: [],
      datas: [{}],
      fields: [
        { key: 'title', label: 'Title', sortable: true, class: 'text-center' },
        { key: 'body', label: 'Body', sortable: true, class: 'text-center' },
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 5,
      pageOptions: [5, 10, 15, { value: 100, text: 'Show a lot' }],
      sortBy: '',
      sortDesc: false,
      sortDirection: 'asc',
      infoModal: {
        id: 'info-modal',
        title: '',
        content: '',
      },
    }
  },
  methods: {
    onSubmit(id) {
      console.log(this.id)
      axios.post('https://gorest.co.in//public/v2/users/'+id+'/posts?access-token=0359472d637566d3ebfe75360ab86cf27a9fd1661e6a425d5d04ac8f8592f2d6', this.forms)
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity()
      this.nameState = valid
      return valid
    },
    resetModal() {
      this.name = ''
      this.nameState = null
    },
    handleOk(bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault()
      // Trigger submit handler
      this.handleSubmit()
    },
    handleSubmit() {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return
      }
      // Push the name to submitted names
      this.submittedNames.push(this.name)
      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide('modal-prevent-closing')
      })
    },
  },
}
</script>

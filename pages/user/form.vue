<template>
    <div class="row justify-content-md-center mt-5">
        <b-card title="" header-tag="header" footer-tag="footer" class="col-4">
            <template #header>
                <h1 class="mb-0 text-center">Form Pengguna</h1>
            </template>
                <b-row class="justify-content-md-center">
                    <b-col>
                        <!-- <b-form @submit="onSubmit" @reset="onReset" v-if="show"> -->
                            <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
                                <b-form-input
                                id="input-2"
                                v-model="form.name"
                                placeholder="Enter name"
                                required
                                ></b-form-input>
                            </b-form-group>
                            
                            <b-form-group id="input-group-3" label="Gender:" label-for="input-3">
                                <b-form-select
                                id="input-3"
                                v-model="form.gender"
                                :options="gander"
                                required
                                ></b-form-select>
                            </b-form-group>

                            <b-form-group
                                id="input-group-1"
                                label="Email address:"
                                label-for="input-1"
                                description="We'll never share your email with anyone else."
                            >
                                <b-form-input
                                id="input-1"
                                v-model="form.email"
                                type="email"
                                placeholder="Enter email"
                                required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-4" label="Status:" label-for="input-4">
                                <b-form-select
                                id="input-4"
                                v-model="form.status"
                                :options="status"
                                required
                                ></b-form-select>
                            </b-form-group>

                            <b-button type="submit" @click="onSubmit()" variant="primary">Submit</b-button>
                            <b-button type="reset" @reset="onReset" variant="danger">Reset</b-button>
                            <!-- </b-form> -->
                    </b-col>
                </b-row>
            <template #footer>
        
            </template>
        </b-card>
    </div>
    
</template>
<script>
import axios from 'axios'
  export default {
    data() {
      return {
        form: {
          email: '',
          name: '',
          gender: null,
          status:null
        },
        gander: [{ text: 'Select One', value: null }, 'male','female'],
        status: [{ text: 'Select One', value: null }, 'active','inactive'],
        show: true
      }
    },
    methods: {
      onSubmit() {
        
        axios.post('https://gorest.co.in//public/v2/users?access-token=0359472d637566d3ebfe75360ab86cf27a9fd1661e6a425d5d04ac8f8592f2d6', this.form)
        .then(function (response) {
          window.location.href='/user'
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });
        // alert(JSON.stringify(this.form))
        // console.log(form)
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.gender = null
        this.form.status = null
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
    
  }
</script>

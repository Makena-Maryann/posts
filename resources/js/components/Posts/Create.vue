<template>
   <div>
      <form @submit.prevent="submit_form">
         Post title:
         <br>
         <input type="text" v-model="fields.title" class="form-control">
         <div class="alert alert-danger" v-if="errors && errors.title">
               {{ errors.title[0] }}
         </div>
         <br>
         Post text:
         <br>
         <textarea rows="10" v-model="fields.post_text" class="form-control"></textarea>
         <div class="alert alert-danger" v-if="errors && errors.post_text">
               {{ errors.post_text[0] }}
         </div>
         <br>
         Category:
         <select v-model="fields.category_id" class="form-control">
            <option v-for="category in categories" :key="category.id" :value="category.id">{{ category.name }}</option>
         </select>
         <div class="alert alert-danger" v-if="errors && errors.category_id">
               {{ errors.category_id[0] }}
         </div>
         <br>
         <input type="submit" class="btn btn-primary" :value="form_submitting ? 'Saving post...' : 'Save post'" :disabled="form_submitting">
      </form>
   </div>
</template>

<script>
    export default {
       data() {
          return {
             categories: {},
             fields: {
                title: '',
                post_text: '',
                category_id: ''
             },
             errors: {},
             form_submitting: false,
          }
       },

       mounted() {
          axios.get('/api/categories')
            .then(response => {
               this.categories = response.data.data;
            });
       },

       methods: {
          submit_form() {
             this.form_submitting = true;
            axios.post('/api/posts', this.fields)
               .then(response => {
                  this.$router.push('/');
                  this.form_submitting = false;
               }).catch(error => {
                  if (error.response.status === 422) {
                     this.errors = error.response.data.errors;
                     this.form_submitting = false;
                  }
               });
          }
       }
    }
</script>

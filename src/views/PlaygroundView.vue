<template>
    <div>
        <input v-model="formValues.complexity" placeholder="edit me"></input>
        <input v-model="formValues.civil" placeholder="edit me"></input>
        <input v-model="formValues.price" placeholder="edit me"></input>
        <input v-model="formValues.otd4" placeholder="edit me"></input>
        <input v-model="formValues.otd9" placeholder="edit me"></input>
        <button @click="submit"> {{ formValues.text }}</button>
        <p>{{formValues.complexity}}</p>
    </div>

    
</template>

<script>

import axios from "axios";

export default {

  data() {
    return {
      formValues: {
        complexity: '',
        civil:'',
        price:'',
        otd4:'',
        otd9:'',
        text:'Submit',
      }
    }
  },
  methods: {
    
    submit() {
        // `this` inside methods points to the current active instance
        //this.formValues.text = 'I was clicked'
        
        axios
           
          .post('https://we4s31ivk9.execute-api.eu-central-1.amazonaws.com/prod/predictions',
            //{ data:'415,1,3000000,1,1'},
            {
              "data":"this.complexity, this.civil, this.price, this.otd4, this.otd9"
            },
            {
              headers:{
                'Content-Type':'text/csv',
              }
            }
            
          )
          
          .then((response) => {
            console.log(response);
           })

          .catch(function (error) {
            if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            console.log(error.response.data);
            console.log(error.response.status);
            console.log(error.response.headers);
            } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request);
            } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message);
            }
            console.log(error.config);
        });

         
        
    }
  }
}


</script>


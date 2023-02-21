<template>
  
  <v-row justify="center">
   
    <v-col
      cols="12"
      sm="10"
      md="8"
      lg="6"
    >
      <v-container>
        <a href="./#/">
            Go back
            </a><br><br>
        
        <h1>Energized.AI</h1>
        <p>In 2023 alone, during the construction of hundreds of energy projects worldwide, 5.2B$ will be lost in project inneficiencies and custumer penalties.
         Our solution with <strong>Energized.ai</strong> intends to reaccomodate that capital, investing it back, bringing a total of 400GW per year back to the network, already thought lost. 
         Our solution was trained on over 25,000 energy projects and worked alongside several technical and business experts in the energy industry. We've identify five core predictors shown below.
        </p>
        <p>You can try it yourself. Use the form below to input the values for your project, the model 
         will return a probability score of how likely the project will be delayed:
        </p>
      </v-container>
      <v-card ref="form">
        <v-card-text>
          <v-text-field
            ref="Complexity"
            v-model="FormValues.Complexity"
            :rules="[() => !!Complexity || 'This field is required']"
            :error-messages="errorMessages"
            label="Project Complexity is a standard measure normally between 250 and 800"
            placeholder="600"
            required
          ></v-text-field>
          <v-text-field
            ref="Civil Works"
            v-model="FormValues.Civil_Works"
            :rules="[
              () => !!Civil_Works || 'This field is required']"
            label="Are there any civil structures needed?"
            placeholder="Yes or No"
            required
            persistent-hint
            return-object
            single-line>
          </v-text-field>
          <v-text-field
            ref="price"
            v-model="FormValues.Price"
            :rules="[() => !!prices || 'This field is required']"
            label="Total price of the Project"
            placeholder="4 millions"
            required
            return-object
          ></v-text-field>
          <v-text-field
            ref="Milestone#4"
            v-model="FormValues.milestone4"
            :rules="[() => !!milestone4 || 'This field is required']"
            label="Project delayed in Milestone #4?"
            required
            placeholder="Middle East"
            return-object
          ></v-text-field>
          <v-text-field
            ref="Milestone#9"
            v-model="FormValues.milestone9"
            :rules="[() => !!milestone9 || 'This field is required']"
            label="Project delayed in Milestone #9?"
            required
            placeholder="Middle East"
            return-object
          ></v-text-field>
          <!--<v-text-field
            ref="zip"
            v-model="zip"
            :rules="[() => !!zip || 'This field is required']"
            label="ZIP / Postal Code"
            required
            placeholder="79938"
          ></v-text-field>
          <v-autocomplete
            ref="country"
            v-model="country"
            :rules="[() => !!country || 'This field is required']"
            :items="countries"
            label="Country"
            placeholder="Select..."
            required
          ></v-autocomplete>-->

        </v-card-text>
        <v-divider class="mt-12"></v-divider>
        <v-card-actions>
          <v-btn text>
            Cancel
          </v-btn>
          <v-spacer></v-spacer>
          <v-slide-x-reverse-transition>
            <v-tooltip
              v-if="formHasErrors"
              left
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  icon
                  class="my-0"
                  v-bind="attrs"
                  @click="resetForm"
                  v-on="on"
                >
                  <v-icon>mdi-refresh</v-icon>
                </v-btn>
              </template>
              <span>Refresh form</span>
            </v-tooltip>
          </v-slide-x-reverse-transition>
          <v-btn
            v-model="FormValues"
            @click="ok(FormValues)"
            color="primary"
            ButtonText
          >
           {{ FormValues.ButtonText }}
          </v-btn>
        </v-card-actions>

        <div>{{ FormValues.Civil_Works}}</div>

        
      
      
      </v-card> <!-- form wrapper closed -->
      <v-container v-show="seen" @click="ok">
        <!--<p>{{ Complexity }}</p>
        <p>{{ Civil_Works }}</p>
        <p>{{ prices }}</p>
        <p>{{ milestone4 }}</p>
        <p>{{ milestone9 }}</p>-->
        <p> {{ FormValues.Civil_Works}} </p>
      </v-container>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";



export default {

  

  data() {
    return {
      seen:false,

      items_civil_works:['1', '0'],
      items_prices:['2000000', '5000000', '15000000', '40000000'],
      items_milestone4:['1', '0'],
      items_milestone9:['1', '0'],
      res:[],

      FormValues: {
        Complexity:'',
        Civil_Works:'',
        Price:'',
        ButtonText:'Submit',
        milestone4:'',
        milestone9:'',

      },
    }
  },

  methods: {

    ok() {
      this.FormValues.ButtonText = 'Generate Another Inference'
      this.seen=true
      
      axios
      
          .post("https://we4s31ivk9.execute-api.eu-central-1.amazonaws.com/prod/predictions",
          {"data":"415,1,2000000,1,1"},
          
          
          {
            mode: 'no-cors',
            headers:{
              "Content-Type":"multipart/form-data"
            }
          }
          )
          
          .then((response) => {
            console.log(response);
          })
          .use(request => {
            console.log('Starting Request', JSON.stringify(request, null, 2))
            return request
          })


    },

    

   

      

      /*let res = await axios.post('http://httpbin.org/post', payload);

      let data = res.data;*/
      
  }
  
   
  
    
};
</script>
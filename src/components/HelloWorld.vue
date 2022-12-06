<template>
  <div>
  <TopNavBar :form-id="form.data[0].form"/>
  
  <v-container class="container">
    <v-tabs v-model="tab">
      <v-tab>Item One</v-tab>
    <v-tab>Item Two</v-tab>
   
   
  </v-tabs>
  <v-tabs-items v-model="tab">
      <v-tab-item>
        <v-card flat>
          <v-card-text >Hi</v-card-text>
        </v-card>
      </v-tab-item>

      <v-tab-item>
        <v-card flat>
          <v-card-text >Hel;lo</v-card-text>
        </v-card>
      </v-tab-item>
    </v-tabs-items>

    <v-card elevation="2" v-for="(q , i) in questions" :key="i" style="padding:30px">

      <v-row align="center">
        <v-col class="d-flex" cols="6" sm="6">
          <v-text-field :label="questions[i].question" v-model="questions[i].question"></v-text-field>

        </v-col>
        <v-col class="d-flex" cols="6" sm="6">

          <v-select v-model="q.question_type" :items="items" :value="q.question_type" label="Question Type">
          </v-select>
        </v-col>
      </v-row>
      <div v-if="q.question_type == 'short answer'">
        <v-text-field label="Another input"></v-text-field>
      </div>
      <div v-if="q.question_type == 'paragraph'">
        <v-textarea name="input-7-1" label="Default style"
          value="The Woodman set to work at once, and so sharp was his axe that the tree was soon chopped nearly through."
          hint="Hint text"></v-textarea>
      </div>

      <div v-if="q.question_type == 'multiple choice'">
        <v-row align="center" v-for="(option,j) in q.choices" :key="j">
          <v-radio></v-radio>
          <v-text-field :value="option.choice"></v-text-field>
          <v-icon v-on:click="removeChoice(option.id)">mdi-close</v-icon>
        </v-row>
        <v-text-field placeholder="Add option" v-on:click="addOption(i)"></v-text-field>
      </div>

      <div v-if="q.question_type == 'checkbox'">
        <v-row align="center" v-for="(option,i) in q.choices" :key="i">
          <v-checkbox></v-checkbox>
          <v-text-field :value="option.choice" v-on:input="editChoice"></v-text-field>
          <v-icon v-on:click="removeChoice(option.id)">mdi-close</v-icon>
          <v-text-field placeholder="Add option" v-on:click="addOption(i)"></v-text-field>
        </v-row>

      </div>



    </v-card>


    <v-btn style="margin-top: 40px;" p-4 elevation="2" v-on:click="addQuestion()">Click</v-btn>
  </v-container>
</div>

</template>

<script>
import TopNavBar from './TopNavBar.vue';
  export default {

    name: 'HelloWorld',
    components: {
    TopNavBar
  },

    data: () => ({
      'items': ['short answer', 'multiple choice', 'checkbox', 'paragraph'],
      'form': [],
      'questions': [],
      'question': {
        'question': 'Untitled Question',
        'options': ['Option 1'],
        'question_type': 'radio'
      },
      tab: null,
        tabs: [
          'form', 'responses'
        ],
    }),
    methods: {
      editChoice(){
        console.log('aa')
      },
      addOption(i) {
        var _this = this
      
        console.log(_this.questions[i])


        fetch('http://127.0.0.1:8000/choices/', {
            method: 'post',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              'form': _this.form.data[0].form,
              'question': _this.questions[i].id,
              'choice': `Option ${_this.questions[i].choices.length}`
            })
          }).then(response => response.json())
          .then(result => {

            console.log(result)
            _this.fetchForm()
          })

        this.questions[i].choices = [...this.questions[i].choices, {
          'choice': "Option 1",
          "id": 90,
          "is_answer": false
        }]

      },
      removeChoice(id) {
        var _this = this
        fetch('http://127.0.0.1:8000/choices/', {
            method: 'delete',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              'id': id
            })
          }).then(response => response.json())
          .then(result => {

            console.log(result)
            _this.fetchForm()
          })
      },
      addQuestion() {
        var question = {
          "form": 13
        }
        var _this = this

        fetch('http://127.0.0.1:8000/question/', {
            method: 'post',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(question)
          }).then(response => response.json())
          .then(result => {
            _this.questions.push(result.data)
            console.log(result.data)
          })

        // this.questions.push(question)
        console.log(this.question)
      },

      fetchForm() {
        var _this = this
        fetch(`http://127.0.0.1:8000/api/form/?id=13`)
          .then(response => response.json())
          .then(result => {
            _this.form = result
            _this.questions = result.data[0].questions
            console.log(result.data[0].questions)
          })
      },


      makeid(length = 5) {
        var result = '';
        var characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
        var charactersLength = characters.length;
        for (var i = 0; i < length; i++) {
          result += characters.charAt(Math.floor(Math.random() * charactersLength));
        }
        return result;
      },
      updateQuestion() {


      }
    },

    created() {
      this.fetchForm()
    }

  }
</script>
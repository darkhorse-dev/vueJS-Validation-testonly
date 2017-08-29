<template>
  <div class="container">
    <h2>Simple Validation Form</h2>
    <div class="form-inline" style="margin-bottom: 10px">
      <h4>Validation Rules</h4>
      <div class="form-group">
        <label>Min Length : </label>
        <input type="number" name="minlength" v-model="min_length" class="form-control">    
      </div>
      <div class="form-group">
        <label>Max Length : </label>
        <input type="number" name="maxlength" v-model="max_length" class="form-control"> 
      </div>
    </div>
    <form class="form-inline">
      <div class="form-group">
        <label for="position">Position address : </label>
        <input type="number" class="form-control" id="position" v-model="position">
      </div>
      <div class="form-group">
        <label for="value">Value : </label>
        <b-form-select v-model="selected" :options="options">
          </b-form-select>
      </div>
      <div v-show="selected == ''" class="form-group">
          <label>Custom Value</label>
          <input class="form-control" type="text" v-model="value">  
        </div>
      <b-button class="btn btn-primary" @click="addRule(selected)">Add rule</b-button>
    </form>
    <table class="table" align="center">
      <thead>
        <tr>
          <th>No.</th>
          <th>Rule</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(rule, index) in rules">
          <td> {{ index+1 }}</td>
          <td align="left"> {{ (rule.position + 1) }} th character must be {{ rule.value }}</td>
          <td> <b-button @click="deleteRule(index)">Delete</b-button> </td>
        </tr>
      </tbody> 
    </table>
    <h4>Input ID</h4>
    <form @submit.prevent="handleIt">
      <input type="text" name="id" v-model="id" class="form-control">
      <p class="error" v-show="!checkValid()">
        {{ err_msg }}
      </p>
      <br>
      <p>
        <button class="btn btn-primary btn-block input-button" type="submit">Submit</button>  
      </p>
    </form>
    <h6>@ It was just made for test only @</h6>
  </div>
</template>

<script>
export default {
  data () {
    return {
      max_length: 10,
      min_length: 0,
      id: '',
      err_msg: '',
      selected: null,
      position: null,
      value: null,
      options: [
        { value: null, text: 'Please select one.' },
        { value: 'number', text: 'number' },
        { value: 'letter', text: 'letter' },
        { value: '', text: 'certain value' },
      ],
      rules: [
       {position: 1, value: 'number'},
       {position: 3, value: 'letter'},
       {position: 5, value: 'L'},
      ],
    }
  },
  methods: {

    isExist: function(pos) {
      for (var i = 0; i < this.rules.length; i++) {
        if ( this.rules[i].position == pos ) {
          return true;
        }
      }
      return false;
    },
    addRule: function(selected) {
      if (this.position == null || this.selected == null || this.position == 0) {
        alert('Input correctly!');
        return;
      }
      if (this.isExist(this.position-1)) {
        alert('Position rule already exist! Delete existing one and recreate it');
        return;
      }
      if (this.position > this.max_length || this.position < this.min_length) {
        alert('Enter valid position!');
        return;
      }
      if (selected == '') {
        this.rules.push({ position: this.position - 1 , value: this.value });  
      } else {
        this.rules.push({ position: this.position - 1, value: this.selected });  
      }
      this.value = null;
      this.position = null;
    },
    deleteRule: function(id) {
      this.rules.splice(id, 1);
    },
    handleIt: function() {
      if ( this.checkValid() ) {
        alert('Submitted Succesfully');
      } else {
        alert('Please fix following errors');
      }
    },
    isCharDigit: function(n) {
      return !!n.trim() && n > -1;
    },
    checkLength: function() {
      if ( this.id.length > this.max_length || this.id.length < this.min_length ) {
        return false;
      } else {
        return true;
      }
    },
    checkValid: function() {
      if (!this.checkLength()) {
        this.err_msg = 'The length of ID must be in ' + this.min_length + ' and ' + this.max_length + ' !';
        return false;
      }
      for (var i = 0; i < this.rules.length; i++) {
        console.log('asdfasdf');
        let char = this.id.charAt(this.rules[i].position);
        console.log(char);
        switch(this.rules[i].value) {
          case 'number':
            if(!this.isCharDigit(char)) {
              this.err_msg = 'The ' + (this.rules[i].position + 1) + 'th character must be a number';
              return false;
            };
            break;
          case 'letter':
            if(this.isCharDigit(char)) {
              this.err_msg = 'The ' + (this.rules[i].position + 1)+ 'th character must be a letter';
              return false;
            };
            break;
          default:
            if (char !== this.rules[i].value) {
              this.err_msg = 'The ' + (this.rules[i].position + 1)+ 'th character must be a ' + this.rules[i].value;
              return false;
            }
        }
      }
      return true;
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  .error {
    color: #ee1133;
  }

  .id_input {
    font-size: 18pt;
    height: 50px;
    width: 300px;
  }

  .input-button {
    height: 40px;
    width: 100px;
  }

  .validation_rules {
    padding: 5px;
    margin: 10px;
  }

</style>

<template>
  <div class="hello">
    <h2>Simple Validation Form</h2>
    <form @submit.prevent="handleIt">
      <input type="text" name="id" v-model="id" class="id_input">
      <br>
      <p class="error" v-show="!checkValid()">
        {{ err_msg }}
      </p>
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
      MAX_LENGTH: 20,
      MIN_LENGTH: 10,
      id: '',
      err_msg: '',
    }
  },
  methods: {
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
      if ( this.id.length > this.MAX_LENGTH || this.id.length < this.MIN_LENGTH ) {
        return false;
      } else {
        return true;
      }
    },
    check5thChar: function() {
      return this.isCharDigit(this.id.charAt(4));
    },
    check3thChar: function() {
      return !this.isCharDigit(this.id.charAt(2));
    },
    check4thChar: function() {
      if ( this.id.charAt(3).toLowerCase() !== 'l' ) {
        return false;
      } else {
        return true;
      }
    },
    checkValid: function() {
      if ( !this.checkLength() ) {
        this.err_msg = 'The ID length must be between ' + this.MIN_LENGTH + ' and ' + this.MAX_LENGTH;
        return false;
      } else if ( this.id.length >= 5 && !this.check5thChar() ) {
        this.err_msg = 'The 5th character must be a number';
        return false;
      } else if ( this.id.length >= 3 && !this.check3thChar() ) {
        this.err_msg = 'The 3th character must be a letter';
        return false;
      } else if ( this.id.length >= 4 && !this.check4thChar() ) {
        this.err_msg = " The 4th character must be a letter 'L' or 'l' ";
        return false;
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

</style>

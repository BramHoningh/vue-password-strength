<template>
<div :class="inputGroupClass">
  <div :class="aboveInputClass">
    <label :class="labelClass" :for="id">{{labelText}}</label>
    <div :class="passwordStrengthClass">
      <div :class="[passwordStrengthInnerClass, getScoreColorClass]"
        :style="{'width': getWidthByScore}"
      ></div>
    </div>
  </div>
  <input
    :id="id"
    :class="inputClass"
    :type="type"
    :placeholder="placeholder"
    :value="value"
    :required="required"
    v-model="password"
    @input="calculateScore"
  />
</div>
</template>

<script>
import zxcvbn from 'zxcvbn'

export default {
  name: 'DHKPasswordInput',
  props: {
    /**
    * Input and label-for id.
    * Default is 'input-password'
    * @type {String}
    */
    id: {
      type: String,
      default: 'input-password'
    },
    /**
    * Input type.
    * Default is password.
    * @type {String}
    */
    type: {
      type: String,
      default: 'password'
    },
    /**
    * Input placeholder.
    * Default is 'Enter your password'.
    * @type {String}
    */
    placeholder: {
      type: String,
      default: 'Enter your password'
    },
    /**
    * Input value.
    * @type {String}
    */
    value: {
      type: String
    },
    /**
    * Input field required flag.
    * Default is false.
    * @type {Boolean}
    */
    required: {
      type: Boolean,
      default: false
    },
    /**
    * Text displayed in the label.
    * Default is 'Password'.
    * @type {String}
    */
    labelText: {
      type: String,
      default: 'Password'
    },
    /**
    * CSS Classname for overall container.
    * Default is 'dhk-password-input'.
    * @type {String}
    */
    inputGroupClass: {
      type: String,
      default: 'dhk-password-input'
    },
    /**
    * CSS Classname for wrapper around the
    * label and password strength indicator.
    * Default is 'dhk-password-above-input'.
    * @type {String}
    */
    aboveInputClass: {
      type: String,
      default: 'dhk-password-above-input'
    },
    /**
    * CSS Classname for the label element.
    * Default is 'dhk-input-label'.
    * @type {String}
    */
    labelClass: {
      type: String,
      default: 'dhk-input-label'
    },
    /**
    * CSS Classname for the input element.
    * Default is 'dhk-input'.
    * @type {String}
    */
    inputClass: {
      type: String,
      default: 'dhk-input'
    },
    /**
    * CSS Classname for password strength indicator container.
    * Default is 'dhk-password-strength-container'.
    * @type {String}
    */
    passwordStrengthClass: {
      type: String,
      default: 'dhk-password-strength-container'
    },
    /**
    * CSS Classname for password strength indicator
    * inner div element (this div displays the color).
    * Default is 'dhk-password-strength-inner'.
    * @type {String}
    */
    passwordStrengthInnerClass: {
      type: String,
      default: 'dhk-password-strength-inner'
    }
  },
  data () {
    return {
      password: '',
      score: 0
    }
  },
  computed: {
    /**
    * Generates the width of the scorebar.
    * @return {String} Width of scorebar in percentage.
    */
    getWidthByScore () {
      return this.score * 25 + '%'
    },

    /**
    * Generates the correct classname to display the right color for each score.
    * @return {String} Classname for backgroundcolor.
    */
    getScoreColorClass () {
      let classPrefix = 'dhk-score-color-'
      let classSuffix = (this.score === 1) ? 'dark-red' : (this.score === 2) ? 'red' : (this.score === 3) ? 'orange' : (this.score === 4) ? 'green' : 'blank'
      return classPrefix + classSuffix
    }
  },
  methods: {
    /**
    * Calculates the score using zxcvbn and emits the score, user input.
    * Also emits the feedback if there is any.
    */
    calculateScore() {
      let calculation = zxcvbn(this.password)
      this.score = calculation.score

      this.$emit('score', calculation.score)
      this.$emit('inputChange', this.password)

      if (calculation.feedback.warning || calculation.feedback.suggestions.length) {
        this.$emit('feedback', calculation.feedback)
      }
    }
  }
}
</script>

<style lang="css" scoped>
.dhk-password-input {
  min-width: 250px;
  max-width: 400px;
}

label.dhk-input-label {
  display: inline-block;
  margin-left: 3px;
  font-size: 18px;
  line-height: 0.78;
  color: #11162a;
}

input.dhk-input {
  display: block;
  position: relative;
  margin-top: 8px;
  padding: 10px 10px;
  width: 100%;
  font-size: 14px;
  color: #11162a;
  border-radius: 2px;
  outline: none;
  border: solid 1px rgba(17, 22, 42, 0.16);
  box-shadow: inset 0 1px 2px 0 rgba(17, 22, 42, 0.04);
  box-sizing: border-box;
}

.dhk-password-above-input {
  position: relative;
}

.dhk-password-strength-container {
  position: absolute;
  top: 50%;
  right: 3px;
  transform: translateY(-50%);
  width: 88px;
  height: 4px;
  background-color: #e3e6e9;
  border-radius: 4px;
}

.dhk-password-strength-inner {
  height: 4px;
  border-radius: 4px;
  transition: all .5s ease-in-out;
}

.dhk-score-color-dark-red {
  background-color: #9e0000;
}

.dhk-score-color-red {
  background-color: #ff5c58;
}

.dhk-score-color-orange {
  background-color: #f79627
}

.dhk-score-color-green {
  background-color: #50ec99;
}
</style>


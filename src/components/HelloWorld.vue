<template>
  <div class="hello">
    <h1>Validation of inputed IP and MAC + input mask</h1>

    <div class="container">
      <form action="" @submit.prevent="submit">
        <div class="input-group my-2">
          <div class="input-group-prepend">
            <span class="input-group-text" id="addon">IP: </span>
          </div>
          <input
            inputGroup
            :formOutline="false"
            :value="ipvalue"
            v-imask="ipmask"
            @accept="
              (e) => {
                ipvalue = e.detail.value;
              }
            "
            class="form-control"
            :class="[isValid(regIP, ipvalue) ? '' : 'border-danger']"
            required
            aria-describedby="addon"
          />
        </div>
        <br />
        <div class="input-group my-2">
          <div class="input-group-prepend">
            <span class="input-group-text" id="addon">MAC: </span>
          </div>
          <input
            inputGroup
            :formOutline="false"
            :value="macvalue"
            v-imask="macmask"
            @accept="
              (e) => {
                macvalue = e.detail.value;
              }
            "
            class="form-control"
            :class="[isValid(regMAC, macvalue) ? '' : 'border-danger']"
            required
            aria-describedby="addon"
          />
        </div>
        <input
          :disabled="!isValid(regMAC, macvalue) || !isValid(regIP, ipvalue)"
          class="btn btn-primary"
          type="submit"
          value="Submit"
        />
      </form>
    </div>
  </div>
</template>

<script>
import { IMaskDirective } from "vue-imask";

export default {
  name: "HelloWorld",
  data() {
    return {
      ipvalue: "",
      macvalue: "",
      ipIsValid: true,
      macIsValid: true,
      ipmask: {
        mask: "0[00]{.}0[00]{.}0[00]{.}0[00]",
        lazy: false,
      },
      macmask: {
        mask: "**{:}**{:}**{:}**{:}**{:}**",
        lazy: false,
      },
      regIP: "^((25[0-5]|(2[0-4]|1\\d|[1-9]|)\\d)(\\.(?!$)|$)){4}$",
      regMAC: "^[0-9a-fA-F]{2}([\\.:-])(?:[0-9a-fA-F]{2}\\1){4}[0-9a-fA-F]{2}$",
    };
  },
  methods: {
    onAccept(e) {
      this.ipvalue = e.detail.value;
    },
    isValid(reg, input) {
      return input !== "" ? new RegExp(reg).test(input) : true;
    },
  },
  directives: {
    imask: IMaskDirective,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
.border-danger {
  border-bottom: 5px solid red;
}
</style>

<template>
<div>
<form id="userForm" :class="hidden ? 'hiddenClas' : ''">
  <div class="data">
      <div class="responsive">
        <div class="inputErrorName">
            <input type="text" id="name" placeholder="name" v-model="name">
            <p v-for="requiredname in namerequired" :key="requiredname" id="errorname" :class="errorclass ? 'error' : ''">{{ requiredname }}</p>
        </div>
        <div class="inputErrorEmail">
            <input type="text" id="email" placeholder="email" v-model="email">
            <p v-for="error in emailerrors" :key="error" id="erroremail" :class="errorclass ? 'error' : ''">{{ error }}</p>
        </div>
      </div>
      <div class="inputErrorMessage">
          <textarea name="message" id="message" placeholder="message" v-model="message"></textarea>
          <p v-for="requiredmessage in messagerequired" :key="requiredmessage" id="errormessage" :class="errorclass ? 'error' : ''">{{ requiredmessage }}</p>
      </div>
  </div>
  <input type="button" value="Submit" id="submit" @click="onSubmit">
</form>
<p id="finalMessage">{{ messages }}</p>
</div>
</template>

<script>
export default {
  name: 'formsend',
  data() {
          return {
            name: '',
            email: '',
            message: '',
            messages: '',
            errorclass: false,
            hidden: false,
            emailerrors: [],
            namerequired: [],
            messagerequired: [],
            regex: /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/
          }
      },
      methods: {
          onSubmit() {
              this.emailerrors = [],
              this.namerequired = [],
              this.messagerequired = []
              if (this.name && this.email && this.message) {
                let self = this;
                let hide = this;
                  if (!this.email.match(this.regex)) {
                    this.errorclass = true;
                    return this.emailerrors.push("email invalid");
                  }
                  this.axios.post('http://greenhorsegames.com/tests/frontend/process.php', {
                      name: this.name,
                      email: this.email,
                      message: this.message,
                  })
                  .then(function (res) {
                    console.log('SUCCESS: ', res);
                    hide.hidden = true;
                    self.messages = "Your message was successfully sent!"
                  })
                  .catch(function (err) {
                    console.log('ERROR: ',err);
                    self.messages = "Your message wasn't sent!"
                  });
              } else {
                  if (this.name == "" || this.name == null) {
                    this.errorclass = true;
                    this.namerequired.push("name is required");
                  }
                  if (this.message == "" || this.message == null) {
                    this.errorclass = true;
                    this.messagerequired.push("message is required");
                  }
                  if(this.email == "" || this.email == null) {
                    this.errorclass = true;
                    this.emailerrors.push("email is required");
                  } else if (!this.email.match(this.regex)) {
                    this.errorclass = true;
                    return this.emailerrors.push("email invalid");
                  }
              }
          }
      }
}
</script>

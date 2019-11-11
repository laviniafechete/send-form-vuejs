<template>
    <div>
        <form id="userForm" v-if="show">
            <div class="data">
                <div class="responsive">
                    <div class="inputName">
                        <input type="text" id="name" placeholder="name" v-model="name">
                        <p id="errorname" class="error" v-if="errorName">{{errorName}}<span></span></p>
                    </div>
                    <div class="inputEmail">
                        <input type="text" id="email" placeholder="email" v-model="email">
                        <p id="erroremail" class="error" v-if="errorEmail">{{errorEmail}}<span></span></p>
                    </div>
                </div>
                <div class="inputMessage">
                    <textarea name="message" id="message" placeholder="message" v-model="message"></textarea>
                    <p id="errormessage" class="error" v-if="errorMessage">{{errorMessage}}</p>
                </div>
            </div>
            <input type="button" value="Submit" id="submit" @click="submitForm">
        </form>
        <p v-if="!show" id="finalMessage">{{ messages }}</p>
    </div>
</template>

<script>
    export default {
        name: 'formsend',
        data() {
            return {
                errorName: '',
                errorEmail: '',
                errorMessage: '',
                name: '',
                email: '',
                message: '',
                messages: '',
                show: true
            }
        },
        methods: {
            submitForm(){
                this.axios.post('http://greenhorsegames.com/tests/frontend/process.php', {
                    name: this.name,
                    email: this.email,
                    message: this.message
                }).then(response => {
                    this.errorName = response.data.errors.name;
                    this.errorEmail = response.data.errors.email;
                    this.errorMessage = response.data.errors.message;
                    this.messages = "Your message was successfully sent!";
                    if(!this.errorName && !this.errorEmail && !this.errorMessage) {
                      this.show = false;
                    }
                }).catch(error => {
                  if (error.status == 422){
                    this.messages = "Error"
                   }
                })
            }
        }
    }
</script>

<template>
    <section id="contact-us" class="contact">
            <div class="container">
                <h2 class="text-center">Book a
                    <strong>FREE</strong> consultation</h2>
                <br>
                <p class="text-center">It really is
                    <strong>win-win</strong>!<br>
                     <div class="center">
                    <ul>
                        <li>
                            <p>
                                <i class="fas fa-check text-highlight"></i> You get to ask our experts for advice without making any commitments.</p>
                        </li>
                        <li>
                            <p>
                                <i class="fas fa-check text-highlight"></i> It helps us because we get to understand what our clients are thinking about.</p>
                        </li>
                        <li>
                            <p>
                                <i class="fas fa-check text-highlight"></i> No SALES people here, just technical experts.</p>
                        </li>

                    </ul>
                     </div>
                </p>
                <p class="text-center">
                    <span class="text-orange">Call us: </span>  <a href="tel:0800 030 6674"> <strong>0800 030 6674</strong> </a> </p>

                <div class="row">
                    <div class="col-md-8 offset-md-2">
                        <form id="contact-form" @submit="submitForm" action="https://r84zh9giqa.execute-api.eu-west-1.amazonaws.com/dev/contactUsHandler" method="post">
                            <div class="row">
                                <div class="col-md-12">
                                    <p class="text-center">
                                        <span>Or contact us:</span>
                                    </p>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="name" class="sr-only">Name</label>
                                        <input required type="text" class="form-control" name="name" id="name" placeholder="Name" v-model="input.name">
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="email" class="sr-only">Email</label>
                                        <input required type="email" class="form-control" name="email" id="email" placeholder="Email" v-model="input.email">
                                    </div>
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="message" class="sr-only">Message</label>
                                <textarea required name="message" rows="5" class="form-control" id="message" placeholder="Message" v-model="input.message"></textarea>
                            </div>
                            <button :disabled="loading" type="submit" class="btn">Send</button>
                        </form>
                        <div class="alert alert-danger" v-if="hasErrors" v-text="error"></div>
                    </div>
                </div>
            </div>
        </section>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      input: {
        name: '',
        email: '',
        message: ''
      },
      hasErrors: false,
      error: '',
      loading: false
    }
  },

  watch: {
    input: {
      handler () {
        this.loading = false
        this.hasErrors = false
      },
      deep: true
    }
  },

  methods: {
    submitForm (event) {
      event.preventDefault()
      const self = this
      self.loading = true
      let emailReg = /^([a-zA-Z0-9\\.]+)@([a-zA-Z0-9\\-\\_\\.]+)\.([a-zA-Z0-9]+)$/i
      let input = self.input

      if (input.name === '' || !emailReg.test(input.email) || input.message === '') {
        self.hasErrors = true
        self.error = 'Please fill in the form correctly.'
      } else {
        axios.post(event.target.action, input)
          .then(response => {
            self.$router.push('thank-you')
            self.loading = false
          })
          .catch(error => {
            console.log(error)
            self.hasErrors = true
            self.error = 'Something went wrong. Please try again some other time'
          })
      }
    }
  }
}
</script>

<style scoped>
.alert {
  margin-top: 30px;
}
.contact {
  background: #2c302e;
  color: white;
  padding: 6vw 0;
  position: relative
}

@media (max-width: 767px) {
  .contact {
      padding: 5rem 0
  }
}

.contact strong {
  color: #228f42;
  font-weight: bolder
}

.contact p {
  font-size: 2rem
}

@media (max-width: 767px) {
  .contact p {
      font-size: inherit
  }
}

.contact ul {
  padding-left: 0
}

.contact ul li {
  list-style: none;
  text-align: left
}

.contact ul li .fa-check {
  font-size: 1.5rem;
  margin-right: 0.5rem
}

.contact ul li p {
  font-size: 1.5rem
}

.contact ul li .fa-check,
.contact ul li p {
  font-size: inherit
}

.contact form input,
.contact form textarea,
.contact form button {
  border-radius: 0
}

.contact form input {
  height: 2.81rem;
  font-size: 1em;
}

.contact form button {
  background: #d64933;
  color: white;
  min-width: 100px;
  text-transform: uppercase;
  padding: 0.7em 1.3em;
}

.contact form .alert {
  margin-top: 1.2em;
}
</style>


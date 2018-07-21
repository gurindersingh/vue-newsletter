<template>
    <div class="w-100">
        <slot :app="this">
            <h3 class="ta-c fsz-md c-primary tt-u ls-11 mb-3">
                <span class="fw-100">Sign up for </span><strong>promotions and update</strong>
            </h3>
            <form class="d-f ai-c jc-c mx-auto" style="width: 500px; max-width: 90%" v-if="!done"
                  @submit.prevent="signup()">
                <div class="form-group mb-0 p-rel fxg-1">
                    <input type="email"
                           class="form-control bdr-full pr-5"
                           id="newsletter-input"
                           v-model="email"
                           placeholder="Enter email and hit enter...">
                    <button v-if="!processing" class="p-ab p-0 m-0 bd-n bg-n fc:otl-n cur-p" type="submit"
                            style="top: 7px; right: 10px">
                        <svg class="" width="40px" height="25px" style=""
                             viewBox="0 0 40 35"
                             version="1.1" xmlns="http://www.w3.org/2000/svg"
                             xmlns:xlink="http://www.w3.org/1999/xlink">
                            <defs>
                                <linearGradient x1="180.903158%" y1="-67.139828%" x2="-40.3415401%" y2="146.347035%"
                                                id="linearGradient-1-newsletter-icon">
                                    <stop stop-color="#0B71F0" offset="0%"></stop>
                                    <stop stop-color="#48F0FA" offset="100%"></stop>
                                </linearGradient>
                            </defs>
                            <g id="" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                                <g id="" transform="translate(-942.000000, -4023.000000)"
                                   fill="url(#linearGradient-1-newsletter-icon)">
                                    <g id="" transform="translate(0.000000, 3925.000000)">
                                        <g id="">
                                            <g id="">
                                                <path d="M981.283764,99.1361345 L969.540783,129.844346 C969.243769,130.61964 968.584223,131.218036 967.756514,131.460452 C966.926622,131.700684 966.024659,131.554361 965.325802,131.071713 L954.34938,123.438885 L979.567197,99.7039558 L951.047281,121.139209 L943.019162,115.559273 C942.285362,115.048234 941.903175,114.196502 942.021107,113.336034 C942.141223,112.475567 942.743987,111.748319 943.591351,111.438201 L980.135018,98.0572741 C980.460423,97.9371581 980.825139,98.0092277 981.071923,98.242908 C981.318707,98.4744043 981.401696,98.8260167 981.283764,99.1361345 L981.283764,99.1361345 Z M953.702938,132.707475 C953.163508,133.039432 952.484306,133.091847 951.896829,132.849431 C951.307169,132.604831 950.885671,132.098159 950.767739,131.491027 L949.210598,123.351528 L949.142896,122.995548 L958.632063,129.490549 C958.708501,129.595378 955.873762,131.370911 953.702938,132.707475 L953.702938,132.707475 Z"
                                                      id=""></path>
                                            </g>
                                        </g>
                                    </g>
                                </g>
                            </g>
                        </svg>
                    </button>
                    <div v-if="processing" class="p-ab" style="right: 26px; top: 14px">
                        <div class="dot-bricks"></div>
                    </div>
                </div>
            </form>
            <div class="d-f jc-c ai-c w-100" style="min-height: 38px">
                <p class="mb-0 ta-c c-green" v-if="done">Thank you for subscription</p>
                <p class="mb-0 ta-c c-red" v-if="error">Valid email is required</p>
            </div>
        </slot>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: "vue-newsletter",

        props: ['signUpUrl'],

        data() {
            return {
                done: false,
                processing: false,
                email: null,
                successMessage: null,
                error: null,
                list: null
            }
        },

        methods: {

            signup() {

                this.error = null;
                this.processing = true;

                axios.post(this.signUpUrl, {email: this.email, list: this.list})
                    .then(res => {
                        this.input = null;
                        this.processing = false;
                        this.done = true;
                        this.successMessage = "Thankyou, your email is subscribed."
                    })
                    .catch(err => {
                        this.error = !!err.response.data.errors.email[0];
                        this.processing = false;
                    });
            }
        }

    }
</script>
<template>
  <div class="pageContent">
    <loading :active.sync="isLoading" :is-full-page="fullPage">
    </loading>
    <form @submit="onSubmit" ref="form" method="POST">


      <div class="page">



        <div class="">

          <div v-show="activePage == 'intro'" class="intro">
            <div class="logo">
              <img src="/logo1.png" />
            </div>
            <div class="content">
              <div class="title" style="display: block;">

                <h1 class=""> Welcome,</h1>
                <p class="tagline text-black">We understand that Life is a circle and people need people to live and
                  survive. Hence, it’s only fair we do our best to acknowledge and appreciate those helping hands.
                  <br />
                  <br />
                  Create a design for the person that is your person.
                </p>
                <br />
              </div>

              <br />
              <div class="buttons my-5 w-1/3">
                <br />
                <button class="Vbutton mt-2 mobile-disabled" type="button" @click="activePage = 'userInfo'">Get
                  Started</button>
              </div>
            </div>
            <div class="img-art">
              <img src="/demoflyer.jpg" />
            </div>

            <div class="buttons">
              <button class="Vbutton mt-2 mobile-enabled" type="button" @click="activePage = 'userInfo'">Get
                Started</button>
            </div>
          </div>



          <div v-show="activePage == 'userInfo'" class="user-info">
            <div class="content">
              <div class="title">
                <h1>Let’s get to know you!</h1>
                <p class="tagline text-black"> Fill the details below to continue</p>

              </div>

              <div class="progress" style="display: block ;">
                <div class="progress-bar " :class="{ 'Half': !canContinue, 'Full': !canSubmit, }"></div>
              </div>

              <div class="form-group">
                <div class="form-input">
                  <label for="name">Enter your name </label>
                  <input id="name" name="name" v-model="form.name" placeholder="David Ayiwaju"
                    class="form-control w-full" />
                </div>

                <div class="form-input">
                  <label for="name">Enter your email </label>
                  <input id="email" type="email" name="email" v-model="email" placeholder="Jname@example.com"
                    class="form-control w-full" />
                  <p style="font-size:12px ;" class="text-red-400" v-show="userEmailError">{{ userEmailError }}</p>

                </div>

                <div class="form-input">
                  <label for="file">Choose a photo</label>
                  <label class="file-box w-full" for="file">
                    <p>{{ userFileName }}</p><span>Choose a photo</span>
                  </label>
                  <input id="file" accept="image/png, image/jpg, image/jpeg" hidden @change="onUserFileChange"
                    name="file" type="file" placeholder="John Doe" class="form-control w-full" />
                  <p style="font-size:12px ;" class="text-red-600" v-show="userFileError">{{ userFileError }}</p>
                  <p style="font-size:12px ;" class="text-black">{Accepted formats: png, jpg, jpeg. Max file size 5Mb}
                  </p>
                </div>

                <div class="buttons my-5 w-1/3">
                  <button type="button" class="Vbutton" :disabled="canContinue"
                    @click="activePage = 'partnerInfo'">Continue</button>
                </div>
              </div>

            </div>

            <div class="img-placeholder">
              <img v-if="form.userUrl" class="userImg" :src="form.userUrl" />
              <div v-if="!form.userUrl" class="placeholder-box">

              </div>
            </div>
          </div>

          <div v-show="activePage == 'partnerInfo'" class="user-info">
            <div class="content">
              <div class="title">
                <h1>Now Let's Talk about Your Everything</h1>
                <p class="tagline text-black"> Please Fill the details of your partner below to continue</p>

              </div>
              <br />
              <div class="progress">
                <div class="progress-bar" :class="{ 'Full': !canSubmit, 'Half': canSubmit }"></div>
              </div>

              <div class="form-group">
                <div class="form-input">
                  <label for="partnerName">Who is your Everything? </label>
                  <input id="partnerName" name="partnerName" v-model="form.partnerName" placeholder="John Doe" maxlength="15"
                    class="form-control w-full" />
                    <p style="font-size:11px ;" class="text-black">Max length of 15 characters (Including spaces)</p>

                </div>

                <div class="form-input">
                  <label for="relationship">What is your relationship with the person?</label>
                  <input id="relationship" name="relationship" v-model="form.relationship" placeholder="Wife"
                    class="form-control w-full" />
                </div>


                <div class="form-input">
                  <label for="partnerfile">Upload the image of Your Everything </label>
                  <label class="file-box w-full" for="partnerfile">
                    <p>{{ partnerFileName }}</p><span>Choose a photo</span>
                  </label>
                  <input id="partnerfile" accept='image/*' hidden @change="onFileChange" name="partnerfile" type="file"
                    class="form-control w-full" />
                  <p style="font-size:12px ;" class="text-red-600" v-show="partnerFileError">{{ partnerFileError }}</p>

                  <p style="font-size:12px ;" class="text-black">Accepted formats: png, jpg, jpeg. Max file size 5Mb</p>

                </div>

                <div class="buttons flex gap-3 my-5 ">
                  <button type="button" @click="activePage = 'userInfo'" class="Vbutton transparent">Back</button>
                  <button type="submit" :disabled="canSubmit" class="Vbutton">Generate Flyer</button>

                </div>
              </div>

            </div>

            <div class="img-placeholder">
              <img v-if="form.partnerUrl" class="userImg" :src="form.partnerUrl" />
              <div v-if="!form.partnerUrl" class="placeholder-box">

              </div>
            </div>
          </div>

        </div>

        <div class="social-share flex justify-center">
          <ul class="horizontal-list">
            <li><a target="_blank" href="https://twitter.com/share?ref_src=twsrc%5Etfw" class=""
                data-show-count="false">
                <font-awesome-icon icon="fa-brands fa-twitter" />
              </a></li>
            <li><a target="_blank" :href="'http://www.facebook.com/share.php?u=' + appUrl">
                <font-awesome-icon icon="fa-brands fa-facebook" />
              </a></li>
            <li><a :href="'whatsapp://send?text=\'Generate your Avon HMO \'My Everything\' Flyer for free! ' + appUrl">
                <font-awesome-icon icon="fa-brands fa-whatsapp" />
              </a></li>
            <li><a target="_blank" :href="'http://www.linkedin.com/shareArticle?mini=true&url=' + appUrl">
                <font-awesome-icon icon="fa-brands fa-linkedin" />
              </a>
            </li>
          </ul>
        </div>


      </div>

    </form>

    <div v-show="showResult" id="result" class="result flex justify-center w-full">

      <div class="r-poster">
        <div class="poster-right-overlay"></div>

        <div class="r-poster-content">
          <div class="poster-header">
          <span > #ForMyPerson</span>
        </div>
          <div class="user-section">
            <div class="flex" style="">
              <div class="userImg">
                <div class="sticker">
                  <img src="/poster-sticker2.png" />
                </div>
                <img :src="form.userUrl" />
              </div>
              <div class="userText pt-3">
                <h1>{{ form.partnerName }}, <br /> You are my person</h1>
              </div>
            </div>
          </div>

          <div class="partner-section">

            <div class="partner-img relative">
              <div class="sticker">
                <img src="/poster-sticker.png" />
              </div>

              <img :src="form.partnerUrl" />
            </div>


          </div>


        </div>
        <div class="poster-footer">

          <img src="/aha.jpg" />

        </div>
      </div>



      <div class="controls">

        <button id="download" @click="print()" class="Vbutton">Download</button>
        <button type="button" class="Vbutton" @click="canShowResult = false">Back</button>
      </div>

    </div>


    <div :class="{ 'hidden': !posterVisible }" class="poster relative" id="poster">


      <div class="poster-right-overlay"></div>

      <div class="poster-content">
        <div class="poster-header py-8">
          <span> #ForMyPerson</span>
        </div>
        <div class="user-section">
          <div class="flex" style="">
            <div class="userImg">
              <div class="sticker">
                <img src="/poster-sticker2.png" />
              </div>
              <img :src="form.userUrl" />
            </div>
            <div class="userText pt-3">
              <h1>{{ form.partnerName }}, <br /> You are my person</h1>
            </div>
          </div>
        </div>

        <div class="partner-section">

          <div class="partner-img relative">
            <div class="sticker">
              <img src="/poster-sticker.png" />
            </div>

            <img :src="form.partnerUrl" />
          </div>


        </div>


      </div>
      <div class="poster-footer">

        <img src="/aha.jpg" />

      </div>

    </div>



  </div>

</template>



<script>
import { onMounted } from 'vue'

import VsButton from '../components/Button.vue'
import axios from 'axios'

import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css"


export default {
  components: { VsButton, Loading },

  mounted() {
    let html2canvas = document.createElement('script')
    html2canvas.setAttribute('src', '/script/html2canvas.js')
    document.head.appendChild(html2canvas)
  },


  data() {
    return {
      isLoading: false,
      fullPage: true,
      email: '',
      form: {
        name: '',
        userUrl: '',
        partnerName: '',
        partnerUrl: '',
        relationship: ''
      },
      url: '',
      appUrl: import.meta.env.VITE_APP_URL,
      partnerUrl: '',
      canShowResult: false,
      posterVisible: false,
      activePage: "intro",
      partnerFileName: '',
      userFileName: '',
      userFileError: '',
      userEmailError: '',
      partnerFileError: '',
      formComplete: false,
    }
  },
  computed: {
    showResult() {
      if (this.canShowResult && this.form.partnerUrl && this.form.userUrl) {
        return true;
      }
      return false;
    },
    canContinue() {
      if (this.form.name == '' || this.form.userUrl == '' || !this.verifyEmail(this.email)) {
        return true;
      }
      return false;
    },

    canSubmit() {
      if (this.form.partnerName == '' || this.form.relationship == '' || this.form.partnerUrl == '') {
        return true;
      }
      return false;
    }
  },

  onMounted() {
    this.url = this.$route.params.path;
  },

  watch: {
    email(value) {
      this.verifyEmail(value)
    }
  },

  methods: {

   

    verifyEmail(value) {
      const pattern = /\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*/
      if (value == '') {
        this.userEmailError = "Enter an email address to Continue";
        return false;
      } else if (!pattern.test(value)) {
        this.userEmailError = "Enter a valid email address";
        return false;
      }
      this.userEmailError = "";
      return true;

    },
    onUserFileChange(e) {
      const file = e.target.files[0];

      if (!this.isImage(file.name)) {
        this.userFileError = "Unsupported File Format";
        return;
      }
      this.userFileName = file.name;
      this.form.userUrl = URL.createObjectURL(file);

    },

    onFileChange(e) {
      const file = e.target.files[0];
      if (!this.isImage(file.name)) {
        this.partnerFileError = "Unsupported File Format";
        return;
      }
      this.partnerFileName = file.name;
      this.form.partnerUrl = URL.createObjectURL(file);

    },

    async onSubmit(e) {
      e.preventDefault();
      this.isLoading = true;
      const url = import.meta.env.VITE_SPREEDSHEET_URL + '?name=' + this.form.name + '&partnerName=' + this.form.partnerName + '&relationship=' + this.form.relationship + '&email=' + this.email;
      return axios({
        method: 'GET',
        url: url,
      }).then(async () => {
        this.isLoading = false;
        this.canShowResult = true;

      }).catch((err) => {
        console.log(err);
        this.isLoading = false;
        this.canShowResult = true;
      })
    },

    getExtension(filename) {
      var parts = filename.split('.');
      return parts[parts.length - 1];
    },

    isImage(filename) {
      var ext = this.getExtension(filename);
      switch (ext.toLowerCase()) {
        case 'jpg':
        case 'jpeg':
        case 'png':
          //etc
          return true;
      }
      return false;
    },



    toSlug(str) {

      //replace all special characters | symbols with a space
      str = str.replace(/[`~!@#$%^&*()_\-+=\[\]{};:'"\\|\/,.<>?\s]/g, ' ').toLowerCase();

      // trim spaces at start and end of string
      str = str.replace(/^\s+|\s+$/gm, '');

      // replace space with dash/hyphen
      str = str.replace(/\s+/g, '-');

      return str;
    },





    async print() {
      await (this.posterVisible = true);
      this.isLoading = true;
      const that = this;
      var node = document.getElementById("poster");
      window.html2canvas(node, { useCORS: true, proxy: true })
        .then((canvas) => {
          (that.posterVisible = false);
          var link = document.createElement('a');
          link.download = 'my-everything-image';
          link.href = canvas.toDataURL('image');
          link.click();
          that.isLoading = false;

        });
    },


  }

}
</script>


<style>
.pageContent {
  width: 100%;
  height: 100vh;
  padding: 5em 7em;
}


.result {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  overflow-y: auto;
  overflow-x: auto;
  background: black;
  height: 100%;
  z-index: 200;
  display: flex;
}

.tagline {
  position: absolute;
}


.tagline h1 {
  font-size: 25px;
  margin-top: 200px;
  padding: 10px 20px;
  color: black;
  background-color: white;
}

.img-section {
  height: 840.2px;
  display: flex;
  position: relative;
  justify-content: flex-end;
  align-items: center;
}

.img-section .filter-div {
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.325);
  position: absolute;
}


.img-section img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
}

.text-section {
  background-color: white;
  height: 352.8px;
  display: flex;
}


@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>

<style>
.poster {
  width: 1000px;
  margin-top: 500px;
  height: 1020px;
  overflow: hidden;
  background: url('/bg.jpg');

}

.poster .user-section {
  display: flex;
  margin-top: 30px;
}


.poster .poster-header{
  padding: 20px 30px;
  font-size: 22px;
  color: grey;
}




.poster .user-section {
  display: flex;
  padding: 1em 3em;
  position: relative;
  z-index: 100;
  gap: 2em;
}



.poster .user-section .userImg {
  width: 386px;
  height: 314px;
  position: relative;
  padding: 15px;
  background-color: white;
  box-shadow: rgb(147, 147, 147) 0px 10px 20px -10px
}

.user-section .userImg img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
}

.poster .userText h1 {
  font-size: 47px;
  font-weight: 800;
  color: rgb(117, 80, 161);
  margin-left: 20px;
  line-height: 50px;
  font-family: avonFntThin, sans-serif !important;
}

.poster .partner-section {
  height: 450px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

.poster .partner-img .sticker {
  width: 143px;
  position: absolute;
  z-index: 222;
  left: 38%;
  top: -17%;
  transform: rotate(-11deg);
}

.poster .userImg .sticker {
  width: 72px;
  position: absolute;
  z-index: 222;
  left: 40%;
  top: -22%;
}

.poster .partner-section .partner-img {
  width: 560px;
  height: 520px;
  background: white;
  padding: 20px;
  position: absolute;
  top: -31%;
  right: 9%;
  transform: rotate(13.5deg);
  z-index: 111;
  box-shadow: rgb(70, 69, 69) -5px -5px 40px -10px
}

.poster .partner-section .partner-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;

}

.poster .partner-section {
  position: relative;
}

.userText h1 {
  font-size: 69px;
  font-weight: 800;
  color: black;
  margin-left: 50px;
  line-height: 60px;
  font-family: 'League Spartan', sans-serif !important;
}

.poster .poster-footer {
  position: absolute;
  width: 100%;
  bottom: 0;
}

.poster .poster-footer img {
  width: 100%;
}

.footer-social {
  width: 50%;
}

.poster-right-overlay {
  position: absolute;
  width: 100%;
  height: 100% !important;
  right: 0;
  opacity: .3;
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.1));
}
</style>
<template>

<div v-if="!isMobile() && closed_captions.length > 3">
 <div>
  <p style=" padding-top: 40px; padding-bottom: 80px; font-size: 32px; font-family: Verdana;"><center>English Speaking Practice</center></p>

<div class="columns is-desktop">
  <div class="column"><div class='column is-12'>
            <div class="card-content">
          <div class="content"  v-if="closed_captions.length > 3" >
            <iframe width="800" height="450" :src="`https://www.youtube-nocookie.com/embed/${this.$route.params.id}?${start_end}`" title="YouTube video player" frameborder="0" :allow="autoPlay" allowfullscreen></iframe>

            <!--<video-player :src="`https://www.youtube.com/watch?v=${this.$route.params.id}?start=${selected.$.start}&end=${selected.$.start+selected.$.dur}`"/> ;end=${selected.$.start+selected.$.dur} -->




<div style="padding-bottom: 40px; padding-top: 40px; display: inline-block" class="control">

<b-loading :is-full-page="isFullPage" v-model="isLoading" :can-cancel="true"></b-loading>

<div class="columns is-mobile">
  <div class="column is-4">
     <b-button type="is-success"
        @click="startRecognition"
        icon-left="microphone">Record</b-button>
  </div>
  <div style="margin-left: 20px;" class="column is-8"><center>{{ runtimeTranscription }}</center></div>
</div>
<br><br>
  <!--<textarea  v-model="transcription" ref="inputTextArea" class="textarea" placeholder="" disabled></textarea>-->
</div>
<!--
  <audio-recorder
    style="margin-top: 30px"
    upload-url="YOUR_API_URL"
    :attempts="1"
    :time="2"
    :headers="headers"
    :before-recording="callback"
    :pause-recording="callback"
    :after-recording="callback"
    :select-record="callback"
    :before-upload="callback"
    :successful-upload="callback"
    :failed-upload="callback"/>
-->
          </div>
        </div>
      </div></div>

  <div class="column"><div   v-if="closed_captions.length > 3" class='column is-12'>
            <div class="card-content">
          <div class="content">

                <b-table
@click="selectedTable($event)"
                height="590px"
                sticky-header
                 class="table"

        :bordered="true"
        :data="closed_captions"
        :columns="columns"
        :selected.sync="selected"

        focusable>
                </b-table>


          </div>
        </div>
      </div></div>


</div>

</div>
</div>
<div v-else>
<div class="columns is-mobile">
  <div class='column is-12'>
        <div class="card-content">
          <div class="content"  v-if="closed_captions.length > 3" >

            <iframe width="100%" height="300"
              :src="`https://www.youtube-nocookie.com/embed/${this.$route.params.id}?${start_end}`"
               title="YouTube video player" frameborder="0"
               allow="autoplay;" allowfullscreen>
            </iframe>
          </div>
        </div>
<div v-if="closed_captions.length > 3" class='column is-12'>
            <div class="card-content">
          <div class="content">



<div style="padding-bottom: 40px; padding-top: 0px; display: inline-block" class="control">

<b-loading :is-full-page="isFullPage" v-model="isLoading" :can-cancel="true"></b-loading>

<div class="columns is-mobile">
     <div v-if="!isShowFireworks" style=" display: flex;
  justify-content: space-between;
  " class="column is-12">
     <b-button type="is-danger"
     style="margin-right: 10px"
        @click="startRecognition"
        icon-left="microphone">Record</b-button>

             <b-button type="is-info"
               style="margin-right: 10px"
        @click="play()"
        icon-left="play">Play</b-button>

        <b-button type="is-danger"
          style="margin-right: 10px"
        @click="clearRecognition()"
        icon-left="delete">Clean</b-button>
     </div>
  </div>

<div style="margin-left: 0px;" class="column is-12">
  <center>
       <VueFireworks :show.sync="isShowFireworks" />

      {{this.selected._}}
      <br v-if="rate">
       <br v-if="rate">
    <div v-if="rate">

    <b-rate
            v-model="rate"
            :icon-pack="packs"
            :icon="icons"
            :max="maxs"
            :size="sizes"
            :locale="locale"
            :show-score="score"
            :custom-text="custom"
            :show-text="text"
            :texts="texts"
            :rtl="isRtl"
            :spaced="isSpaced"
            :disabled="isDisabled">
        </b-rate>
    </div>
    <br v-if="rate">
    <hr>
    <br v-if="rate">
    <p v-if="transcription">{{transcription[0]}}</p>
    {{ runtimeTranscription }}
  </center>
</div>
</div>

</div>
                <b-table
@click="selectedTable($event)"
           :paginated="true"
            :per-page="perPage"
            :current-page.sync="currentPage"
            :pagination-simple="true"
          :pagination-position="paginationPosition"
                 class="table"
        :bordered="true"
        :data="closed_captions"
        :columns="columns"
        :selected.sync="selected"
        focusable>
                </b-table>



        </div>
      </div></div>
      </div>
    </div>


</template>

<script>
import axios from 'axios';
import VueFireworks from '@meijie/vue-fireworks'
export default {
  components: {
    VueFireworks,
  },
  computed: {
    start_end: {
      //console.log(JSON.parse(localStorage.getItem("SPEECH")))
      //console.log(JSON.parse(localStorage.getItem(this.$route.params.id)))
		get: function() {

      if(JSON.parse(localStorage.getItem(this.$route.params.id))){
        this.sum = (parseInt(JSON.parse(localStorage.getItem(this.$route.params.id)).$.start)+parseInt(JSON.parse(localStorage.getItem(this.$route.params.id)).$.dur))+1;

        if(this.selected.$.start !== 0){
           return "&rel=0&controls="+this.control+"&autoplay=1&start="+parseInt(this.selected.$.start)+"&end="+this.sum;
        }

         // return "controls=0&autoplay=1&start=2&end=3";
      }else{
        this.selected = {"$": { "start": 0, "dur": 0} };
      }
		},
		set: function(newValue) {
      return "&rel="+newValue+"&controls="+this.control+"&autoplay=1&start="+parseInt(this.selected.$.start)+"&end="+this.sum;
		}

    }
  },
   props: {
    lang: {
      type: String,
      default: "en-US",
    },
  },
  data(){
    return {
       rate: '',
                maxs: 5,
                isShowFireworks: false,
                sizes: 'is-large',
                packs: 'mdi',
                icons: 'star',
                score: false,
                custom: '',
                text: false,
                texts: ['Very bad', 'Bad', 'Good', 'Very good', 'Awesome'],
                isRtl:false,
                isSpaced: false,
                isDisabled: true,
                locale: undefined // Browser locale


                ,
          runtimeTranscription: "",
    transcription: [],
    recognition: null,

   isLoading: false,
                isFullPage: true,
      headers: {},
      closed_captions: [],
      control: 0,
      //play: 0,
         currentPage: 1,
                perPage: 10,
                paginationPosition: 'both',
      selected: {"$": { "start": 0, "dur": 1} },
      autoPlay: '',
      sum: 0,
      columns: [
                {
                    field: '_',
                    label: 'Caption',
                }],

    }
  },
  methods: {
    selectedTable(a){
      this.control = !this.control;
      if(process.browser){
            localStorage.setItem(this.$route.params.id, JSON.stringify(a));
        }

    },
    play(){
      //https://serversideup.net/packaging-a-nuxtjs-app-for-ios-and-android-with-capacitorjs/
        this.$toast.error('Please Click on the video player')
    },
clearRecognition(){
  if(this.transcription.length > 0){
     this.transcription = '';
     this.$toast.success('Successfully deleted')
  }else{
    this.$toast.error('There is nothing to delete')
  }
},
      startRecognition() {
      //this.$toast.success("Starting")
      //console.log("Starting");
      this.isLoading = true
      this.checkApi();
      this.recognition.start();
    },
    stopRecognition() {
      this.$toast.warning("Stopping")
     // console.log("Stopping");
      this.isLoading = false
      this.recognition.stop();
      this.recognition = null;
    },
    checkApi() {
      window.SpeechRecognition =
        window.SpeechRecognition || window.webkitSpeechRecognition || android.speech.SpeechRecognizer;
      if (!SpeechRecognition && "development" !== "production") {
        throw new Error(
          "Speech Recognition does not exist on this browser. Use Chrome or Firefox"
        );
      }
      if (!SpeechRecognition) {
        this.$toast.warning("No Speech Recognition")
        console.log("No Speech Recognition");
        return;
      }
      this.$toast.success("Recording...")
      this.transcription = [];

      this.recognition = new SpeechRecognition();

      this.recognition.lang = this.lang;
      this.recognition.interimResults = true;
        this.$toast.success("before result")
      this.recognition.addEventListener("result", (event) => {
         this.$toast.success("After result")

        //console.log("result");
        const text = Array.from(event.results)
          .map((result) => result[0])
          .map((result) => result.transcript)
          .join("");
        this.runtimeTranscription = text;
      });
      this.recognition.addEventListener("end", () => {
        //console.log("End");
        //this.$toast.error("Time is up")
        this.isLoading = false
        if (this.runtimeTranscription !== '') {

          this.transcription.push(this.runtimeTranscription);

          var arr1 = JSON.parse(localStorage.getItem(this.$route.params.id))._.split(" ").map(v => v.toLowerCase());
          arr1 = arr1.map(v => v.replace(".", ""))
          arr1 = arr1.map(v => v.trim())
          arr1 = arr1.map(v => v.replace(",", ""))

          var arr2 = this.transcription[0].split(" ").map(v => v.toLowerCase());
          arr2 = arr2.map(v => v.trim())
          arr2 = arr2.map(v => v.replace(",", ""))
          arr2 = arr2.map(v => v.replace(".", ""))

        const intersection = arr1.filter(element => arr2.includes(element));
          console.log(arr1);
          console.log(arr2);
          console.log(intersection);

          this.rate = ((intersection.length * 100) / arr1.length) / 20;

          if(this.rate == 0){
            this.rate = 0.000001;
          }

          if(this.rate === 5){
            this.isShowFireworks = true
          }
          console.log(this.rate)
          this.$emit("onTranscriptionEnd", {
            transcription: this.transcription,
            lastSentence: this.runtimeTranscription,
          });
        } else {
          this.$toast.error("An error has occurred")
          this.isLoading = false
        }
        this.runtimeTranscription = "";
      });
      this.recognition.onresult = function (event) {
        var color = event.results[0][0].transcript;
        //console.log(color);
      };
    },

    callback(data){
     //console.log(data.url)
    },
 isMobile() {
    if (process.browser) {
   if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
     return true
   }}
 }
 },

  async created() {
    if(process.browser && !localStorage.getItem(this.$route.params.id)){
        localStorage.setItem(this.$route.params.id, JSON.stringify(this.selected));
         this.selected = JSON.parse(localStorage.getItem(this.$route.params.id));
        this.autoPlay = 'autoplay'
    }else{
     this.selected =  {"$": { "start": 0, "dur": 1} }
    }

  const headers = { "Content-Type": "application/json" };
  await axios.post("https://tortoiseshell-petal-larkspur.glitch.me/api/v1/speeches/"+this.$route.params.id, { headers })
    .then(response => this.closed_captions = response.data);

  },
}
</script>

<style scoped>
.table:hover {
  cursor: pointer;
}
</style>

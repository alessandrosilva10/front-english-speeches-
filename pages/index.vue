<template>
  <section class="section">
<div class='columns is-multiline'>
  <div v-for="(speeches, id) in data" :key="id" class='column is-4'>
    <div class="card">
        <div class="card-content">
          <div class="content">
             <img @click="openNewTab(data[id].id.videoId)" :src="`https://img.youtube.com/vi/${data[id].id.videoId}/hqdefault.jpg`" />
          </div>
        </div>
  </div>
</div>
</div>

  </section>
</template>

<script>
import Card from '~/components/Card'
import axios from 'axios';

export default {
  name: 'HomePage',

  components: {
    Card
  },
  methods: {
  	openNewTab: function(videoId) {
      this.$router.push('/practice/'+videoId)
    }
  },
  data(){
    return {
      data: [

      ]
    }
  },
  async created() {
  const headers = { "Content-Type": "application/json" };
  await axios.get("https://tortoiseshell-petal-larkspur.glitch.me/api/v1/speeches", { headers })
    .then(response => this.data = response.data.data[0].items);

},
}
</script>

<style scoped>
img:hover {
  transition: transform .2s; /* Animation */
  transform: scale(1.2); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
}
</style>


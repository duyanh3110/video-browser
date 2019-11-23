<template>
  <div class="container">
    <!-- @ stand for v-on: -->
    <SearchBar @termChange="onTermChange" />
    <div class="row">
      <!-- v-bind: van be : -->
      <VideoList @videoSelect="onVideoSelect" :videos="videos" />
      <VideoDetail :video="selectedVideo" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar";
import VideoList from "./components/VideoList";
import VideoDetail from "./components/VideoDetail";

const API_KEY = "AIzaSyDJuFtCpjVmQ9tE0JPS-suc-b65HplhGzQ";

export default {
  name: "App",
  components: {
    SearchBar,
    VideoList,
    VideoDetail
  },
  data() {
    return { videos: [], selectedVideo: null };
  },
  methods: {
    onTermChange(searchTerm) {
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            key: API_KEY,
            type: "video",
            part: "snippet",
            q: searchTerm
          }
        })
        .then(response => {
          this.videos = response.data.items;
        })
        .catch(error => console.log(error.message));
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    }
  }
};
</script>

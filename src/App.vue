<template>
  <div id="app">
    <section class="header">
      <h1 class="text-center mt-2"><i class="fa fa-spotify"></i> Spotyler</h1>
    </section>
    <section class="main">
      <div class="playlists">
        <!-- Button trigger modal -->
        <button
          type="button"
          class="btn button-modal"
          data-bs-toggle="modal"
          data-bs-target="#exampleModal"
        >
          <i class="fa fa-list"></i>
        </button>

        <!-- Modal -->
        <div
          class="modal fade"
          id="exampleModal"
          tabindex="-1"
          aria-labelledby="exampleModalLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Your Playlist</h5>
                <button
                  type="button"
                  class="btn btn-primary"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                >
                  <i class="fa fa-close"></i>
                </button>
              </div>
              <div class="modal-body">
                <button
                  class="playlist py-2"
                  v-for="song in songs"
                  :key="song.src"
                  @click="play(song)"
                  :class="song.src == current.src ? 'song playing' : 'song'"
                >
                  {{ song.artist }} - {{ song.title }}
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="image-album">
        <img :src="current.imageAlbum" :class="this.isPlaying ? 'rotate' : ''" />
      </div>
      <div class="content-song mt-4">
        <h5 class="song artist">{{ current.artist }}</h5>
        <h5 class="song title">{{ current.title }}</h5>
      </div>
      <div class="controls text-center">
        <button class="btn btn-dark prev" @click="prev">
          <i class="fa fa-backward"></i>
        </button>
        <button class="btn btn-primary btn-lg play" v-if="!isPlaying" @click="play">
          <i class="fa fa-play"></i>
        </button>
        <button class="btn btn-primary btn-lg play" @click="pause" v-else>
          <i class="fa fa-stop"></i>
        </button>
        <button class="btn btn-dark next" @click="next">
          <i class="fa fa-forward"></i>
        </button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Edge of Desires",
          artist: "John Mayer",
          imageAlbum:
            "https://bostonglobe-prod.cdn.arcpublishing.com/resizer/155g-MeUpCeeKnoI865-FyjSpjs=/1440x0/arc-anglerfish-arc2-prod-bostonglobe.s3.amazonaws.com/public/N4CFOCQGT4I6HCFRX6RULTPAKU.jpg",
          src: require("./assets/Music/Edge of Desire.mp3"),
        },
        {
          title: "Bertaut",
          artist: "Nadin Amizah",
          imageAlbum:
            "https://images.genius.com/0df14fac91dc52c3974d1f8ae0a94b0f.640x640x1.jpg",
          src: require("./assets/Music/Nadin Amizah - Bertaut (Official Music Video).mp3"),
        },
        {
          title: "Back to December",
          artist: "Taylor Swift",
          imageAlbum:
            "https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/e902b262-aeb7-479a-9ef7-6f65bb802e88/d5eadoe-95ed7dbc-dc09-40c7-9547-27efba8772ad.png?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOiIsImlzcyI6InVybjphcHA6Iiwib2JqIjpbW3sicGF0aCI6IlwvZlwvZTkwMmIyNjItYWViNy00NzlhLTllZjctNmY2NWJiODAyZTg4XC9kNWVhZG9lLTk1ZWQ3ZGJjLWRjMDktNDBjNy05NTQ3LTI3ZWZiYTg3NzJhZC5wbmcifV1dLCJhdWQiOlsidXJuOnNlcnZpY2U6ZmlsZS5kb3dubG9hZCJdfQ.kfMhTglDHN4qr1Iyy9jSpMsClaVfln_yUkGop1_1uJE",
          src: require("./assets/Music/Taylor Swift - Back To December.mp3"),
        },
        {
          title: "Aib Show Medley",
          artist: "Aib",
          imageAlbum: "https://cdn.booooooom.com/wp-content/uploads/2017/11/Lee1.jpg",
          src: require("./assets/Music/Aib Show Medley.mp3"),
        },
        {
          title: "Reinkarnasi",
          artist: "Adeavery Feat Ray Prasetya",
          imageAlbum: "https://cdn.booooooom.com/wp-content/uploads/2017/11/Lee2.jpg",
          src: require("./assets/Music/Adeavery Reinkarnasi feat Ray Prasetya.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.next();
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  font-family: monospace;
  min-height: 100vh;
  background-color: rgb(34, 34, 34);
}
.header {
  background-color: black;
  color: rgb(128, 253, 128);
  padding: 10px;
}
.image-album {
  text-align: center;
  margin-top: 3em;
}
img {
  border-radius: 50%;
  object-fit: none; /* Do not scale the image */
  object-position: center; /* Center the image within the element */
  height: 30vh;
  width: 30vh;
}
.button-modal {
  display: block;
  color: rgb(173, 173, 173);
  margin: 18px 18px 10px auto;
}
.modal-content {
  background-color: black;
  color: white;
}
.playlist {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
  color: white;
  display: block;
  width: 100%;
  font-size: 16px;
}
.playing {
  background-image: linear-gradient(to right, #12175e, #62757a);
}
.song {
  color: white;
  padding-left: 30px;
  padding-right: 30px;
  text-align: center;
}
.artist {
  font-weight: bold;
}
.controls {
  padding-left: 30px;
  margin-top: 7em;
  padding-right: 30px;
}
.prev {
  margin-right: 3em;
}
.next {
  margin-left: 3em;
}
.rotate {
  animation: rotation 6s infinite linear;
  -moz-box-shadow: 2px 3px 10px rgb(0, 0, 95);
  -webkit-box-shadow: 1px 1px 10px rgb(0, 0, 41);
  box-shadow: 4px 4px 10px rgb(94, 255, 0);
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
</style>

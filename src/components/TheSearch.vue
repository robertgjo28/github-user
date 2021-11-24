<template>
  <div class="justify-center" :class="{ bgDarkish: moon, bgLightish: sun }">
    <div class="container optimize-width">
      <div class="row" :class="{ textDark: sun }">
        <div class="col-6 header">devfinder</div>
        <div class="col-6 text-end switches">
          <p class="text-uppercase">
            <span @click="changeBackground">
              {{ moon ? "Light" : "Dark" }}
              <i class="far fa-sun ms-1" v-if="moon"></i>
              <i class="far fa-moon ms-1" v-if="sun"></i>
            </span>
          </p>
        </div>
      </div>
      <div class="row mt-2">
        <div class="input-group mb-3 bg-behind" :class="{ boxShadow: sun }">
          <span class="input-group-text"
            ><i class="fas fa-search ms-2"></i
          ></span>
          <input
            type="text"
            class="form-control input-changes ms-3"
            placeholder="Search GitHub user..."
            v-model="user"
            :class="{ textDark: sun, textLight: moon }"
          />
          <span class="input-group-text"
            ><button
              type="submit"
              @click.prevent="findUser"
              class="btn py-2 px-4"
            >
              Search
            </button></span
          >
        </div>
      </div>
      <div class="row mt-4">
        <div class="results py-4 px-4" :class="{ boxShadow: sun }">
          <div class="row">
            <div class="col-3 text-center">
              <img :src="userData.avatar_url" alt="" class="img-changes" />
            </div>
            <div class="col-5">
              <p class="lead git-heading">{{ userData.name }}</p>
              <a :href="userData.html_url" class="link-git" target="_blank"
                >@{{ userData.login }}</a
              >
            </div>
            <div class="col-4 text-end">
              <p class="lead git-right font-stats">
                Created at: {{ getLocalDate() }}
              </p>
            </div>
          </div>
          <div class="row">
            <div class="col-12 mt-3 text-justify">
              <p class="subheading-git px-2">
                {{ userData.bio }}
              </p>
            </div>
            <div class="col-12 stats-git py-3 mt-3" :class="{ statsDark: sun }">
              <div class="row text-center">
                <div class="col-4 font-stats">Repos</div>
                <div class="col-4 font-stats">Following</div>
                <div class="col-4 font-stats">Followers</div>
              </div>
              <div class="row text-center">
                <div class="col-4">
                  <a :href="getRepos()" target="_blank" class="no-underlines">{{
                    userData.public_repos
                  }}</a>
                  {{}}
                </div>
                <div class="col-4">
                  <a
                    :href="getFollowing()"
                    target="_blank"
                    class="no-underlines"
                    >{{ userData.following }}</a
                  >
                </div>
                <div class="col-4">
                  <a
                    :href="getFollowers()"
                    target="_blank"
                    class="no-underlines"
                    >{{ userData.followers }}</a
                  >
                </div>
              </div>
            </div>
          </div>
          <div class="row mt-3 mx-5 pe-5">
            <div class="col-12 mb-4 mt-3">
              <div class="row">
                <div class="col-7">
                  <div class="row">
                    <div class="col-2">
                      <i class="fas fa-map-marker-alt fa-font-size"></i>
                    </div>
                    <div class="col-10">
                      <p>
                        {{
                          userData.location
                            ? userData.location
                            : "Not available"
                        }}
                      </p>
                    </div>
                  </div>
                </div>
                <div class="col-5">
                  <div class="row">
                    <div class="col-2">
                      <i class="fas fa-link fa-font-size"></i>
                    </div>
                    <div class="col-10">
                      <a
                        :href="getWebsite()"
                        target="_blank"
                        class="ms-3 info-href"
                        :class="{ infoIf: userData.blog, textGray: sun }"
                      >
                        {{ userData.blog ? userData.blog : "Not available" }}
                      </a>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="col-7">
                  <div class="row">
                    <div class="col-2">
                      <i class="fab fa-twitter fa-font-size"></i>
                    </div>
                    <div class="col-10">
                      <a
                        :href="getTwitter()"
                        target="_blank"
                        class="info-href"
                        :class="{
                          infoIf: userData.twitter_username,
                          textGray: sun,
                        }"
                        >{{
                          userData.twitter_username
                            ? "@" + userData.twitter_username
                            : "Not available"
                        }}</a
                      >
                    </div>
                  </div>
                </div>
                <div class="col-5">
                  <div class="row">
                    <div class="col-2">
                      <i class="fas fa-building fa-font-size"></i>
                    </div>
                    <div class="col-10">
                      <p class="ms-3">
                        {{
                          userData.company ? userData.company : "Not available"
                        }}
                      </p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TheSearch",

  data() {
    return {
      user: "",
      userData: {},
      dateOptions: { year: "numeric", month: "short", day: "numeric" },
      moon: true,
      sun: false,
    };
  },

  created() {
    axios
      .get(`https://api.github.com/users/robertgjo28`)
      .then((response) => {
        this.userData = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },

  methods: {
    findUser() {
      this.userData = {};
      this.loading = true;
      axios
        .get(`https://api.github.com/users/${this.user}`)
        .then((response) => {
          this.userData = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
      this.loading = false;
    },
    getRepos() {
      if (this.user != "") {
        return `https://github.com/${this.user}?tab=repositories`;
      } else {
        return `https://github.com/robertgjo28?tab=repositories`;
      }
    },
    getFollowers() {
      if (this.user != "") {
        return `https://github.com/${this.user}?tab=followers`;
      } else {
        return `https://github.com/robertgjo28?tab=followers`;
      }
    },
    getFollowing() {
      if (this.user != "") {
        return `https://github.com/${this.user}?tab=following`;
      } else {
        return `https://github.com/robertgjo28?tab=following`;
      }
    },
    getWebsite() {
      if (this.user != "") {
        if (
          this.userData.blog.includes("https") ||
          this.userData.blog.includes("http")
        ) {
          return `${this.userData.blog}`;
        } else {
          return `https://${this.userData.blog}`;
        }
      } else {
        return `https://robertgj.dev`;
      }
    },
    getTwitter() {
      if (this.userData.twitter_username) {
        return `https://twitter.com/${this.userData.twitter_username}`;
      }
    },
    getLocalDate() {
      return new Date(this.userData.created_at).toLocaleDateString(
        "en-us",
        this.dateOptions
      );
    },
    changeBackground() {
      this.moon = !this.moon;
      this.sun = !this.sun;
    },
  },
};
</script>

<style scoped>
.bgLightish {
  background-color: #f6f8ff;
}

.bgDarkish {
  background-color: #141d2f;
}

.textDark {
  color: #000 !important;
}

.textLight {
  color: #fff !important;
}

.no-underlines {
  text-decoration: none;
}

.boxShadow {
  background-color: #fff !important;
  color: #4b6a9b;
  box-shadow: 0px 16px 30px -10px rgb(70 96 187 / 20%);
}

.statsDark {
  background-color: #f6f8ff !important;
}

.justify-center {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100vh;
  color: #fff;
  font-family: "Space Mono", monospace;
}

.header {
  font-size: 30px;
}

.switches {
  font-size: 16.5px;
  letter-spacing: 0.1rem;
}

.fa-search {
  color: #0079ff;
  font-size: 1.6rem;
}

.input-changes {
  background-color: transparent;
  border: none;
  color: #fff;
}

.input-changes:focus {
  background-color: transparent;
  box-shadow: none;
  border: none;
}

.input-group-text {
  background-color: transparent;
  border: none;
  color: #fff;
}

.input-group-text button {
  color: #fff;
  background-color: #0079ff;
  border: 1px solid #0079ff;
}

.bg-behind {
  background-color: #1e2a47;
  margin: 0;
  padding: 0;
  height: 70px;
  border-radius: 15px;
}

.results {
  background-color: #1e2a47;
  border-radius: 15px;
}

.img-changes {
  border-radius: 50%;
  width: 125px;
  height: 125px;
}

.git-heading {
  font-size: 1.1rem;
}

.link-git {
  text-decoration: none;
}

.git-right {
  font-size: 1rem;
}

.subheading-git {
  letter-spacing: 0.01rem;
  font-size: 1rem;
}

.stats-git {
  background-color: #141d2f;
  border-radius: 15px;
}

.font-stats {
  font-size: 0.8rem;
  margin-bottom: 0.5rem;
}

.fa-font-size {
  font-size: 1.6rem;
  color: #0079ff;
}

.info-href {
  color: #fff;
  text-decoration: none;
}

.infoIf {
  color: #0079ff;
}

.textGray {
  color: #4b6a9b !important;
}

.optimize-width {
  width: 40%;
}
</style>

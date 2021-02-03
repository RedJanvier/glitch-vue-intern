<template>
  <div id="app">
    <div class="content">
      <h1>{{ name }}</h1>
      <img
        alt="main"
        src="https://cdn.glitch.com/511b35f3-cc62-456b-9917-a7f7a573af3c%2Fmain.png?v=1610119233266"
      />
      <ul class="navigation">
        <li>Home</li>
        <li>About</li>
        <li>
          <img
            src="https://cdn.glitch.com/511b35f3-cc62-456b-9917-a7f7a573af3c%2Fmain.png?v=1610119233266"
          />
        </li>
        <li>Gallery</li>
        <li>Contact</li>
      </ul>
      <ul class="links">
        <li v-for="(link, propName) in social_media">
          <a :href="link"
            ><i
              :class="[
                'fa',
                propName == 'instagram'
                  ? 'fa-instagram'
                  : propName == 'email'
                  ? 'fa-envelope-square'
                  : 'fa-' + propName + '-square',
              ]"
            ></i
          ></a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  created() {
    fetch("https://hirng-x2021.glitch.me/api")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        const { name, social_media } = data;
        this.name = name;
        const social_medias = {};
        for (const media in social_media) {
          social_medias[media] = this.generateLink(media, social_media[media]);
        }
        console.log("These are medias", social_medias);
        return (this.social_media = social_medias);
      })
      .catch(console.log);
  },
  data() {
    return {
      name: "",
      social_media: {},
    };
  },
  methods: {
    generateLink(type, username) {
      switch (type) {
        case "instagram":
          return "https://www." + type + ".com/" + username;
        case "twitter":
          return "https://" + type + ".com/" + username;
        case "snapchat":
          return "https://" + type + ".com/add/" + username;
        default:
          return username;
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  letter-spacing: 4px;
}
a {
  color: white;
  text-decoration: none;
}
#app {
  position: relative;
  font-family: "Montserrat", sans-serif;
  text-transform: uppercase;
  font-weight: 100;
  text-align: center;
  color: #ffffff;
  background: #be4e73;
}
#app::before {
  content: "";
  width: 50vw;
  height: 100%;
  left: 0;
  top: 0;
  position: absolute;
  background: #b92355;
}
#app .content {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-content: space-around;
  justify-content: center;
  height: 100vh;
}
#app .content h1 {
  padding: 1rem 0;
}
#app .content > img {
  margin: auto;
  max-width: 35rem;
  width: 100%;
  height: auto;
  box-shadow: rgba(0, 0, 0, 0.25) -5px 10px 45px,
    rgba(0, 0, 0, 0.2) -1px 5px 30px;
}
ul {
  list-style: none;
}
.navigation {
  display: grid;
  align-items: center;
  grid-template-columns: repeat(auto-fit, minmax(30px, 1fr));
  padding: 1rem 0;
  width: 100%;
  margin: auto;
  animation: navbarIn 1s ease-in-out forwards;
}
.navigation img {
  width: 5rem;
  height: 5rem;
  border-radius: 50%;
}
.navigation li {
  padding: 0 1rem;
}
.links {
  top: 50%;
  display: flex;
  position: fixed;
  flex-direction: column;
  gap: 1rem;
  opacity: 0;
  right: 1rem;
  font-size: 2rem;
  transform: translateY(-50%);
  animation: linksIn 1s ease-in-out 0.5s forwards;
}
@keyframes navbarIn {
  0% {
    width: 30%;
  }
  100% {
    width: 50%;
  }
}
@keyframes linksIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@media only screen and (max-width: 1178px) {
  .navigation {
    width: 100% !important;
    margin: 0;
    grid-template-columns: repeat(auto-fit, minmax(5%, 1fr));
  }
}
@media only screen and (max-width: 480px) {
  .navigation {
    width: 100% !important;
    grid-gap: 0.4rem;
    margin: 0;
    grid-template-columns: 1fr;
  }
  .navigation img {
    width: 20%;
    height: 20%;
  }
}
</style>

<template>
  <footer >
 <div class="footer">
      <div>
      <b> &copy;</b> <span>{{ copyRight }} All rights reserved</span>
    </div>

    <div>
      <a
        href="https://github.com/thykingdoncome/track-covid19"
        target="_blank"
        rel="noopener noreferrer"
      >
        <i class="fab fa-github"></i>
      </a>
    </div>
 </div>
  </footer>
</template>

<script>
export default {
  name: "Footer",
  data() {
    return {
      copyRight: ""
    };
  },
  methods: {
    myCopy() {
      this.copyRight = new Date().getFullYear();
    }
  },

  mounted() {
    this.myCopy();
  }
};
</script>

<style scoped>
footer {
width: 100%;
  background: black;
}

.footer {
  width: 70%;
  margin: 0 auto;
  padding: 1em 0;
  color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
footer div:nth-of-type(1) {
  display: flex;
  align-items: center;
}
footer b {
  font-size: 1.7em;
  padding-right: 0.2em;
}
footer i {
  color: white;
  font-size: 2em;
}

@media screen and (max-width: 780px) {
  .footer {
    width: 95%;
  }
}
</style>

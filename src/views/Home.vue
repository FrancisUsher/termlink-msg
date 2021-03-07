<template>
  <div class="home scanlines">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <h1 ref="glitchTitle"></h1>
    <div v-for="(t, i) in messageLines" :key="i">
      <span :ref="`glitchText${i}`"></span>
      <span style="display:none;">{{ t }}</span>
      <br />
    </div>
    <!-- <span ref="glitchText0"></span>
    <br />
    <span ref="glitchText1"></span> -->
    <div class="statusmessage">
      {{ statusMessage }}
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import { glitchWrite } from "glitched-writer";

export default {
  name: "Home",
  components: {
    // HelloWorld,
  },
  data() {
    return {
      message: `Hello Mr. Chavez...
        This is a demo of the secure-term messenger service.
        Do not attempt to reply to this message.
        Do not attempt to determine my identity.
        Your dead drop will be in the dumpster behind SawCon.
        ... 
        ...
        SawCon deez nuts
        LULW KEKW OMEGALUL`,
      heading: "secure-term messenger v1.0",
      decrypted: false,
    };
  },
  computed: {
    messageLines() {
      return this.message.split("\n");
    },
    statusMessage() {
      if (this.decrypted) {
        return "message decrypted successfully";
      } else {
        return "decrypting secure message...";
      }
    },
    // messageLinePromises() {
    //   return this.messageLines.map((message, i) =>
    //     glitchWrite(this.$refs[`glitchText${i}`], message)
    //   );
    // },
  },
  mounted() {
    // console.log(this.$refs);
    glitchWrite(this.$refs.glitchTitle, this.heading)
      .then(() => {
        return this.renderLines();
      })
      // .then(() => {
      //   return glitchWrite(
      //     this.$refs.glitchText0,
      //     "TermLink Messenger 1.0 Alpha initializing..."
      //   );
      // })
      // .then(() => {
      //   return glitchWrite(
      //     this.$refs.glitchText1,
      //     "This is a demo of the termlink-msg service"
      //   );
      // })
      .then(() => {
        this.decrypted = true;
      });
  },
  methods: {
    renderLines() {
      // console.log(this.messageLines);
      return this.messageLines.reduce((chain, line, index) => {
        return chain.then(() => {
          return glitchWrite(this.$refs[`glitchText${index}`][0], line.trim());
        });
      }, Promise.resolve());
    },
  },
};
</script>

<style lang="scss">
.home {
  width: 1080px;
  aspect-ratio: 16 / 9;
  background: radial-gradient(ellipse at 75% 75%, #15221d, #000),
    radial-gradient(ellipse at bottom, #4d9f0c, transparent);
  // border: #42b983 2px;
  box-shadow: 0 0 24px #42b977;
  border-radius: 12px;
  margin: 10px auto;
  padding: 24px;
}

.statusmessage {
  position: absolute;
  bottom: 12px;
  right: 12px;
}
</style>

<template>
    <div class="social-share">
        <a class="twitter-share" :href="twitterShareUrl">
            <img class="twitter-share__image" :src="twitterLogo" alt="Share on Twitter">
            <span class="twitter-share__text">
                <slot></slot>
            </span>
        </a>
    </div>
</template>

<script>
import twitterLogo from "@/assets/twitter-logo-white-on-blue.svg";

const TWITTER_BASE_URL = "https://twitter.com/intent";
const HASHTAGS = [ '5minfeature' ]
export default {
  props: {
    text: {
      type: String
    }
  },
  data() {
    return {
      twitterLogo
    };
  },
  computed: {
    twitterShareUrl() {
      return `${TWITTER_BASE_URL}/tweet?text=${
        this.text
      }&hashtags=${HASHTAGS.join(',')}`;
    }
  }
};
</script>

<style scoped>
.social-share {
  display: flex;
}
.twitter-share {
  display: flex;
  align-items: center;
  font-size: 0.8em;
}
.twitter-share__image {
  width: 2em;
  height: 2em;
  margin-right: 1em;
}
.twitter-share__text {
  display: flex;
}
</style>

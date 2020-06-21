<template>
  <div class="home">
    <h1 class="headline center">Xin's-blog</h1>
    <LazyImg src="https://forthebadge.com/images/badges/built-with-love.svg" />
    <div class="center">
      <h4>
        感谢你发现了我的博客 😁
        <a href="http://github.com/mactanxin" target='_blank'>github</a>
      </h4>
      <h4>下面 👇 这张图来自unsplash, 我没有对图片的尺寸进行设置(希望保留原作者的创作意图)</h4>
    </div>
    <LazyImg 
      lazySrc="https://source.unsplash.com/random"
      lazySrcset="https://source.unsplash.com/random"
      backgroundColor="#3399ff"
    />
    <div class="sections">
      <div v-for="(section, index) in Object.keys(entries)" :key="index" class="group">
        <h2 class="center">{{section}}</h2>
        <div class="section" v-for="entry in entries[section]" :key="entry.id">
          <div class="entry">
            <h3 @click="$router.push({name: entry.id})">
              {{entry.title}}
              <span class="subtitle">{{entry.date}}</span>
            </h3>
            <p>{{entry.description}}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BLOGENTRIES from '@/statics/data/blogs.json'
import LazyImg from '../components/lazyImg';

export default {
  name: 'home',
  computed: {
    entries() {
      return BLOGENTRIES
    }
  },
  components: {
    LazyImg
  }
}
</script>
<style lang="scss" scoped>
.center {
  text-align: center;
}
.headline {
  text-transform: uppercase;
  margin: 4rem auto;
  font-size: 4rem;
}
img {
  display: block;
  margin: 0 auto;
  width: 800px;
}

h2 {
  color: #35495e;
  text-transform: capitalize;
  margin-bottom: 2rem;
}

h3 {
  color: #42b883;
  margin-bottom: 0;
  cursor: pointer;
  &:hover {
    text-decoration: underline;
  }
  .subtitle {
    color: grey;
    font-size: .98rem;
    float: right;
    font-weight: normal;
  }
}

h4 {
  color: #42b883;
  &:hover {
    text-decoration: none;
  }
}

p {
  margin-top: .4rem;
}

.sections {
  max-width: 40vw;
  margin: 0 auto;
  margin-top: 4rem;
}

.section {
  margin-bottom: 3rem;
}

.group {
  margin-bottom: 4rem;
}

</style>

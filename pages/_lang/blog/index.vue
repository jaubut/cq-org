<template>
  <div id="Index">
    <Bloc class="span-2 align-left no-padding overflow-yes whitebg">
      <BigTexte>
          <span>{{ blogCount }}</span>
          <h4>Le blog</h4>
      </BigTexte>
      <p>Le meilleur contenu sur le chanvre au Québec.</p>
    </Bloc>
    <BlocPost v-for="post in posts" :key="post.fields.title" :post="post">
    </BlocPost>
  </div>
</template>

<script>
import {createClient} from '@/plugins/contentful'
import BlocPost from '@/components/cq-blog-post'

const client = createClient()

export default {
  name: 'Index',
  scrollToTop: true,
  components: {
    BlocPost
  },
  data () {
    return {
      filter: null,
      description: 'Obtenez un service qui vous aide à trouver des informations pertinentes sur le chanvre avec notre blog tout en vous connectant aux entreprises québécoises du domaine. Il y a des articles de blogs, des sections d’informations et des profils d’entreprises.',
      title: 'Le blog'
    }
  },
  head () {
    return {
      title: this.title,
      meta: [
        { hid: 'description', name: 'description', content: this.description },
        { hid: 'og:image', property: 'og:image', content: 'https://cq2.imgix.net/img/background-social-media.png?w=320&h=320&' },
        { hid: 'og:description', property: 'og:description', content: this.description },
        { hid: 'og:title', property: 'og:title', content: this.title }
      ]
    }
  },
  mounted () {
    this.$initFbSDK()
  },
  computed: {
    blogCount: function() {
      return this.posts.length
    }
  },
  asyncData ({ env, params }) {
    return client.getEntries({
      'content_type': 'blogPost',
      order: '-sys.createdAt'
    }).then(entries => {
      return {
        posts: entries.items
      }
    })
  }
}
</script>
<style scoped>
  #Index {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-auto-rows: calc(100vh/1.7);
    grid-gap: 10px;
    width: 100%;
  }
  .tag-list {
    display: flex;
    flex-flow: row wrap;
    justify-content: space-around;
    align-items: center;
  }
  span {
    padding: 0 5px;
    color: rgba(#4F5D56, 0.6);
  }
  span:hover {
    color: rgba(#4F5D56, 1);
  }
  @media screen and (max-width: 468px) {
    #Index {
      grid-auto-rows: minmax(calc(100vh/1.7), auto);
    }
  }
</style>

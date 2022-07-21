<template>
  <div>
    <div v-if="fetchState.pending">
      <p>Loading...</p>
    </div>
    <div v-else-if="fetchState.error">
      <p>An error occurred :(</p>
    </div>
    <div v-else>
      <ul>
        <li v-for="post in posts" :key="post.id">
          <div>
            <h2>
              <strong># {{ post.id }}</strong>
              {{ post.title }}
            </h2>
          </div>
          <div>
            <p>{{ post.body }}</p>
          </div>
          <div>
            <nuxt-link :to="`/posts/${post.id}`">Read more</nuxt-link>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import {
  defineComponent,
  useFetch,
  ref,
  useContext,
} from '@nuxtjs/composition-api'

export default defineComponent({
  name: 'MyComponent',
  setup() {
    const { $axios } = useContext()
    const posts = ref([])

    const { fetchState } = useFetch(async () => {
      const res = await $axios.get('/posts')
      posts.value = res.data
    })

    return {
      posts,
      fetchState,
    }
  },
})
</script>

<style></style>

<template>
  <div>
    {{ fetchState }}
    <div v-if="fetchState.pending">
      <p>Loading...</p>
    </div>
    <div v-else-if="fetchState.error">
      <p>An error occurred :(</p>
    </div>
    <div v-else>
      <div>
        <h2>
          <strong># {{ post.id }}</strong>
          {{ post.title }}
        </h2>
      </div>
      <div>
        <p>{{ post.body }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import {
  defineComponent,
  ref,
  useContext,
  useFetch,
  useRoute,
} from '@nuxtjs/composition-api'

export default defineComponent({
  name: 'PostIdPage',
  setup() {
    const route = useRoute()
    const { $axios } = useContext()
    const post = ref([])

    const { fetchState } = useFetch(async () => {
      const res = await $axios.get(`/posts/${route.value.params.pId}`)
      post.value = res.data
    })

    return { post, fetchState }
  },
})
</script>

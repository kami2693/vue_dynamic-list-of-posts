<template>
  <div class="home">
    <h1 class="title home-title">Список постів</h1>
    <PostsList
      ref="postsList"
      @open-post="$emit('open-post', $event)"
      @edit-post="$emit('edit-post', $event)"
    />

    <Sidebar
      :is-open="isSidebarOpen"
      :post="selectedPost"
      @close="$emit('close-sidebar')"
      @post-updated="$emit('post-updated')"
    />
  </div>
</template>

<script>
import { ref } from 'vue'
import PostsList from '../components/PostsList.vue'
import Sidebar from '../components/Sidebar.vue'

export default {
  name: 'HomeView',
  components: {
    PostsList,
    Sidebar
  },
  props: {
    isSidebarOpen: Boolean,
    selectedPost: Object
  },
  setup(props, { expose }) {
    const postsList = ref(null)

    const openPost = (post) => {
      props.selectedPost = post
      props.isSidebarOpen = true
    }

    const editPost = (post) => {
      props.selectedPost = post
      props.isSidebarOpen = true
    }

    const openNewPost = () => {
      props.selectedPost = null
      props.isSidebarOpen = true
    }

    const closeSidebar = () => {
      props.isSidebarOpen = false
      props.selectedPost = null
    }

    const handlePostUpdate = () => {
      if (postsList.value) {
        postsList.value.refreshPosts()
      }
    }

    // Додаю можливість викликати openNewPost ззовні через ref
    expose({ openNewPost })

    return {
      postsList,
      openPost,
      editPost,
      openNewPost,
      closeSidebar,
      handlePostUpdate
    }
  }
}
</script>

<style scoped>
.home {
  padding: 20px;
}

.home-title {
  color: #222 !important;
  font-weight: 800;
  font-size: 2.2rem;
  margin-bottom: 1.5rem;
  letter-spacing: 0.01em;
}
</style> 
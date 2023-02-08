<script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import type { Post, Todo } from '@/api/types';
  import type { AxiosInstance } from 'axios';
  import SectionTitle from '@/components/SectionTitle.vue'
  import PostCard from '@/components/PostCard.vue'
  import TodoCard from '@/components/TodoCard.vue'
  import axios from 'axios';

  const posts = ref<Post[]>([]);
  const todos = ref<Todo[]>([]);

  const client : AxiosInstance = axios.create({
    baseURL: 'https://gorest.co.in/public/v2/',
  });

  const fetchPosts = async () : Promise<void> => {
    let response = await client.get('posts');
    response.data.length = 2;

    posts.value = response.data
  };

  const fetchToDos = async () : Promise<void> => {
    let response = await client.get('users/331282/todos');

    todos.value = response.data
  };

  onMounted(async () => {
    fetchPosts();

    fetchToDos();
  });
</script>

<template>
  <main>
    <h1 className="text-4xl text-center mb-8">
      My dashboard
    </h1>

    <div class="grid gap-8 lg:min-w-[800px]">
      <SectionTitle>
        Latest posts
      </SectionTitle>

      <div class="grid grid-flow-row lg:grid-flow-col gap-8 lg:min-h-[400px]">
        <PostCard 
          v-for="post in posts"
          :key="post.id"
          :title="post.title"
          :description="post.body"
        />
      </div>

      <SectionTitle>
        My ToDo's
      </SectionTitle>

      <div class="grid grid-flow-col gap-8 min-h-[145px]">
        <TodoCard
          v-for="todo in todos"
          :key="todo.id"
          :title="todo.title"
          :deadline="todo.due_on"
          :status="todo.status"
        />
      </div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>

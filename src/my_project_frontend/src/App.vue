<script setup>
import { ref } from 'vue';
import { my_project_backend } from 'declarations/my_project_backend/index';
const blogs = ref([]);
const tags = ref([]);

async function handleSubmit(e) {
  e.preventDefault();
  const target = e.target;
  const title = target.querySelector('#title').value;
  const content = target.querySelector('#content').value;

  await my_project_backend.add_blog(title, content, tags)
  await getBlogs()
}

async function getBlogs() {
  const tempBlogs = await my_project_backend.get_blogs()
  blogs.value = tempBlogs.map((blog) => {
    return {
      ...blog,
      date: blog.date.toString()
    }
  })
}

function saveTag() {
  const tagsContent = document.querySelector('#tags').value;
  tags.value.push(tagsContent)
  document.querySelector('#tags').value = "";
}

function removeTag(id) {
  tags.value.splice(id, 1);
}

getBlogs()
</script>

<template>
  <main class="container mx-auto">
    <img src="/logo2.svg" alt="DFINITY logo" class="mx-auto mt-4"/>
    <br />
    <br />
    <form class="grid gap-4 pb-4 mb-4 border-solid border-b-2 border-indigo-500" action="#" @submit="handleSubmit">
      <div>
        <p class="text-black font-bold">Title: </p>
        <input 
          id="title" 
          alt="title" 
          type="text" 
          class="
            w-full 
            rounded-3xl 
            py-1 
            px-4 
            outline-none
            border-solid 
            border-2
            border-black
            transition 
            duration-150 
            ease-in-out
            hover:border-indigo-500
            "/>
      </div>
      <div>
        <p class="text-black font-bold ">Content: </p>
        <textarea
          id="content" 
          alt="content" 
          type="text" 
          class="
            w-full 
            rounded-3xl 
            py-1 
            px-4 
            outline-none 
            min-h-[100px] 
            border-solid 
            border-2
            border-black
            transition 
            duration-150 
            ease-in-out
            hover:border-indigo-500
          "></textarea>
      </div>
      <div>
        <p class="text-black font-bold">Tags: </p>
        <input 
          id="tags" 
          alt="tags" 
          type="text"
          v-on:keyup.enter="saveTag"
          class="
            w-full 
            rounded-3xl 
            py-1 
            px-4 
            outline-none
            border-solid 
            border-2
            border-black
            transition 
            duration-150 
            ease-in-out
            hover:border-indigo-500"
          />
          <div class="flex gap-1 flex-wrap my-2">
          <div v-for="(tag, id) in tags" class="
          text-white 
          bg-indigo-400 
          rounded-3xl 
          py-1 
          px-4
          text-sm
          w-fit" @click="removeTag(id)">
          {{ tag }}
        </div></div>
      </div>
      <div class="flex justify-end">
        <button 
          class="
          text-white 
          bg-indigo-400 
          rounded-3xl 
          py-1 
          px-4
          transition 
          duration-150 
          ease-in-out
          hover:scale-110
          " type="submit">
            Click to add!
        </button>
      </div>
    </form>
    <div>
      <div v-for="blog in blogs" class="pb-4 border-solid border-b-2 border-indigo-500 mb-4"> 
        <div class="
          mb-1
          text-right
        ">{{ new Date(Number(blog.date) / 1_000_000).toLocaleString() }}</div>
        <h3 class="text-xl mb-2">{{ blog.title }} </h3>
        <p>{{ blog.content }}</p>
        <div class="mt-2">
          <div class="flex gap-2 flex-wrap">
            <div v-for="tag in blog.tags" class="
              text-white 
              bg-indigo-400 
              rounded-3xl 
              py-1 
              px-4
              text-sm
              w-fit"> {{ tag }}</div>
            </div>
        </div>
      </div>
    </div>
  </main>
</template>
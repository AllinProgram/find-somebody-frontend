<template>
  <div class="flex flex-col justify-center items-center">
    <div>
      <h1 class="card-title">“{{ content }}”</h1>
      <div class="card-actions justify-end">
        <a v-bind:href="source" target="_blank">——《{{ name }}》{{ author }}</a>
      </div>
    </div>

    <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1)">
      留言板
    </div>

    <p>欢迎你来到这里，同时也欢迎你在这里畅所欲言，分享自己的故事~</p>

    <div class="container">
      <form @submit.prevent="submitComment">
        <div class="mb-3">
          <label for="nickname" class="form-label">Nickname</label>
          <input
            type="text"
            class="form-input"
            id="nickname"
            v-model="nickname"
            required
          />
        </div>
        <div class="mb-3">
          <label for="content" class="form-label">Content</label>
          <textarea
            class="form-textarea"
            id="content"
            rows="3"
            v-model="content"
            required
          ></textarea>
        </div>
        <div class="mb-3">
          <label for="contact" class="form-label">Contact</label>
          <input type="text" class="form-input" id="contact" v-model="contact" required />
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>

    <!-- TODO 需要增加提交评论按钮~ -->

    <!-- TODO 这里的样式有蛮大问题~ -->
    <div class="chat chat-start" v-for="comment in commentList">
      <div class="chat-header">
        <b>{{ comment.nickname }}</b>
        <time class="text-xs opacity-50"></time>
      </div>
      <div class="chat-bubble">{{ comment.content }}</div>
      <div class="chat-footer opacity-50">{{ comment.publishTime }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      content: "",
      author: "",
      source: "https://hitokoto.cn/?uuid=",
      commentList: [],
    };
  },
  created() {
    this.fetchHitokoto();
    this.fetchAllComment();
  },
  methods: {
    fetchHitokoto() {
      fetch("https://v1.hitokoto.cn/?c=i")
        .then((response) => response.json())
        .then((data) => {
          this.name = data.from;
          this.content = data.hitokoto;
          this.author = data.from_who;
          this.source += data.uuid;
        })
        .catch((error) => {
          console.error("Error fetching quote:", error);
        });
    },
    fetchAllComment() {
      fetch("http://localhost:8080/api/comment")
        .then((response) => response.json())
        .then((result) => {
          this.commentList = JSON.parse(result.data).reverse();
        })
        .catch((error) => {
          console.error("Error fetching comments:", error);
        });
    },
  },
  mounted() {},
};
</script>

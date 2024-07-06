<template>
  <div class="flex flex-col justify-center items-center">
    <div>
      <h1 class="card-title">“{{ hitokoto }}”</h1>
      <div class="card-actions justify-end">
        <a v-bind:href="source" target="_blank">——《{{ name }}》{{ author }}</a>
      </div>
    </div>

    <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1)">
      留言板
    </div>

    <p>欢迎你来到这里，同时也欢迎你在这里畅所欲言，分享自己的故事~</p>

    <!-- TODO 提交后数据插入现在的list -->
    <div class="container">
      <form @submit.prevent="submitComment">
        <label class="input input-bordered flex items-center mt-2">
          称呼
          <input
            v-model="nickname"
            type="text"
            class="grow"
            placeholder="怎么称呼你？"
            required
          />
        </label>
        <label class="input input-bordered flex items-center mt-2">
          联系
          <input
            v-model="contact"
            type="text"
            class="grow"
            placeholder="可以是邮箱甚至可以是邮编！"
            required
          />
        </label>
        <label class="input input-bordered flex items-center mt-2">
          留言
          <input
            v-model="content"
            type="text"
            class="grow"
            placeholder="若有什么建议或者意见欢迎随时提交给我～"
            required
          />
        </label>
        <div class="flex justify-end mt-2">
          <button class="btn btn-success" type="submit">提交</button>
        </div>
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
      // 一言
      name: "",
      hitokoto: "",
      author: "",
      source: "",

      // 留言
      nickname: "",
      contact: "",
      content: "",
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
          this.hitokoto = data.hitokoto;
          this.author = data.from_who;
          this.source += "https://hitokoto.cn/?uuid=" + data.uuid;
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
    async submitComment() {
      try {
        const response = await fetch("http://localhost:8080/api/comment", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            nickname: this.nickname,
            contact: this.contact,
            content: this.content,
          }),
        });
        this.response = await response.json();
      } catch (error) {
        console.error(error);
        this.response = { code: 0, message: "请联系公众号进行处理！" };
      }
    },
  },
  mounted() {},
};
</script>

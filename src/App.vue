<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addContent">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="select">
          <option :value="item.key" v-for="item in optionData">{{ item.value }}</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="text"></textarea>
      </div>

      <button class="btn primary" :disabled="text.length < 4">Добавить</button>
    </form>

    <app-right-block
        :data="data"
    ></app-right-block>
  </div>
  <div class="container">
    <app-comment-block
        :comments="comments"
        @load="getComments"

    ></app-comment-block>

    <app-loader v-if="loader === true"></app-loader>
  </div>
</template>

<script>
import AppRightBlock from "./AppRightBlock.vue";
import AppCommentBlock from "./AppCommentBlock.vue";
import AppLoader from "./AppLoader.vue";

export default {
  data() {
    return {
      optionData: [
        { key: 'title', value: 'Заголовок' },
        { key: 'subtitle', value: 'Подзаголовок' },
        { key: 'avatar', value: 'Аватар' },
        { key: 'text',value: 'Текст' },
      ],
      select: 'title',
      text: '',
      data: [],
      comments: [],
      loader: false
    }
  },
  methods: {
    getComments() {
      this.loader = true
      setTimeout(async () => {
        const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42', {
          method: 'GET',
          headers: {
            'Content-Type': 'application/json'
          }
        });
        this.comments = await response.json()
        this.loader = false
      },500)

    },
    async addContent() {
      // https://myjsapp-c2f0f-default-rtdb.firebaseio.com/sections.json
      const response = await fetch('https://myjsapp-c2f0f-default-rtdb.firebaseio.com/sections.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          key: this.select,
          value: this.text
        })
      })

      const firebaseData = await response.json()
      this.data.push({
        key: this.select,
        value: this.text
      })
      this.text = ''
      this.select = 'title'

    },
    async getContent() {
      const response = await fetch('https://myjsapp-c2f0f-default-rtdb.firebaseio.com/sections.json', {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json'
        }
      })

      const data = await response.json();
      this.data = Object.keys(data).map((item) => {
        return {
            key: data[item]['key'],
            value: data[item]['value']
        }
      })
    }
  },
  mounted() {
    this.getContent()
  },
  components: {
    AppRightBlock, AppCommentBlock, AppLoader
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>

<template>
  <p>
    <button class="btn primary" @click="$emit('load')" v-if="comments.length === 0">Загрузить комментарии</button>
  </p>
  <div class="card"  v-if="comments.length !== 0">
    <h2>Комментарии</h2>
    <ul class="list">
      <li class="list-item" v-for="comment in comments">
        <div>
          <p><strong>{{ comment.email }}</strong></p>
          <small>{{comment.body}}</small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "AppComment",
  props: {
    comments: {
      type: Array,
      required: true
    }
  },
  emits: {
    load: {
      type: Function,
      required: true
    }
  }

}
</script>

<style scoped>

</style>
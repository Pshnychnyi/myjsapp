<template>
  <div class="card card-w70">
    <component
        v-for="item in data"
        :is="'item-' + item.key"
        v-bind="item"
    ></component>

    <h3 v-if="data.length === 0">Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
import ItemAvatar from "./Components/ItemImage.vue";
import ItemSubtitle from "./Components/ItemSubtitle.vue";
import ItemText from "./Components/ItemText.vue";
import ItemTitle from "./Components/ItemTitle.vue";


export default {
  name: "AppRightBlock",
  props: {
    data: {
      type: Object
    }
  },
  components: {
    ItemAvatar, ItemSubtitle, ItemText, ItemTitle
  }

}
</script>

<style scoped>

</style>
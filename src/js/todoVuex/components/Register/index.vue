<template>
  <form class="register">
    <div class="register__input">
      <p class="register__input__title">やることのタイトル</p>
      <input
        type="text"
        name="title"
        placeholder="ここにTODOのタイトルを記入してください"
        v-model="title"
      >

    </div>
    <div class="register__input">
      <p class="register__input__title">やることの内容</p>
      <textarea
        name="detail"
        :rows="rows"
        placeholder="ここにTODOの内容を記入してください。"
        v-model="detail"
      />
    </div>
    <div class="register__submit">
      <button
        class="register__submit__btn"
        type="button"
        name="addButton"
        @click="(targetTodo.id === null) ? addTodo() : editTodo()"
      >
        <template v-if="targetTodo.id === null">
          <span>登録する</span>
        </template>
        <template v-else>
          <span>変更する</span>
        </template>
      </button>
    </div>
  </form>
</template>

<script>
export default {
  computed: {
    targetTodo() {
      return this.$store.state.targetTodo;
    },
    title: {
      get() {
        return this.$store.state.targetTodo.title;
      },
      set(value) {
        this.$store.dispatch({
          type: 'updateTargetTodo',
          name: 'title',
          value,
        });
      },
    },
    detail: {
      get() {
        return this.$store.state.targetTodo.detail;
      },
      set(value) {
        this.$store.dispatch({
          type: 'updateTargetTodo',
          name: 'detail',
          value,
        });
      },
    },
    rows() {
      const num = this.targetTodo.detail.split('\n').length;
      return (num > 3) ? num : 3;
    },
  },
  methods: {
    addTodo() {
      return this.$store.dispatch('addTodo', this.targetTodo);
    },
    editTodo() {
      return this.$store.dispatch('editTodo');
    },
  },
};
</script>

<style lang="scss" scoped>
.register {
  padding: 10px 20px;
  padding-bottom: 0;
  &__inner {
    width: 80%;
  }
  &__input {
    & + & {
      margin-top: 10px;
    }
    &__title {
      font-weight: bold;
      font-size: 14px;
    }
    input, textarea {
      padding: 10px;
      width: 100%;
      font-size: 14px;
      border: 1px solid #ddd;
    }
  }
  &__submit {
    margin-top: 10px;
    text-align: right;
    &__btn {
      padding: 10px 25px;
      color: #fff;
      font-size: 12px;
      border-radius: 7px;
      background-color: #54b363;
    }
  }
}
</style>

<template>
  <tr>
    <td>
      <span v-if="!editable" @click="edit()">{{ user.nickname }}</span>
      <input
        v-show="editable"
        ref="editNickname"
        v-model="user.nickname"
        @blur="user.editable = false"
      />
    </td>
    <td>{{ user.email }}</td>
  </tr>
</template>

<script lang="ts">
import { defineComponent, ref, nextTick, PropType } from '@vue/composition-api';

export interface User {
  nickname: string;
  email: string;
}

const userValidator = (user: User) => {
  if (!user || !user.nickname || !user.email) {
    return false;
  }
  return true;
};

export default defineComponent({
  props: {
    user: {
      type: Object as PropType<User>, // point
      required: true,
    },
  },
  // リアクティブな変数は setup() 内で定義
  setup() {
    const editable = ref(false);
    const editNickname = ref<HTMLFormElement | null>(null); // point
    const edit = () => {
      editable.value = true;
      nextTick(() => {
        // DOM更新後に実行
        // eslint-disable-next-line @typescript-eslint/no-non-null-assertion
        editNickname.value!.focus(); // !はnon nullであることをコンパイラに伝える
      });
    };
    // テンプレートからアクセスする変数、関数はリターンすること
    return {
      editable,
      editNickname,
      edit,
    };
  }
});
</script>

<style scoped>
td input {
  width: 95%;
}
</style>

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
//import Vue from 'vue';
import { Component, Vue, Prop } from 'vue-property-decorator';

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

@Component
export default class UserRowComponent extends Vue {
  // propsオプション: `@Prop()`アノテーションをつけてprivateで宣言
  //@Prop({ required: true, validator: userValidator })
  @Prop({ required: true })
  private user!: User;

  // dataオプション: クラス変数としてprivateで定義
  private editable = false;

  // methodsオプション: 同様にクラスメソッドとしてprivateで定義
  private edit() {
    this.editable = true;
    this.$nextTick(() => {
      // DOM更新後に実行
      (this.$refs.editNickname as HTMLFormElement).focus();
    });
  }
}
</script>

<style scoped>
td input {
  width: 95%;
}
</style>

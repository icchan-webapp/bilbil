<template>
  <Header>C O N T E N T</Header>
  <main class="container my-5">
    <div class="row mx-2">
      <div class="col-lg-6 mx-auto">
        <!-- ページ遷移リンク -->
        <div class="mb-3">
          <div class="mb-2">
            <a class="py-1" data-method="delete" href="/admin/sign_out"
              >ログアウトする。</a
            >
          </div>
          <div>
            <router-link class="py-1" to="/admin/content/new"
              >トークテーマ・カテゴリーの作成へ。</router-link
            >
          </div>
        </div>
        <!-- /ページ遷移リンク -->
        <!-- 各カテゴリーのトークテーマ一覧 -->
        <div
          class="row dropdown"
          v-for="category in categories"
          :key="category.id"
        >
          <dl
            class="col-sm-11 col-10 d-flex flex-wrap bg-dark text-white p-2 m-0"
          >
            <dt class="w-25 me-3">{{ category.name }}</dt>
            <dd class="me-auto">
              <router-link
                class="btn btn-success me-2"
                :to="{
                  name: 'CategoryEditPage',
                  params: { id: category.id },
                }"
                >編集</router-link
              >
              <a class="btn btn-danger" @click="deleteCategory(category.id)"
                >削除</a
              >
            </dd>
            <dd class="me-1">
              トークテーマ数: {{ category.talk_themes.length }}
            </dd>
          </dl>
          <button
            type="button"
            class="btn btn-dark dropdown-toggle col-sm-1 col-2"
            data-bs-toggle="dropdown"
            data-bs-auto-close="false"
          ></button>
          <ol class="dropdown-menu">
            <li
              class="dropdown-item"
              v-for="talk_theme in category.talk_themes"
              :key="talk_theme.id"
              v-if="category.talk_themes.length != 0"
            >
              <div class="row d-inline-flex flex-wrap m-0">
                <div class="col-12 col-sm-6 mb-2 text-wrap">
                  {{ talk_theme.content }} ?
                </div>
                <div class="col-12 col-sm-6">
                  <router-link
                    class="btn btn-success me-2"
                    :to="{
                      name: 'TalkThemeEditPage',
                      params: { id: talk_theme.id },
                    }"
                    >編集</router-link
                  >
                  <a
                    class="btn btn-danger me-2"
                    @click="deleteTalkTheme(talk_theme.id)"
                    >削除</a
                  >
                  <span> いいね数: {{ talk_theme.likes.length }} </span>
                </div>
              </div>
            </li>
            <li class="d-block px-1 py-2" v-else>
              <dl class="m-0">
                <dt>トークテーマはありません。</dt>
              </dl>
            </li>
          </ol>
        </div>
        <!-- 各カテゴリーのトークテーマ一覧 -->
      </div>
    </div>
  </main>
  <admin-footer></admin-footer>
</template>

<script>
import axios from "axios";

import Header from "../components/Header.vue";
import AdminFooter from "../components/AdminFooter.vue";

export default {
  components: {
    Header,
    AdminFooter,
  },
  data() {
    return {
      categories: [], // 全カテゴリーのデータの配列。
      talk_themes: [], // 全トークテーマのデータの配列。
    };
  },
  created() {
    // 全カテゴリーのデータを取得し、いいね数が多い順にトークテーマを並び替える。
    axios.get("/api/v1/categories").then((response) => {
      this.categories = response.data;
      this.sortedTalkThemesByLikes;
    });

    // 全トークテーマのデータの配列。
    axios
      .get("/api/v1/talk_themes")
      .then((response) => (this.talk_themes = response.data));
  },
  computed: {
    // トークテーマをいいね数が多い順に並び替える。
    sortedTalkThemesByLikes() {
      this.categories.forEach((category) => {
        category.talk_themes.sort((a, b) => {
          return b.likes.length - a.likes.length;
        });
      });
    },
  },
  methods: {
    // 特定のカテゴリーを削除する。
    deleteCategory(delete_id) {
      if (
        confirm(`「${this.categoryName(delete_id)}」を削除してよろしいですか?`)
      )
        axios.delete(`/api/v1/categories/${delete_id}`).then(() => {
          this.updateContents();
        });
    },

    // 特定のトークテーマを削除する。
    deleteTalkTheme(delete_id) {
      if (
        confirm(
          `「${this.talkThemeContent(delete_id)}」を削除してよろしいですか?`
        )
      )
        axios.delete(`/api/v1/talk_themes/${delete_id}`).then(() => {
          this.updateContents();
        });
    },

    // 削除したいカテゴリーのカテゴリー名を取得する。
    categoryName(delete_id) {
      const filterDate = this.categories.filter(
        (category) => category.id === delete_id
      );
      return filterDate[0].name;
    },

    // 削除したいトークテーマの内容を取得する。
    talkThemeContent(delete_id) {
      const filterDate = this.talk_themes.filter(
        (talk_theme) => talk_theme.id === delete_id
      );
      return filterDate[0].content;
    },

    // 全カテゴリーのデータを取得する。
    updateContents() {
      axios
        .get("/api/v1/categories")
        .then((response) => (this.categories = response.data));
    },
  },
};
</script>

<style scoped>
.bg-dark {
  border-bottom: 1px solid #fff;
}
.dropdown-toggle {
  border-radius: 0;
  border-bottom: 1px solid #fff;
}

.dropdown-menu {
  position: static !important;
  transform: translate(0, 0) !important;
  border-radius: 0;
}
.dropdown-item {
  display: list-item;
  list-style: decimal inside;
}

.d-inline-flex {
  width: 95%;
}
</style>



<template>
  <div>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">작성자</th>
            <th class="text-center">글 제목</th>
            <th class="text-left">날짜</th>
          </tr>
        </thead>
        <tbody>
          <list-row
            v-for="(board, idx) in boards"
            :key="idx"
            :board="board"
          ></list-row>
        </tbody>
      </template>
    </v-simple-table>

    <div v-if="boards.length">
      <div class="d-flex justify-content-center container">
        <nav aria-label="...">
          <ul class="pagination">
            <div class="text-center">
              <v-pagination
                v-model="page"
                :length="totalPage"
                :total-visible="7"
                @input="movePage"
              ></v-pagination>
            </div>
          </ul>
        </nav>
      </div>
    </div>
    <div v-else>글이 없습니다.</div>
    <div class="text-right"></div>

    <v-btn depressd color="primary" @click="write">등록</v-btn>
  </div>
</template>


<script>
import ListRow from "@/components/Row.vue";
import { mapGetters } from "vuex";
export default {
  name: "list",
  components: {
    ListRow,
  },
  data: function () {
    return {
      boarda: [{ curpage: "" }],
      page: 1,
      // items: [],
      // pages: [],
      // board: "",
      // page: "",
    };
  },
  created() {
    this.$store.dispatch("getBoards", this.boarda); // list들 받아오기
  },
  computed: {
    ...mapGetters(["boards", "startPage", "endPage", "curPage", "totalPage"]),
  },
  methods: {
    write() {
      this.$router.push("write");
    },
    range(start, end) {
      let list = [];
      for (let i = start; i <= end; i++) {
        list.push(i);
      }
      return list;
    },
    movePage(page) {
      this.$store.dispatch("movePage", page);
    },
    // next() {
    //   var page = this.$store.getters.endPage;
    //   this.$store.dispatch("movePage", page + 1);
    // },
    // previous() {
    //   var page = this.$store.getters.startPage;
    //   this.$store.dispatch("movePage", page - 1);
    // },
  },
};
</script>


<style>
.page-item {
  list-style: none;
}
</style>
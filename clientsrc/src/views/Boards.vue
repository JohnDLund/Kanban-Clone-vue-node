<template>
  <div class="boards">
    <h2 class="text-light p-3">WELCOME TO YOUR BOARDS</h2>
    <div class="row justify-content-center">
      <form class @submit.prevent="addBoard">
        <input
          class="form-control text-center text-capitalize mb-1"
          type="text"
          placeholder="title"
          v-model="newBoard.title"
          required
        />
        <input
          class="form-control text-center text-capitalize"
          type="text"
          placeholder="description"
          v-model="newBoard.description"
        />
        <button class="btn btn-success mt-2" type="submit">Create Board</button>
      </form>
    </div>

    <div class="row py-3">
      <editBoard class="col-12 col-md-4" v-for="board in boards" :boardData="board" :key="board.id"></editBoard>
    </div>
  </div>
</template>

<script>
import editBoard from "../components/editBoard";
export default {
  name: "boards",

  mounted() {
    this.$store.dispatch("getBoards");
  },
  data() {
    return {
      newBoard: {
        title: "",
        description: "",
      },
    };
  },
  computed: {
    boards() {
      return this.$store.state.boards;
    },
    user() {
      return this.$store.state.user;
    },
  },
  methods: {
    addBoard() {
      this.$store.dispatch("addBoard", this.newBoard);
      this.newBoard = { title: "", description: "" };
    },
  },
  components: {
    editBoard,
  },
};
</script>
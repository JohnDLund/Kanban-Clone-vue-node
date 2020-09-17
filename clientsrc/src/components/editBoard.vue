<template>
  <div class="editBoard">
    <div
      class="bg-dark text-primary p-2 m-2 rounded border border-white d-flex justify-content-between shadow-lg"
    >
      <i class="fa fa-2x fa-pencil text-warning" @click="editBoardClicked = !editBoardClicked"></i>
      <router-link :to="{name: 'board', params: {boardId: boardData.id}}">
        <h3 class="text-capitalize">{{boardData.title}}:</h3>
        <h3 class="text-capitalize">{{boardData.description}}</h3>
      </router-link>
      <i class="fa fa-2x fa-trash-o text-danger" @click="removeBoard(boardData.id)"></i>
    </div>
    <form
      v-if="editBoardClicked == true"
      class="form mx-2 shadow-lg"
      @submit="editBoard(boardData.id)"
    >
      <input
        type="text"
        class="form-control mb-1 text-capitalize"
        placeholder="Edit list ..."
        v-model="editedBoardObject.title"
      />
      <input
        type="text"
        class="form-control mb-1 text-capitalize"
        placeholder="Edit description ..."
        v-model="editedBoardObject.description"
      />
      <button type="submit" class="btn btn-block btn-success">Save Change</button>
    </form>
  </div>
</template>


<script>
export default {
  name: "editBoard",
  props: ["boardData"],
  data() {
    return {
      editedBoardObject: {},
      editBoardClicked: false,
    };
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
  },
  methods: {
    editBoard(id) {
      let editedBoardTitle = this.editedBoardObject.title
        ? this.editedBoardObject.title
        : this.boardData.title;
      let editedBoardDescription = this.editedBoardObject.description
        ? this.editedBoardObject.description
        : this.boardData.description;
      this.$store.dispatch("editBoard", {
        id: id,
        title: editedBoardTitle,
        description: editedBoardDescription,
        creatorEmail: this.user.email,
      });
      this.editBoardClicked = false;
      this.editedBoardObject.title = "";
      this.editedBoardObject.description = "";
    },
    removeBoard(id) {
      this.$store.dispatch("deleteBoard", id);
    },
  },
  components: {},
};
</script>


<style scoped>
</style>
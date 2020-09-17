<template>
  <div dropzone="list" @dragover.prevent @drop.prevent="moveTask()">
    <div class="d-flex justify-content-between">
      <i class="fa fa-pencil text-warning" @click="editBoardClicked = !editBoardClicked"></i>

      <i class="fa fa-trash-o text-danger" @click="removeList"></i>
    </div>
    <form v-if="editBoardClicked == true" class="form" @submit="editList">
      <input
        type="text"
        class="form-control mb-2"
        placeholder="Edit list ..."
        v-model="editedListObject.title"
      />
    </form>

    <h4 class="lists p-2 text-capitalize">{{listData.title}}</h4>
    <tasks
      class="rounded border border-primary bg-primary m-2"
      v-for="task in tasks"
      :taskData="task"
      :listId="listData.id"
      :key="task.id"
      draggable="true"
    ></tasks>
    <div class="input-group my-4 d-flex justify-content-center">
      <div class="input-group-prepend"></div>
      <form @submit="createTask">
        <input
          type="text"
          class="form-control text-capitalize"
          placeholder="Enter New Task ..."
          v-model="newTaskObject.title"
        />
      </form>
    </div>
  </div>
</template>


<script>
import tasks from "../components/tasks";
export default {
  name: "lists",
  props: ["listData"],
  data() {
    return {
      editedListObject: {},
      newTaskObject: {},
      editBoardClicked: false,
    };
  },
  mounted() {
    this.$store.dispatch("getTasks", this.listData.id);
  },

  computed: {
    tasks() {
      return this.$store.state.tasks[this.listData.id];
    },
    user() {
      return this.$store.state.user;
    },
  },

  methods: {
    moveTask() {
      let task = JSON.parse(event.dataTransfer.getData("data"));
      let moveData = {
        oldListId: event.dataTransfer.getData("list"),
        taskToMove: task,
        newListId: this.listData.id,
      };
      console.log(moveData);
      this.$store.dispatch("moveTask", moveData);
    },
    removeList() {
      this.$store.dispatch("deleteList", this.listData);
    },

    editList() {
      this.$store.dispatch("editList", {
        id: this.listData.id,
        title: this.editedListObject.title,
        boardId: this.listData.boardId,
        creatorEmail: this.user.email,
      });
      this.editBoardClicked = false;
      this.editedListObject.title = "";
    },

    createTask() {
      this.$store.dispatch("createTask", {
        title: this.newTaskObject.title,
        listId: this.listData.id,
        creatorEmail: this.user.email,
      });
      this.newTaskObject.title = "";
    },
  },

  components: {
    tasks,
  },
};
</script>


<style>
</style>
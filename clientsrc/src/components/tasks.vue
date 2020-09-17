<template>
  <div @dragstart="moveTask()">
    <div class="tasks d-flex justify-content-between p-2 shadow-lg">
      <i class="fa fa-pencil text-warning" @click="editTaskClicked = !editTaskClicked"></i>

      <h5
        @click="commentsClicked = !commentsClicked"
        class="text-capitalize text-wrap"
        data-toggle="tooltip"
        data-placement="top"
        title="Click to show/hide Comments"
      >{{taskData.title}}</h5>
      <i class="fa fa-trash-o text-danger" @click="deleteTask"></i>
    </div>
    <form v-if="editTaskClicked == true" class="form m-1" @submit="editTask">
      <input
        type="text"
        id="editTask"
        class="form-control mb-2"
        placeholder="Edit task ..."
        v-model="editedTaskObject.title"
      />
    </form>
    <div v-if="commentsClicked">
      <comment
        v-for="comment in comments"
        :commentData="comment"
        :taskId="taskData.id"
        :listId="taskData.listId"
        :key="comment.id"
      ></comment>
      <div class="input-group my-4 d-flex justify-content-center">
        <div class="input-group-prepend"></div>
        <form @submit="createComment">
          <input
            type="text"
            class="form-control text-capitalize"
            placeholder="Enter New Comment ..."
            v-model="newCommentObject.comment"
          />
        </form>
      </div>
    </div>
  </div>
</template>


<script>
import comment from "../components/comment";
export default {
  mounted() {
    $(function () {
      $('[data-toggle="tooltip"]').tooltip();
    });
  },
  name: "tasks",
  props: ["taskData", "listId"],
  data() {
    return {
      commentsClicked: false,
      editTaskClicked: false,
      editedTaskObject: {},
      newCommentObject: {},
    };
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    comments() {
      return this.taskData.comments;
    },
    lists() {
      return this.$store.state.lists;
    },
  },
  methods: {
    moveTask() {
      event.dataTransfer.setData("data", JSON.stringify(this.taskData));
      event.dataTransfer.setData("list", this.listId);
    },
    // changeList(listId) {
    //   this.$store.dispatch("changeList", {
    //     listId: listId,
    //     oldId: this.taskData.listId,
    //     taskId: this.taskData.id,
    //   });
    // },

    createComment() {
      this.$store.dispatch("createComment", {
        comment: this.newCommentObject.comment,
        user: this.user.name,
        taskId: this.taskData.id,
        listId: this.taskData.listId,
      });
      this.newCommentObject.comment = "";
    },

    deleteTask() {
      this.$store.dispatch("deleteTask", this.taskData);
    },
    editTask() {
      this.$store.dispatch("editTask", {
        id: this.taskData.id,
        title: this.editedTaskObject.title,
        listId: this.taskData.listId,
        creatorEmail: this.user.email,
      });
      this.editTaskClicked = false;
      this.editedTaskObject.title = "";
    },
  },
  components: {
    comment,
  },
};
</script>


<style scoped>
</style>
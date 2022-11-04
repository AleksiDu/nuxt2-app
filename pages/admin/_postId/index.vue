<template>
  <div class="admin-post-page">
    <section class="update-form">
      <AdminPostForm :post="loadedPost" @submit="onSubmitted" />
    </section>
  </div>
</template>

<script>
import AdminPostForm from "@/components/Admin/AdminPostForm";
import axios from "axios";

export default {
  layout: "admin",
  components: {
    AdminPostForm,
  },
  asyncData(context) {
    return context.app.$axios
      .$get(process.env.baseUrl + "/posts/" + context.params.postId + ".json")
      .then((data) => {
        return {
          loadedPost: { ...data, id: context.params.postId },
        };
      })
      .catch((e) => context.error());
  },
  methods: {
    onSubmitted(editedPost) {
      axios
        .put(
          "https://nuxt2-app-default-rtdb.firebaseio.com/posts/" +
            this.$router.params.id +
            ".json",
          editedPost
        )
        .then((res) => {
          this.$router.push("/admin");
        })
        .catch((e) => console.log(e));
    },
  },
};
</script>

<style scoped>
.update-form {
  width: 90%;
  margin: 20px auto;
}

@media (min-width: 768px) {
  .update-form {
    width: 500px;
  }
}
</style>

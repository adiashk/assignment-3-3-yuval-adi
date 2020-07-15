<template>
  <div class="container " style="color:black;">
    <div class="row">
      <div class="card mb-3" id="p" style="max-width: 1080px;">
        <div class="row no-gutters">
          <div class="col-md-6">
            <b-card-img
              :src="userInfo.image"
              class="rounded-1"
              alt="user image"
            ></b-card-img>
            <div class="col-md-1"></div>
          </div>

          <div class="col-md-6" style="color:black;" id="q">
            <div class="card-body">
              <dt class="card-title">
                User: {{ this.$root.store.username }}
              </dt>
              <dt class="card-text">First name: {{ userInfo.firstName }}</dt>
              <dt class="card-text">Last name: {{ userInfo.lastName }}</dt>
              <dt class="card-text">email: {{ userInfo.email }}</dt>
              <dt class="card-text">Country: {{ userInfo.country }}</dt>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!--  -->
</template>

<script>
export default {
  data() {
    return {
      userInfo: Object,
    };
  },
  mounted() {
    this.getUserInfo();
  },
  methods: {
    async getUserInfo() {
      try {
        if (this.$root.store.username) {
          console.log(
            "this.$root.store.username: " + this.$root.store.username
          );
          const response = await this.axios.get(
            this.$root.store.base_url + "/user/getUserInfo",
            {
              params: { username: this.$root.store.username },
            }
          );
          console.log(response.data.profileInformation);
          const info = response.data.profileInformation;
          console.log(info);
          //this.userInfo.push(...info);
          this.userInfo = info;
          console.log(userInfo);
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
  props: {
    Profile: {
      type: Object,
      required: true,
    },
  },
};
</script>
<style>
#q {
  background-color: rgba(255, 255, 255, 0) !important;
}

#p {
  background-color: rgba(255, 255, 255, 0.24) !important;
}
</style>

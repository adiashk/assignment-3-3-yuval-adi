<template>
  <b-card no-body class="overflow-hidden" style="max-width: 540px;">
    <router-link
      :to="{ name: 'Profile', params: { username: username } }"
      class="profile-view"
    >
      <b-card-body>
        <b-row no-gutters>
          <b-col md="12">
            <b-card-img :src="userInfo.image" class="rounded-0"></b-card-img>
          </b-col>
          <b-col md="12">
            <b-card-body :user="this.$root.store.username">
              <b-card-text> User: {{ this.$root.store.username }} </b-card-text>
              <b-card-text> First name: {{ userInfo.firstName }} </b-card-text>
              <b-card-text> Last Name: {{ userInfo.lastName }} </b-card-text>
              <b-card-text> email: {{ userInfo.email }} </b-card-text>
              <b-card-text> Country: {{ userInfo.country }} </b-card-text>
            </b-card-body>
          </b-col>
        </b-row>
      </b-card-body>
    </router-link>
  </b-card>
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

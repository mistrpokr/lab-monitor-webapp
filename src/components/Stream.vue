<template>
  <v-card class="mb-4">
    <v-card-title>
      直播控制
    </v-card-title>
    <v-divider></v-divider>
    <v-card-text>
      直播状态：
      <div class="pt-2" v-if="liveStatus">
        <v-icon class="px-2">mdi-account-music</v-icon>
        <b>在线</b>
      </div>
      <div class="pt-2" v-else>
        <v-icon class="px-2">mdi-account-question</v-icon>
        <b>离线</b>
      </div>
    </v-card-text>
    <v-card-actions>
      <v-dialog
          v-model="warningDialog"
          width="500"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" v-bind="attrs" v-on="on">重启直播</v-btn>
        </template>

        <v-card>
          <v-card-title class="">
            提示
          </v-card-title>
          <v-divider></v-divider>
          <v-card-text class="mt-4">
            重启时将短暂中断后台视频录制，可能产生至多数十秒的画面缺失。
            <br/>
            是否确认重启？
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
                color="red"
                @click="restartClickWrapper"
            >
              确定
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-card-actions>
  </v-card>
</template>

<script>
import axios from "axios";

export default {
  name: "Stream",
  data() {
    return {
      warningDialog: false,
      liveStatus: false,
    };
  },
  methods: {
    restartClickWrapper() {
      this.warningDialog = false;
      this.restartStream();
    },
    restartStream() {
      axios
          .post("/api/live/")
          .then(response => console.log(response.data))
    },
    checkStream() {
      let that = this;
      axios
          .get("/api/live")
          .then(function (response) {
            response.data.status === 1 ? that.liveStatus = true : that.liveStatus = false;
          })
    }
  },
  mounted() {
    this.timer = setInterval(() => {
      this.checkStream();
    }, 1000);
  }
}
</script>

<style scoped>

</style>
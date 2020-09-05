<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8>
      <v-card
        min-width="400
      "
      >
        <v-snackbar v-model="snackbar" :timeout="6000" top>
          {{ message }}
          <v-btn color="pink" flat @click="snackbar = false">Close</v-btn>
        </v-snackbar>
        <v-card-title>
          <h1>Hoho chat</h1>
        </v-card-title>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field
              v-model="name"
              :counter="16"
              :rules="nameRules"
              label="Your name"
              required
            ></v-text-field>

            <v-text-field
              v-model="room"
              :rules="roomRules"
              label="Enter room"
              required
            ></v-text-field>

            <v-btn
              :disabled="!valid"
              color="success"
              class="mr-4"
              @click="submit"
            >
              Enter
            </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  layout: "empty",
  head: {
    title: "govno-zhopa"
  },
  data: () => ({
    valid: true,
    name: "",
    nameRules: [
      v => !!v || "Name is required",
      v => (v && v.length <= 16) || "Name must be less than 16 characters"
    ],
    room: "",
    roomRules: [v => !!v || "Room is required"],
    snackbar: false,
    message: ""
  }),
  mounted() {
    const { message } = this.$route.query;
    if (message === "noUser") {
      this.message = "Input data";
    } else if ((message = "leftChat")) {
      this.message = "You has left the chat";
    }

    this.snackbar = !!this.message
  },
  sockets: {
    connect: function() {
      console.log("socket connected");
    }
  },
  methods: {
    ...mapMutations(["setUser"]),
    submit() {
      if (this.$refs.form.validate()) {
        const user = {
          name: this.name,
          room: this.room
        };

        this.$socket.emit("userJoined", user, data => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            user.id = data.userId;
            this.setUser(user);
            this.$router.push("/chat");
          }
        });
      }
    }
  }
};
</script>

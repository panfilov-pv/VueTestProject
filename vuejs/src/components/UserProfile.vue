<template>
  <div class="user-profile">
      <div class="user-profile__user-panel">
          <h1 class="user-profile__username">@{{ user.username }}</h1>
          <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
          <div class="user-profile__admin-badge" v-else>Default user</div>
          <div class="user-profile__follower-count">
              <strong>Followers: </strong> {{ followers }}
          </div>
          <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
              <label for="newTwoot"><strong>New Twoot</strong></label>
              <textarea id="newTwoot" rows="4" v-model="newTwootContent"/>
              <div class="user-profile__create-twoot-type">
                  <label for="newTwootType"><strong>Type: </strong></label>
                  <select id="newTwootType" v-model="selectedTwootType">
                      <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                          {{ option.name }}
                      </option>
                  </select>
              </div>
              <button>
                  Twoot
              </button>
          </form>
      </div>
      <div class="user-profile__twoots-wrapper">
          <TwootItem v-for="twoot in user.twoots" :key="twoot.id" :username="user.username" :twoot="twoot" @favourite="toggleFavourite" />
      </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";

export default {
    name: "UserProfile",
    components: { TwootItem },
    data() {
        return {
            newTwootContent: '',
            selectedTwootType: 'instant',
            twootTypes: [
                { value: 'draft', name: 'Draft' },
                { value: 'instant', name: 'Instant Twoot' }
            ],
            followers: 0,
            user: {
                id: 1,
                username: "42ama",
                firstname: "Maxim",
                lastname: "Alonov",
                email: "42ama@gmail.com",
                isAdmin: true,
                twoots: [
                    { id: 1, content: 'Twotter is Amazing!'},
                    { id: 2, content: "Don't forget to subscribe!" },
                    { id: 3, content: "grs's legs is a very beautifull!"}
                ]
            }
        }
    },
    watch: {
        followers(newFollowerCount, oldFollowerCount) {
            if (oldFollowerCount < newFollowerCount) {
                console.log(`${this.user.username} has gained a follower!`);
            }
        }
    },
    methods: {
        followUser() {
            this.followers++;
        },
        toggleFavourite(id) {
            console.log(`Favourited Tweet #${id}`);
        },
        createNewTwoot() {
            if (this.newTwootContent && this.selectedTwootType !== 'draft') {
                this.user.twoots.unshift({
                    id: this.user.twoots.length + 1,
                    content: this.newTwootContent
                });
                this.newTwootContent = "";
            }
        }
    },
    computed: {
        fullName() {
            return `${this.user.firstname} ${this.user.lastname}`;
        }
    },
    mounted() {
        this.followUser();
    }
}
</script>

<style>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;
}
.user-profile__admin-badge {
    background: yellowgreen;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}

.user-profile__create-twoot {
    border-top: 1px solid black;
    padding-top: 20px;
    display: flex;
    flex-direction: column;
}

h1 {
    margin: 0;
}

</style>
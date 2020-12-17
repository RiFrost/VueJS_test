<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__user-badge" v-else>
                User
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded': newTwootCharCount > 180 }">
                <label for="newTwoot"><strong>New Twoot</strong>  ({{ newTwootCharCount }}/180)</label>
                <textarea name="" id="newTwoot" cols="30" rows="4" v-model="newTwootContent"></textarea>

                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type</strong></label>
                    <select name="" id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>

                <button>
                    Twoot!
                </button>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem 
                v-for="twoot in user.twoots" 
                :key="twoot.id" 
                :username="user.username" 
                :twoot="twoot" 
                @favourite="toggleFavourite" 
            />
        </div>
    </div>
</template>

<script>
import TwootItem from './TwootItem'

export default {
name: 'UserProfile',
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
        username: 'MusterMax',
        firstName: 'Max',
        lastName: 'Mustermann',
        email: 'max-mustermann@test.de',
        isAdmin: false,
        twoots: [
            { id: 1, content: 'Twotter is amazing' },
            { id: 2, content: 'Second twoot on this account. ' }
        ]
      }
    }
  },
watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if(oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a Follower`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    },
    newTwootCharCount() {
        return this.newTwootContent.length
    }
  },
  methods: {
    followUser() {
      this.followers++
    },
    toggleFavourite(id) {
        console.log(`Favourited Twoot #${id}`)
    },
    createNewTwoot() {
        if(this.newTwootContent && this.selectedTwootType !== 'draft') {
            this.user.twoots.unshift({
                id: this.user.twoots.length + 1,
                content: this.newTwootContent
            })

            this.newTwootContent = ''
        }
    }
  },
  mounted() {
    this.followUser()
  }
}
</script>

<style lang="scss" scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;

    .user-profile__user-panel {
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #DFE3E8;

        h1 {
            margin: 0;
        }   

        .user-profile__admin-badge {
            background: rebeccapurple;
            color: white;
            border-radius: 5px;
            margin-right: auto;
            padding: 0 10px;
            font-weight: bold;
        }

        .user-profile__create-twoot {
            padding-top: 20px;
            display: flex;
            flex-direction: column;

            &.--exceeded {
                color: red;
                border-color: red;
                -webkit-animation-name: shakeme;
                -webkit-animation-duration: 0.9s;
                -webkit-transform-origin:50% 50%;
                -webkit-animation-iteration-count: 1;
                -webkit-animation-timing-function: linear;
            }
        }
    }

    .user-profile__user-badge {
        background: green;
        color: white;
        border-radius: 5px;
        margin-right: auto;
        padding: 0 10px;
        font-weight: bold;
    }

    @-webkit-keyframes shakeme {
        0% { -webkit-transform: translate(2px, 1px) rotate(0deg); }
	10% { -webkit-transform: translate(-1px, -2px) rotate(-1deg); }
	20% { -webkit-transform: translate(-3px, 0px) rotate(1deg); }
	30% { -webkit-transform: translate(0px, 2px) rotate(0deg); }
	40% { -webkit-transform: translate(1px, -1px) rotate(1deg); }
	50% { -webkit-transform: translate(-1px, 2px) rotate(-1deg); }
	60% { -webkit-transform: translate(-3px, 1px) rotate(0deg); }
	70% { -webkit-transform: translate(2px, 1px) rotate(-1deg); }
	80% { -webkit-transform: translate(-1px, -1px) rotate(1deg); }
	90% { -webkit-transform: translate(2px, 2px) rotate(0deg); }
	100% { -webkit-transform: translate(1px, -2px) rotate(-1deg); }
}
}

</style>
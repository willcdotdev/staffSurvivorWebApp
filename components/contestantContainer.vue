<template>
  <div id="contestants">
    <div
      v-for="(contestant,cindex) of all"
      :id="contestant['Name'].split(' ')[0].toLowerCase()"
      :key="`${cindex}contestant`"
      :class="`contestant ${team[contestant['Name'].split(' ')[0].toLowerCase()]} ${eliminated.includes(contestant['Name'].split(' ')[0]) ? 'eliminated' : ''}`"
    >
      <img :src="`/images/${contestant['Name'].replace(/\s/gi,'')}.png`" alt="">
      <div class="biography">
        <h1>{{ contestant['Name'] }}</h1>
        <h3>Age</h3>
        <p>{{ contestant['Age'] }}</p>
        <h3>Hometown</h3>
        <p>{{ contestant['Hometown'] }}</p>
        <h3>Current Residence</h3>
        <p>{{ contestant['Current Residence'] }}</p>
        <h3>Occupation</h3>
        <p>{{ contestant['Occupation'] }}</p>
        <h3>Favorite Hobbies</h3>
        <p>{{ contestant['Favorite Hobbies'] }}</p>
        <h3>3 Words to Describe You</h3>
        <p>{{ contestant['3 Words to Describe You'] }}</p>
        <h3>Pet Peeve</h3>
        <p>{{ contestant['Pet Peeve'] }}</p>
        <h3>What is the accomplishment you are most proud of</h3>
        <p>{{ contestant['What is the accomplishment you are most proud of'] }}</p>
        <h3>What is something we would never know from looking at you</h3>
        <p>{{ contestant['What is something we would never know from looking at you'] }}</p>
        <h3>Who is your hero and why</h3>
        <p>{{ contestant['Who is your hero and why'] }}</p>
        <h3>Which past Survivor will you play the game most like</h3>
        <p>{{ contestant['Which past Survivor will you play the game most like'] }}</p>
        <h3>Why do you believe you can be the Sole Survivor</h3>
        <p>{{ contestant['Why do you believe you can be the Sole Survivor'] }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContestantContainer',
  props: {
    all: {
      type: Array,
      default () {
        return []
      }
    },
    team: {
      type: Object,
      default () {
        return {}
      }
    },
    eliminated: {
      type: Array,
      default () {
        return []
      }
    }
  }

}
</script>

<style scoped lang="scss">
#contestants {
  @apply w-full md:container mx-auto grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 ;
  .contestant {
    @apply relative border flex-col flex;
    .biography {
      @apply m-3 mr-0 mb-0 overflow-y-scroll max-h-[40vh];
    }

    .bioButton {
      @apply text-center py-5 border m-1 text-white
    }

    &.Taku ,&.Ika , &.Vati {
      @apply text-base
    }

    img {
      @apply w-full
    }

    h1 {
      @apply text-3xl my-5 border-b font-extrabold
    }

    h3 {
      @apply text-2xl font-bold;
    }

    p {
      @apply mb-2
    }

  }
}
</style>

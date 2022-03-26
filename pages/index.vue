<template>
  <div id="contestants">
    <div
      v-for="contestant of contestants"
      :id="contestant['Name'].split(' ')[0].toLowerCase()"
      :key="contestant['Name']"
      :class="`contestant ${contestant.team} ${contestant.eliminated ? 'eliminated' : ''}`"
      @click="toggleBio"
    >
      <div class="gradient" />
      <div class="staff">
        <span v-for="e of contestant.staff" :key="e">
          {{ e }}
        </span>
      </div>
      <img :src="contestant.img" :alt="contestant.Name">
      <div class="biography hidden">
        <h1>{{ contestant.Name }}</h1>
        <h3>Age</h3>
        <p>{{ contestant.Age }}</p>
        <h3>Hometown</h3>
        <p>{{ contestant.Hometown }}</p>
        <h3>Current Residence</h3>
        <p>{{ contestant['Current Residence'] }}</p>
        <h3>Occupation</h3>
        <p>{{ contestant.Occupation }}</p>
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

<script type="text/javascript">
export default {
  name: 'IndexPage',
  async asyncData ({ $content }) {
    const teams = await $content('teams').fetch()
    const staff = await $content('staff').fetch()
    const eliminated = await $content('eliminated').fetch()

    const contestants = [...await $content('bios').fetch()].map((contestant) => {
      const first = contestant.Name.split(' ')[0]
      const firstLower = first.toLowerCase()
      contestant.img = `/images/${contestant.Name.replace(/\s/gi, '')}.png`
      contestant.eliminated = eliminated.names.includes(first)
      contestant.team = teams.teams[firstLower]
      contestant.staff = []

      for (const member in staff.staff) {
        if (staff.staff[member].includes(first)) {
          contestant.staff.push(member)
        }
      }
      return contestant
    })
    return {
      contestants
    }
  },
  methods: {
    toggleBio (ev) {
      ev.currentTarget.classList.toggle('bio')
    }
  }

}
</script>

<style lang="scss" scoped>
.Taku {
  @apply border-orange text-orange;
  img, .staff span {
    @apply border-orange
  }

  .bioButton {
    @apply bg-orange
  }

  .gradient {
    @apply bg-gradient-to-t from-orange to-transparent;
  }

}

.Ika {
  @apply border-blue-500 text-blue-500;
  img, .staff span {
    @apply border-blue-500
  }

  .bioButton {
    @apply bg-blue-500
  }

  .gradient {
    @apply bg-gradient-to-t from-blue-500 to-transparent;
  }
}

.Vati {
  @apply border-green-500 text-green-500;
  img, .staff span {
    @apply border-green-500
  }

  .bioButton {
    @apply bg-green-500
  }

  .gradient {
    @apply bg-gradient-to-t from-green-500 to-transparent;
  }
}

.eliminated {
  @apply order-last opacity-50
}

.gradient {
  @apply w-full h-16 absolute bottom-0 left-0
}

#contestants {
  @apply w-full md:container mx-auto grid grid-cols-1 md:grid-cols-2 lg:h-screen lg:grid-cols-6 gap-4 p-4;
  .contestant {
    @apply relative border-2 flex-col flex overflow-hidden relative rounded-2xl;
    aspect-ratio: 4/5;

    &.bio {
      img, .staff,.gradient {
        @apply hidden
      }
      .biography {
        @apply flex flex-col
      }

    }

    .staff{
      @apply absolute bottom-0 left-0 z-10 flex w-full;
      span {
        @apply bg-white rounded-2xl m-1 px-3 py-1 border w-full text-center;
      }
    }

    .biography {
      @apply p-3 pb-0 overflow-y-scroll h-full absolute top-0 left-0;
      width:calc(100% + 17px);
      aspect-ratio: 4/5;
    }

    &.Taku ,&.Ika , &.Vati {
      @apply text-base
    }

    img {
      @apply h-full;
      aspect-ratio: 4/5;
    }

    h1 {
      @apply text-3xl mb-5 pb-2 border-b font-extrabold
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

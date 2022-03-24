<template>
  <main>
    <staff-container :staff="eliminatedPicks" :team="teams.teams" @chooseFilter="chooseFilter($event)" />
    <contestant-container :all="all" :team="teams.teams" :eliminated="eliminated.names" />
  </main>
</template>

<script type="text/javascript">
export default {
  name: 'IndexPage',
  async asyncData ({ $content }) {
    const teams = await $content('teams').fetch()
    const staff = await $content('staff').fetch()
    const contestants = await $content('bios').fetch()
    const eliminated = await $content('eliminated').fetch()

    return {
      staff,
      contestants,
      teams,
      eliminated

    }
  },
  data () {
    return {
      current: ''
    }
  },
  computed: {
    eliminatedPicks () {
      const obj = {}
      for (const name in this.staff.staff) {
        obj[name] = this.staff.staff[name].filter(v => !this.eliminated.names.includes(v))
      }
      return obj
    },
    all () {
      if (!this.current.length) {
        return this.contestants
      }
      return this.contestants.filter((contestant) => {
        return this.staff.staff[this.current].includes(contestant.Name.split(' ')[0])
      })
    }

  },
  methods: {
    chooseFilter (name) {
      if (name) {
        this.current = name
      } else {
        this.current = ''
      }
    }

  }

}
</script>

<style lang="scss">
main {
  @apply w-full flex flex-col p-2;

  .Taku {
    @apply border-orange text-orange;
    img {
      @apply border-orange
    }

    .bioButton {
      @apply bg-orange
    }

  }

  .Ika {
    @apply border-blue-500 text-blue-500;
    img {
      @apply border-blue-500
    }

    .bioButton {
      @apply bg-blue-500
    }
  }

  .Vati {
    @apply border-green-500 text-green-500;
    img {
      @apply border-green-500
    }

    .bioButton {
      @apply bg-green-500
    }
  }

  .eliminated {
    @apply order-last opacity-50 border-red-800 text-red-800
  }

}
</style>

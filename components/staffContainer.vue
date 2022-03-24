<template>
  <div id="staffComponent">
    <a id="staffHideButton" href="#" @click="toggleStaff">Toggle Staff List</a>
    <div id="staffContainer">
      <div id="staff">
        <div v-for="(bets, name) of staff" :key="`${name}`" class="staffMember">
          <a href="#" @click="chooseFilter(name)"><h2>{{ name }}</h2></a>
          <div class="staffPicks">
            <div v-for="contestantName of bets" :key="`${contestantName}${name}`">
              <p :class="`${team[contestantName.toLowerCase()]}`">
                {{ contestantName }}
              </p>
            </div>
          </div>
        </div>
      </div>
      <a id="all" href="#" @click="chooseFilter"><h2>All</h2></a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'StaffContainer',
  props: {
    staff: {
      type: Object,
      default () {
        return {}
      }
    },
    team: {
      type: Object,
      default () {
        return {}
      }
    }

  },
  methods: {
    chooseFilter (name) {
      this.$emit('chooseFilter', name)
    },
    toggleStaff () {
      document.body.querySelector('#staffContainer').classList.toggle('hidden')
    }
  }
}
</script>

<style scoped lang="scss">
#staffComponent {
  @apply w-full flex flex-col p-2;
  #staffContainer {
    @apply w-full m-0 p-0;
    &.hidden {
      @apply hidden ;
    }

    #all {
      @apply md:container w-full mx-auto text-center border border-gray-200 py-2 mb-4 rounded-bl-2xl rounded-br-2xl flex items-center justify-center;
    }

    #staff {
      @apply grid md:container w-full mx-auto grid-cols-1 md:grid-cols-3 lg:grid-cols-6;

      h2 {
        @apply text-3xl capitalize text-center border-b my-4 border-black pb-2;
      }

      .staffMember {
        @apply flex flex-col w-full border border-gray-200 p-3;
        &:first-child {
          @apply rounded-tl-2xl rounded-tr-2xl md:rounded-tr-none
        }

        &:last-child {
          @apply lg:rounded-tr-2xl
        }

        &:nth-child(3) {
          @apply md:rounded-tr-2xl lg:rounded-tr-none
        }

        .staffPicks {
          @apply w-full flex gap-1;
          div {
            @apply w-full;
            p {
              @apply text-center py-1 border w-full;
            }
          }
        }
      }
    }

  }

  #staffHideButton {
    @apply w-full text-center my-2 py-2
  }
}

</style>

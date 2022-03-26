<template>
  <div id="staffComponent" class="hidden">
    <div id="controls">
      <a id="staffHideButton" href="#" @click="toggleStaff">Toggle Staff List</a>
      <a id="all" href="#" @click="chooseFilter"><h2>All</h2></a>
    </div>
    <div id="staffContainer">
      <div id="staff">
        <div v-for="(bets, name) of staff" :key="`${name}`" class="staffMember">
          <a href="#" @click="chooseFilter(name)"><h2>{{ name }}</h2></a>
          <div class="staffPicks">
            <div v-for="contestantName of bets" :key="`${contestantName}${name}`">
              <a :href="`#${contestantName.toLowerCase()}`" :class="`${team[contestantName.toLowerCase()]}`" @click="jumpTo">
                {{ contestantName }}
              </a>
            </div>
          </div>
        </div>
      </div>
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
  data () {
    return {
      active: false
    }
  },
  mounted () {
    const observer = new IntersectionObserver((entry) => {
      console.log(entry[0].intersectionRect.top)
    }, { rootMargin: '-184px' })

    observer.observe(document.body.querySelector('.contestant:first-of-type'))
    // window.addEventListener('scroll', (ev) => {
    //   if (this.active) { return }
    //   this.active = true
    //   const component = document.body.querySelector('#staffComponent')
    //   const container = document.body.querySelector('#contestants')
    //   if (window.scrollY === 0) {
    //     component.classList.remove('scrolled')
    //     container.style.transform = 'translateY(0)'
    //   } else if (window.scrollY > 0 && !component.classList.contains('scrolled')) {
    //     component.classList.add('scrolled')
    //     container.style.transform = `translateY(${component.clientHeight + 16 + 'px'})`
    //   }
    //   setTimeout(() => {
    //     this.active = false
    //   }, 10)
    // })
  },
  methods: {
    chooseFilter (name) {
      this.$emit('chooseFilter', name)
    },
    toggleStaff (ev) {
      ev.preventDefault()
      const container = document.body.querySelector('#staffContainer')
      container.classList.toggle('hidden')
    },
    jumpTo () {
      const container = document.body.querySelector('#staffContainer')
      container.classList.toggle('hidden')
    }
  }
}
</script>

<style scoped lang="scss">
#staffComponent {
  @apply w-full flex flex-col pb-2  bg-white;
  &.scrolled {
    @apply fixed top-0 z-10 left-0 p-2;
  }
  #controls {
    @apply w-full mx-auto grid items-center justify-center grid-cols-2 text-center gap-2;
    a {
      @apply border bg-gray-100 p-4 hover:bg-gray-300
    }
  }
  #staffContainer {
    @apply w-full m-0 p-0;
    &.hidden {
      @apply hidden;
    }

    #staff {
      @apply grid md:container w-full mx-auto grid-cols-1 md:grid-cols-3 lg:grid-cols-6 md:gap-10 md:gap-2 ;

      h2 {
        @apply text-3xl capitalize text-center pb-2;
      }

      .staffMember {
        @apply flex flex-col w-full md:my-4;

        .staffPicks {
          @apply w-full flex gap-1;
          div {
            @apply w-full flex;
            a {
              @apply text-center py-1 border w-full hover:text-white;

              &.Taku {
                @apply hover:bg-orange
              }
              &.Ika {
                @apply hover:bg-blue-500
              }
              &.Vati {
                @apply hover:bg-green-500
              }
            }
          }
        }
      }
    }

  }

}

</style>

<template>
  <div>
    <div class="fixed top-14 z-50 lg:sticky lg:top-12">
      <button
        class="rounded-r-full bg-white text-black lg:hidden"
        @click="toggle"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          stroke-width="2"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M9 5l7 7-7 7"
          />
        </svg>
      </button>
      <nav
        :aria-disabled="toggleOpen"
        :style="{ transform: toggleOpen ? 'translateX(0)' : '' }"
        class="dpc-sidebar lg:backdrop-blur-3x fixed inset-0 max-h-screen translate-x-[-100vw] overflow-y-scroll scroll-smooth rounded border-stone-50/30 bg-zinc-900 transition-transform duration-100 md:max-h-[calc(100vh-6rem)] lg:relative lg:inset-auto lg:mx-auto lg:max-w-none lg:translate-x-0 lg:border lg:bg-[rgba(0,0,0,0.5)]"
      >
        <button
          class="mt-4 rounded-r-full bg-white text-black lg:hidden"
          @click="toggle"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M15 19l-7-7 7-7"
            />
          </svg>
        </button>
        <section class="grid justify-items-center gap-8 p-8">
          <div
            class="grid grid-cols-[1fr_2fr] items-center gap-8 lg:grid-cols-1"
          >
            <img
              src="/assets/avatar.png"
              alt="avator"
              class="h-auto rounded-full"
              width="220"
              height="220"
            />
            <header class="grid justify-items-center gap-2">
              <h2 class="font-title text-xl font-bold">Temples DAO</h2>
              <p class="flex items-center gap-2 font-body text-xs">
                $TEMPL on
                <img src="/assets/ETH.svg" class="h-[1.8em]" alt="Ethereum" />
              </p>
            </header>
          </div>
          <div
            class="grid grid-cols-[1fr_2fr] items-center gap-8 lg:grid-cols-1"
          >
            <a href="/members">{{ members || 'NaN' }} members</a>
          </div>
          <aside
            class="border-accent-800 grid w-full grid-cols-[1fr_auto] justify-between rounded border font-body text-sm"
          >
            <h3 class="border-accent-800 border-b p-2 opacity-50">
              Club Allocation
            </h3>
            <p class="border-accent-800 border-b p-2 opacity-50">TBD</p>
            <h3 class="p-2 opacity-50">APY for $DEV</h3>
            <p class="p-2 opacity-50">&*%</p>
          </aside>
          <HSButton type="filled fullwidth" link="/join">Join</HSButton>
          <div class="grid w-full gap-3">
            <HSButton link="/perks">Perks</HSButton>
            <HSButton type="disabled" link="#">Quests</HSButton>
            <HSButton type="disabled" link="#">Updates</HSButton>
            <HSButton type="disabled" link="#">Vote</HSButton>
          </div>
        </section>
      </nav>
    </div>
  </div>
</template>

<script>
import { providers } from 'ethers'
import { detectStokensByPropertyAddress } from '../../fixtures/dev-kit'
import HSButton from '../Hashi/HSButton.vue'

export default {
  name: 'Sidebar',
  components: { HSButton },
  data() {
    return {
      members: 0,
      toggleOpen: false,
    }
  },
  async created() {
    const providerURL = import.meta.env.PUBLIC_WEB3_PROVIDER_URL
    const provider = new providers.JsonRpcProvider(providerURL)
    const propertyAddress = import.meta.env.PUBLIC_PROPERTY_ADDRESS
    await detectStokensByPropertyAddress(provider, propertyAddress).then(
      (res) => {
        this.members = res.length
      }
    )
  },
  methods: {
    toggle() {
      this.toggleOpen = !this.toggleOpen
      document.body.classList.toggle('overflow-hidden')
    },
  },
}
</script>
<style lang="scss" scoped>
.dpc-sidebar {
  scrollbar-width: thin;
  scrollbar-color: rgba(255 255 255 / 50%) rgba(0 0 0 / 30%);

  &::-webkit-scrollbar {
    width: 7px;
    background-color: rgba(0 0 0 / 30%);
  }

  &::-webkit-scrollbar-thumb {
    background-color: rgba(255 255 255 / 50%);
  }
}
</style>

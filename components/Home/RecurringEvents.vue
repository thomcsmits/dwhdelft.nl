<i18n lang="yaml">
en:
  announcement: <strong>DWH</strong> is open {count} nights a week
nl:
  announcement: <strong>DWH</strong> is elke week op {count} avonden open
</i18n>

<template>
  <div class="container px-4 mx-auto py-8 md:py-16">
    <h2
      class="md:text-center text-brand-500 font-medium text-5xl md:mb-6 leading-tight"
      v-html="$t('announcement', { count: openingHours.length })"
    />

    <table class="lg:w-2/3 mx-auto border-separate border-spacing-y-16">
      <tr v-for="event in openingHours" :key="event.name">
        <td class="align-top pt-1 block md:table-cell mb-4 md:mb-0">
          <div class="text-gray-500 text-2xl uppercase font-semibold" v-text="$tt(event.day)" />
          <div class="text-gray-400 text-xl" v-text="event.start_time" />
          <div v-if="event.restrictions" class="space-x-2 pt-2">
            <div v-for="restriction in event.restrictions" :key="restriction" class="text-center flex items-center">
              <div
                class="bg-brand-200 rounded-lg px-2 py-1 text-xs uppercase tracking-wider"
                v-text="$tt(restriction)"
              />
            </div>
          </div>
        </td>
        <td class="align-top block md:table-cell">
          <h2 class="mb-2 text-brand-500 font-semibold text-3xl" v-text="event.name" />
          <p class="text-gray-500" v-html="$tt(event.description)" />
          <p v-if="event.announcement" class="text-brand-500 font-semibold" v-html="$tt(event.announcement)" />
          <a
            v-if="event.link"
            :href="event.link.url"
            class="mt-6 inline-block button-pink px-5 py-2 text-sm font-semibold"
            v-html="$tt(event.link.name)"
          />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      openingHours: [],
    }
  },
  async fetch() {
    this.openingHours = (await this.$content('opening_hours').fetch()).events
  },
}
</script>

<template lang="pug">
div
  // TODO: Add some way to disable this notification, probably by making the ratio threshold configurable
  b-alert.my-2(v-if="isVisible", variant="info", show)
    p.mb-0
      | #[b High uncategorized time]
      br
      | You have a total of {{ uncategorizedDuration[0] | friendlyduration }} uncategorized time,
      | that's {{ Math.round(100 * uncategorizedDuration[0] / uncategorizedDuration[1]) }}% of all time today.
      | You can address this by using the #[router-link(:to="{ path: '/settings/category-builder' }") Category Builder].
</template>

<script>
import { mapState } from 'pinia';
import { useActivityStore } from '~/stores/activity';

export default {
  name: 'aw-uncategorized-notification',
  computed: {
    ...mapState(useActivityStore, ['uncategorizedDuration']),
    ratio() {
      console.log(this.uncategorizedDuration);
      return this.uncategorizedDuration
        ? this.uncategorizedDuration[0] / this.uncategorizedDuration[1]
        : 0;
    },
    isVisible() {
      // if ratio is > 0.3, show it   // TODO: make configurable
      // if there's a category filter (url has category query param), don't show it
      return this.ratio > 0.3 && !this.$route.query.category;
    },
  },
};
</script>

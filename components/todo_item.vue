<template>
  <li>
    <input id="" type="checkbox" name="" :checked="done" />
    {{ name }}
    終了予定:
    <span>{{ toDate.getFullYear() }}年</span>
    <span>{{ toDate.getMonth() + 1 }}月</span>
    <span>{{ toDate.getDate() }}日</span>
  </li>
</template>

<script>
export default {
  props: ['name', 'done', 'scheduled_at'],
  computed: {
    toDate() {
      const timestamp = new this.$fireModule.firestore.Timestamp(
        this.scheduled_at.seconds,
        this.scheduled_at.nanoseconds
      )

      return timestamp.toDate()
    },
  },
}
</script>

<style scoped></style>

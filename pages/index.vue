<template>
  <div class="container">
    <h1>Result: {{ records }}</h1>
    <h2>Todo:</h2>
    <ul v-for="todo in records" :key="todo.name">
      <li>
        <div>
          <input id="" type="checkbox" name="" :checked="todo.done" />
          {{ todo.name }}
          終了予定:
          <span>{{ toDate(todo.scheduled_at).getFullYear() }}年</span>
          <span>{{ toDate(todo.scheduled_at).getMonth() + 1 }}月</span>
          <span>{{ toDate(todo.scheduled_at).getDate() }}日</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // dummy data for v-for loop
      records: [
        {
          id: 0,
          name: 'none',
          done: false,
          scheduled_at: {
            seconds: 0,
            nanoseconds: 0,
          },
        },
      ],
    }
  },
  mounted() {
    this.readFromFirestore()
  },
  methods: {
    toDate(timestampObj) {
      const timestamp = new this.$fireModule.firestore.Timestamp(
        timestampObj.seconds,
        timestampObj.nanoseconds
      )

      return timestamp.toDate()
    },
    async readFromFirestore() {
      try {
        const todoRef = this.$fire.firestore.collection('todos')
        const snapshot = await todoRef.get()
        const writingData = []

        snapshot.forEach((doc) => {
          writingData[writingData.length] = doc.data()
        })

        this.records = writingData
      } catch (e) {
        console.log(e)
      }
    },
  },
}
</script>

<style></style>

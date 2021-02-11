<template>
  <div class="container">
    <h1>Result: {{ records }}</h1>
    <h2>Todo:</h2>
    <ul v-for="todo in records" :key="todo.name">
      <li>
        <todo-item
          :name="todo.name"
          :done="todo.done"
          :scheduled_at="todo.scheduled_at"
        ></todo-item>
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

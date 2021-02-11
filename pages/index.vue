<template>
  <div class="container">
    <h1>Result: {{ records }}</h1>
    <div>
      <Logo />
      <h1 class="title">firebase_demo</h1>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          rel="noopener noreferrer"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      records: 'Records is empty...',
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

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>

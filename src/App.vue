<template>
  <Header />
  <main>
    <div class="main-content">
      <div class="container">
        <div class="title">
          Timefaker
        </div>
        <div class="description">
          A tool for generate random timestamp with specified time range.
        </div>
        <div class="description-2">
          (Example: Generate the timestamp between 2024/08/06 - 2024/08/07, every 10:00 - 18:00.)
        </div>
        <GenerateForm />
      </div>
    </div>
  </main>
</template>

<script>
import Header from '@/components/Header.vue';
import GenerateForm from '@/components/GenerateForm.vue';

export default {
  name: 'App',
  components: {
    Header,
    GenerateForm,
  },
}
</script>

<style lang="scss">
.title {
  font-size: 3em;
  font-weight: bold;
  text-align: center;
  margin-top: 50px;
  margin-bottom: 10px;
}

.description {
  font-size: 2em;
  text-align: center;
  margin-bottom: 5px;
}

.description-2 {
  margin-bottom: 30px;
  text-align: center;
}
</style>

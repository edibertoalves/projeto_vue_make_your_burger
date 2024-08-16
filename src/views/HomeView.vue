<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Monte o seu burger</h1>
      <BurgerForm />
    </div>

  </div>
</template>

<script>
import Banner from '../components/banner.vue'
import BurgerForm from '../components/BurgerForm.vue'

export default {
  name: 'HomeView',
  components: {
    Banner,
    BurgerForm
  }
}

</script>

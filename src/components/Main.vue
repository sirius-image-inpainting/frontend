<template>
  <div class='main'>
    <Header/>
    <GAN/>
    <Footer/>
  </div>
</template>

<script>
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import GAN from '@/components/GAN.vue'

export default {
  name: 'Main',
  components: {
    Header,
    Footer,
    GAN,
  }
}
</script>

<style lang='scss' scoped>
.main {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
</style>

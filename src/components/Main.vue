<template>
  <div class='main'>
    <Header/>
    <Painter/>
    <Footer/>
  </div>
</template>

<script>
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import Painter from '@/components/Painter.vue'

export default {
  name: 'Main',
  components: {
    Header,
    Footer,
    Painter
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

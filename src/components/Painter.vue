<template>
  <div class='painter'>
    <div class='image'>

    </div>
    <div class='tools'>
        <div class='button-box'>
            <div :class='{"button": true, "selected": selected_button == 0}'
                 @click="select_button(0)">
                🖌
            </div>

            <div :class='{"button": true, "selected": selected_button == 1}'
                 @click="select_button(1)">
                e
            </div>

            <div :class='{"button": true, "selected": selected_button == 2}'
                 @click="select_button(2)">
                r
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
        selected_button: 0,
    }
  },
  name: 'Painter',
  methods: {
    select_button(id) {
      this.selected_button = id
    }
  },
}
</script>

<style lang='scss' scoped>

.painter {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;

    .image {
        width: 400px;
        height: 400px;
        margin: 0 30px;

        border-radius: 50px;
        box-shadow:  20px 20px 60px #c8c8c8,
             -20px -20px 60px #ffffff;
    }

    .tools {
        width: 400px;
        height: 400px;

        .button-box {
            display: flex;
            flex-direction: row;

            .button {
                width: 50px;
                height: 50px;
                font-size: 1rem;
                display: flex;
                margin: 0 15px;
                justify-content: center;
                align-items: center;
                border-radius: 150px;
                box-shadow:  8px 8px 16px #acacac,
                            -8px -8px 16px #ffffff;
                user-select: none;
            }

            .selected {
                box-shadow: inset 8px 8px 16px #acacac,
                            inset -8px -8px 16px #ffffff;
            }
        }
    }
}

</style>

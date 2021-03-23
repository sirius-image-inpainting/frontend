<template>
  <div class='gan'>
    <div class='label' id='input-label'>Input</div>
    <div class='image' id='input-image'></div>
    <div class='button' id='random-image'>Random</div>

    <div class='nn' id='nn1'>
        <div class='layer' id='layer1' style='height: 256px;' />
        <div class='layer' id='layer1' style='height: 190px;' />
        <div class='layer' id='layer1' style='height: 140px;' />
        <div class='layer' id='layer1' style='height: 100px;' />
        <div class='layer' id='layer1' style='height: 140px;' />
        <div class='layer' id='layer1' style='height: 190px;' />
        <div class='layer' id='layer1' style='height: 256px;' />
    </div>

    <div class='label' id='output-label'>Result</div>
    <div class='image' id='output-image'></div>
    <div class='button' id='compute'>Compute</div>
  </div>
</template>

<script>
export default {
  name: 'GAN',
  components: {
  }
}
</script>

<style lang='scss' scoped>

.gan {
    display: grid;
    justify-content: center;
    row-gap: 20px;

    grid-template:
        [row1-start] "input-label   .  output-label" [row1-end]
        [row2-start] "input-image   nn output-image" [row2-end]
        [row3-start] "choose-random .  compute     " [row3-end]
        / 256px 440px 256px;

    .image {
        justify-self: center;
        width: 256px;
        height: 256px;

        border-radius: 0px;
        background-color: #d1d1d1;
        /* box-shadow: inset 20px 20px 60px #bebebe, */
            /* inset -20px -20px 60px #ffffff; */
    }

    .label {
        justify-self: center;
        font-family: sans-serif;
        font-size: 1.2rem;
        color: $TEXTCOLOR;
    }

    .button {
        justify-self: center;
        font-size: 1rem;
        font-family: sans-serif;
        color: $TEXTCOLOR;
        user-select: none;

        margin-top: 20px;
        padding: 10px 16px;
        border-radius: 50px;
        background: #e0e0e0;
        box-shadow:  6px 6px 11px #bebebe,
                    -6px -6px 11px #ffffff;

        &:active {
            border-radius: 50px;
            left: -20px;
            background: #e0e0e0;
            box-shadow: inset 6px 6px 11px #bebebe,
                        inset -6px -6px 11px #ffffff;
        }
    }

    .nn {
        justify-self: center;
        align-self: center;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;

        .layer {
            width: 30px;
            height: 128px;
            border-radius: 20px;
            margin: 0 12px;

            border-radius: 50px;
            background-color: #d1d1d1;
        }
    }

    #nn1 {
        grid-area: nn;
    }

    #input-image {
        grid-area: input-image;
    }

    #input-label {
        grid-area: input-label;
    }

    #output-label {
        grid-area: output-label;
    }

    #output-image {
        grid-area: output-image;
    }

    #random-image {
        grid-area: choose-random;
    }

    #compute {
        grid-area: compute;
    }
}

</style>

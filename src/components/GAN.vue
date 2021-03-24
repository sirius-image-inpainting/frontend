<template>
  <div class='gan'>
    <div class='label' id='input-label'>Input</div>
    <Editor :canvasWidth="canvas_width"
            :canvasHeight="canvas_height"
            ref='editor'
            id='input-image'
            class='image' @click="load_file"/>
    <div class='button' id='random-image' @click="load_random_image">Random</div>

    <div class='nn' id='nn1'>
        <div class='layer' id='layer1' style='height: 256px;' />
        <div class='layer' id='layer1' style='height: 256px;' />
        <div class='layer' id='layer1' style='height: 190px;' />
        <div class='layer' id='layer1' style='height: 190px;' />
        <div class='layer' id='layer1' style='height: 140px;' />
        <div class='layer' id='layer1' style='height: 140px;' />
        <div class='layer' id='layer1' style='height: 100px;' />
        <div class='layer' id='layer1' style='height: 100px;' />
        <div class='layer' id='layer1' style='height: 140px;' />
        <div class='layer' id='layer1' style='height: 140px;' />
        <div class='layer' id='layer1' style='height: 190px;' />
        <div class='layer' id='layer1' style='height: 190px;' />
        <div class='layer' id='layer1' style='height: 256px;' />
        <div class='layer' id='layer1' style='height: 256px;' />
    </div>

    <div class='label' id='output-label'>Result</div>
    <div class='image' id='output-image'></div>
    <div class='button' id='compute' @click='run_model'>Compute</div>

    <div class='tools' id='tools-panel'>
        <div class='choose-gan' id='choose-gan'>
            GAN:
            <div class='tool-button choose-gan-button'>#1</div>
            <div class='tool-button choose-gan-button'>#2</div>
            <div class='tool-button choose-gan-button'>#3</div>
        </div>

        <div class='toolset' id='toolset-1'>
            <div :class='{"brush": true, "tool-button": true, "active": active_tool == 0}'
                @click="set_active_tool('draw')" />
            <input type="range" min="5" max="30" v-model="brush_width" class='slider' @change="set_brush_conf">
            <div :class='{"eraser": true, "tool-button": true, "active": active_tool == 1}'
                @click="set_active_tool('eraser')" />
            <input type="range" min="1" max="100" v-model="eraser_width" class='slider' @change="set_eraser_conf">
        </div>

    </div>
  </div>
</template>

<script>
import Editor from 'vue-image-markup'

export default {
    name: 'GAN',
    components: {
        Editor,
    },
    data: () => {
        return {
            canvas_width: '256',
            canvas_height: '256',

            /* tool panel */
            active_tool: 0,
            brush_width: 10,
            eraser_width: 10,
        }
    },
    methods: {
        load_file() {
            console.log('load_image')
        },
        load_random_image() {
            this.$refs.editor.setBackgroundImage('place.jpg')
        },
        run_model() {
            console.log('run_model')
        },
        set_active_tool(tool_name) {
            if (tool_name == 'draw') {
                this.active_tool = 0
            }
            if (tool_name == 'eraser') {
                this.active_tool = 1
            }
        },
        set_brush_conf() {
            this.$refs.editor.set('freeDrawing', {strokeWidth: this.brush_width})
        },
        set_eraser_conf() {
            this.$refs.editor.set('freeDrawing', {strokeWidth: this.eraser_width})
        }
    },
    mounted() {
       this.$refs.editor.set('freeDrawing')
    }
}
</script>

<style lang='scss' scoped>

.gan {
    display: grid;
    justify-content: center;
    row-gap: 20px;

    grid-template:
        [row1-start] "input-label   .     output-label" [row1-end]
        [row2-start] "input-image   nn    output-image" [row2-end]
        [row3-start] "choose-random tools compute     " [row3-end]
        / 270px 470px 270px;

    .image {
        justify-self: center;
        width: 256px;
        height: 256px;
        border: 10px solid #eee;
        /* box-sizing: border-box; */

        border-radius: 5px;
        background-color: #fff;
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

        width: auto;
        height: 30px;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;

        /* margin-top: 20px; */
        padding: 5px 16px;
        border-radius: 50px;
        background: #d1d1d1;
        box-shadow:  6px 6px 12px #b2b2b2,
                    -6px -6px 12px #f0f0f0;

        &:active {
            border-radius: 50px;
            left: -20px;
            background: #e0e0e0;
            box-shadow: inset 6px 6px 11px #bebebe,
                        inset -6px -6px 11px #ffffff;
        }
    }

    .tools {
        justify-self: center;
        align-self: start;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;

        .choose-gan {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            width: 100%;
            color: $TEXTCOLOR;
            font-family: sans-serif;

            .choose-gan-button {
                border-radius: 5px;
                margin: 0 10px;
            }
        }

        .toolset {
            display: grid;
            grid-template-columns: 40px 300px;
            row-gap: 10px;
            column-gap: 10px;
            margin-top: 20px;
        }

        input[type='range'] {
            align-self: center;
            appearance: none;
            user-select: none;
            outline: none;

            height: 8px;
            border-radius: 10px;
            background-color: #bbb;

            &::-webkit-slider-thumb {
                appearance: none;
                width: 12px;
                height: 16px;
                border-radius: 50%;
                background-color: #333;
            }
        }

        .tool-button {
            justify-self: center;
            font-size: 1rem;
            font-family: sans-serif;
            color: $TEXTCOLOR;
            user-select: none;
            display: flex;
            justify-content: center;
            align-items: center;

            width: 40px;
            height: 40px;

            border-radius: 50px;
            background: #d1d1d1;
            box-shadow:  6px 6px 12px #b2b2b2,
                        -6px -6px 12px #f0f0f0;
        }

        .active {
            box-shadow: inset 6px 6px 12px #b2b2b2,
                        inset -6px -6px 12px #f0f0f0;
        }

        .brush {
            background-image: url('~@/assets/pencil.png');
            background-size: 15px;
            background-repeat: no-repeat;
            background-position: center;
        }

        .eraser {
            background-image: url('~@/assets/eraser.png');
            background-size: 15px;
            background-repeat: no-repeat;
            background-position: center;
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
            width: 20px;
            border-radius: 20px;
            margin: 0 5px;

            border-radius: 50px;
            background-color: #d1d1d1;
            box-shadow:  4px 4px 9px #b2b2b2,
                         -4px -4px 9px #f0f0f0;

            &:hover {
                background-color: rgba(0, 0, 0, 0.1);
            }
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

    #tools-panel {
        grid-area: tools;
    }
}

</style>

<template>
  <div class='gan'>
    <div class='label' id='input-label'>Input</div>
    <Editor :canvasWidth="canvas_width"
            :canvasHeight="canvas_height"
            ref='editor'
            id='input-image'
            class='image'/>

    <div id='load-buttons' class='load-buttons'>
        <!-- <div class='button' id='load-image' @click="load_image_from_file">Load</div> -->
        <div class='button' id='random-image' @click="load_random_image">Random</div>
    </div>

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
        <span>GAN:</span>
        <div :class='{"tool-button": true, "choose-gan-button":true, "gan_active": cgan==1}'
            @click="set_gan(1)">#1</div>
        <div :class='{"tool-button": true, "choose-gan-button":true, "gan_active": cgan==2}'
            @click="set_gan(2)">#2</div>
        <div :class='{"tool-button": true, "choose-gan-button":true, "gan_active": cgan==3}'
            @click="set_gan(3)">#3</div>
        <span>Tools:</span>
        <div :class='{"box": true, "tool-button": true, "active": active_tool == 0}'
            @click="set_active_tool(0)" />
        <div :class='{"brush": true, "tool-button": true, "active": active_tool == 1}'
            @click="set_active_tool(1)" v-if="brush_visible"/>
        <div :class='{"eraser": true, "tool-button": true, "active": active_tool == 2}'
            @click="set_active_tool(2)" />
    </div>
  </div>
</template>

<script>
import Editor from '@/components/Editor/Editor.vue'
import axios from 'axios'

export default {
    name: 'GAN',
    components: {
        Editor,
    },
    data: () => {
        return {
            canvas_width: '320',
            canvas_height: '320',

            /* tool panel */
            active_tool: 0,
            cgan: 1,
            brush_visible: false,

            /* server */
            server_host: 'http://127.0.0.1:1337',
        }
    },
    methods: {
        set_active_tool(tool_id) {
            this.active_tool = tool_id
            if (tool_id == 0) {
                this.$refs.editor.set('rect', {
                    fill: '#00ff00',
                    stroke: '#00ff00',
                    strokeWidth: 0,
                })
            }
            if (tool_id == 1) {
                this.$refs.editor.set('freeDrawing', {
                    'stroke': '#00ff00',
                })
            }
            if (tool_id == 2) {
                this.$refs.editor.set('eraser')
            }
        },
        set_gan(gan_id) {
            if (gan_id > 1) {
                this.brush_visible = true
            } else {
                this.brush_visible = false
            }
            this.$refs.editor.clear()
            this.cgan = gan_id
        },
        image2base64(url) {
            var xhr = new XMLHttpRequest()
            /* xhr.responseType = 'blob' */
            xhr.open('GET', url, false)
            xhr.send()
            print(xhr.response)

            var reader = new FileReader()
            return reader.readAsDataURL(xhr.response)
        },
        load_random_image() {
            this.$refs.editor.setBackgroundImage('/random_image')
        },
        load_image_from_file() {
            // TODO
            console.log('load_image')
        },
        run_model() {
            console.log('run_model')
            let bg_image = this.$refs.editor.getBackgroundImage()
            let mask = this.$refs.editor.saveImage()
        },
    },
    mounted() {
        this.set_active_tool(this.active_tool)
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
        / 340px 470px 340px;

    .image {
        justify-self: center;
        width: 320px;
        height: 320px;
        border: 12px solid #eee;

        border-radius: 5px;
        background-color: #fff;
    }

    .load-buttons {
        justify-self: center;

        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: flex-start;

        #random-image {
            width: 90px;
        }

        #load-image {
            width: 90px;
            margin-right: 20px;
        }
    }

    .label {
        justify-self: center;
        font-family: sans-serif;
        font-size: 1.5rem;
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

        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        justify-content: center;
        align-items: center;
        width: 50%;
        row-gap: 30px;

        color: $TEXTCOLOR;
        font-family: sans-serif;
        user-select: none;

        .toolset {
            display: grid;
            grid-template-columns: 40px 300px;
            row-gap: 10px;
            column-gap: 10px;
            margin-top: 20px;
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

        .choose-gan-button {
            border-radius: 5px;
            margin: 0 10px;
        }

        .gan_active {
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

        .box {
            background-image: url('~@/assets/square.png');
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

    #load-buttons {
        grid-area: choose-random;
    }

    #compute {
        grid-area: compute;
        width: 90px;
    }

    #tools-panel {
        grid-area: tools;
    }
}

</style>

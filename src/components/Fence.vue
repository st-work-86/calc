<template>
    <div class="wrap">
        <div v-if="selector" class="fence-selector">
            <div class="close-button" v-on:click="closeFenceSelector()"></div>
            <ul class="row">
                <li v-for="(item, index) in fenceData.fence" :key="index" class="col-xs-6" v-on:click="selectFence(index)">
                    <img v-bind:src="getImgUrl(item.src)" alt="" />
                </li>
            </ul>
        </div>
        <div class="fence-bg">
            <div class="fence-group">
                <div v-if="options.peak" :key="0" class="fence-peak"></div>
                <div class="fence-pillar">
                    <div class="fence-controls-left">
                        <ul>
                            <li v-for="(item, index) in fenceSections" :key="index" class="fence-controls-item">
                                <span class="arrow-left" v-on:click="prevSection(index)"></span>
                                <span class="text">Поменять</span>
                                <span class="arrow-right" v-on:click="nextSection(index)"></span>
                            </li>
                            <li v-if="options.base" :key="5" class="fence-base-placeholder"></li>                        
                        </ul>
                    </div>                    
                </div>
                <div class="fence-pillar">
                    <div class="fence-sections">
                        <ul>
                            <li v-for="(item, index) in fenceSections" :key="index" class="fence-section-selector" v-on:click="openFenceSelector(index)">
                                <img v-bind:src="getImgUrl(item.src)" alt="" />
                            </li>
                            <li v-if="options.base" :key="5" class="fence-base"></li>
                        </ul>              
                    </div>                    
                </div>                
                <div class="fence-pillar">
                    <div class="fence-sections">
                        <ul>
                            <li v-for="(item, index) in fenceSections" :key="index" class="fence-section-selector" v-on:click="openFenceSelector(index)">
                                <img v-bind:src="getImgUrl(item.src)" alt="" />
                            </li>
                            <li v-if="options.base" :key="5" class="fence-base"></li>
                        </ul>              
                    </div>                    
                </div>
            </div>
            <div class="grass"></div>
        </div>
    </div>
</template>

<script>
import { EventBus } from './../main';

export default {
    props: ['fenceData'],
    data() {
        return {
            options: {
                'sections': 3,
                'peak': false,
                'base': false,
                'length': 0
            },
            fence: {
                peak: this.fenceData.peak,
                base: this.fenceData.base,
                sections: [0, 0, 0, 0, 0]
            },
            pointer: 0,
            selector: false,
            priceTable: false         
        }
    },
    methods: {
        getImgUrl(pic) {
            return require('./../assets/'+pic)
        },
        fenceRender() {
            this.options.sections++;
            this.options.sections--;            
        },
        prevSection(index) {
            if(this.fence.sections[index] > 0) {
                this.fence.sections[index]--;
            } else {
                this.fence.sections[index] = this.fenceData.fence.length-1;
            }
            this.fenceUpdated();
        },
        nextSection(index) {
            if(this.fence.sections[index] < this.fenceData.fence.length-1) {
                this.fence.sections[index]++;
            } else {
                this.fence.sections[index] = 0;
            }
            this.fenceUpdated();
        },
        openFenceSelector(i) {
            this.pointer = i;
            this.selector = true;
        },
        closeFenceSelector() {
            this.selector = false;
        },
        selectFence(section_index) {
            this.fence.sections[this.pointer] = section_index;
            this.selector = false;
            this.fenceUpdated();           
        },
        fenceUpdated() {
            this.fenceRender();
            const result = [];
             for (var i=0;i<this.options.sections;i++) {
                result.push(this.fenceData.fence[this.fence.sections[i]]);
            }           
            EventBus.$emit('fenceUpdated', {"options": this.options, "fence": result, "peak": this.fenceData.peak, "base": this.fenceData.base, "pillar": this.fenceData.pillar});
        }
    },
    computed: {
        fenceSections() {
            const result = [];
            for (var i=0;i<this.options.sections;i++) {
                result.push(this.fenceData.fence[this.fence.sections[i]]);
            }
            return result;
        }        
    },
    created() {
        EventBus.$on('ControlsUpdated', (data) => {
            this.options.sections = data.sections;
            this.options.peak = data.peak;
            this.options.base = data.base;
            this.options.length = data.length;
            this.fenceUpdated();
        })
    },
    mounted() {
        this.fenceUpdated();
    }
}
</script>

<style lang="scss" scoped>
    .wrap {
        margin-top: 169px;
        width: 100%;
        height: 201px;
        background-image: url("./../assets/fence-bg.png");
        background-repeat: no-repeat;        
    }
    .fence-bg {
        position: relative;
        width: 100%;
        height: 100%;
        .grass {
            position: absolute;
            bottom: 0;
            left: 0;
            height: 31px;
            width: 100%;
            background-image: url("./../assets/grass.png");
        }
    }
    .fence-group {
        position: absolute;
        width: 100%;
        height: auto;
        left: 191px;
        bottom: 0;
    }
    .fence-controls-left {
        width: 100px;
        margin-right: 45px;
        .fence-controls-item {
            display: block;
            background: rgba(255, 255, 255, 0.8);
            height: 50px;
        }
        ul {
            width: 100%;
            li.fence-controls-item {
                display: block;
                position: relative;
                overflow: visible;
                .arrow-left, .arrow-right {
                    display: block;
                    position: absolute;
                    top: 50%;
                    margin-top: -15px;
                    width: 30px;
                    height: 30px;
                    background-image: url('./../assets/interface/arrow.png');
                    background-color: rgba(0, 0, 0, 0.5);
                    background-position: 7px 6px;
                    background-repeat: no-repeat;
                    z-index: 1;
                    -webkit-border-radius: 50%;
                    border-radius: 50%;
                    cursor: pointer;
                }
                .arrow-left {
                    left: -37px;
                }
                .arrow-right {
                    right: -37px;
                    -moz-transform: rotate(180deg);
                    -webkit-transform: rotate(180deg);
                    -o-transform: rotate(180deg);
                    -ms-transform: rotate(180deg);
                    transform: rotate(180deg);                    
                }
                .text {
                    display: block;
                    width: 100%;
                    height: 100%;
                    line-height: 50px;
                    text-align: center;
                }
            }
        }
    }
    .fence-sections {
        ul {
            li {
                height: 50px;
            }
        }
        .fence-section-selector {
            display: block;
            position: relative;
            &:hover {
                &:before {
                    opacity: 0.5;
                }
            }
            &:before {
                content: " ";
                display: block;
                position: absolute;
                left: 0;
                top: 0;
                background: #ccc;
                border: 1px #666 solid;
                box-sizing: border-box;
                z-index: 100;
                width: 100%;
                height: 100%;
                -webkit-transition: all 0.3s ease;
                -moz-transition: all 0.3s ease;
                -ms-transition: all 0.3s ease;
                -o-transition: all 0.3s ease;
                transition: all 0.3s ease;
                opacity: 0;
                cursor: pointer;          
            }
        }
    }
    .fence-pillar {
        display: block;
        float: left;
        background: url("./../assets/pillar.png") repeat-y top right;
        padding: 0 18px 0 0px;
    }
    .fence-peak {
        position: absolute;
        height: 19px;
        width: 453px;
        background: url("./../assets/peak.png") no-repeat;
        margin: 0 0 0 145px;
        left: 0;
        top: -14px;
    }
    .fence-base {
        height: 19px!important;
        background: #a5a5a4;
    }
    .fence-base-placeholder {
        height: 19px;
        background: #fff;
    }

    ul {
        position: relative;
        width: 200px;
        margin: 0;
        padding: 0;
    }

    li {
        display: block;
        width: 100%;
        overflow: hidden;
    }

    .fence-selector {
        display: block;
        position: absolute;
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
        background: #fff;
        z-index: 300;
        .close-button {
            width: 50px;
            height: 50px;
            z-index: 310;
            position: absolute;
            background: #ccc url("./../assets/interface/close.png");
            right: 10px;
            top: 10px;
            -webkit-transition: all 0.3s ease;
            -moz-transition: all 0.3s ease;
            -ms-transition: all 0.3s ease;
            -o-transition: all 0.3s ease;
            transition: all 0.3s ease;
            cursor: pointer;
            &:hover {
                background-color: #666;
            }       
        }
        ul {
            margin: 0;
            padding: 100px 230px;
            width: 100%;
            height: 100%;
            li {
                width: 50%;
                height: 100px;
                text-align: center;
                border: 2px transparent solid;
                line-height: 95px;
                cursor: pointer;
                &:hover {
                    border: 2px #999 solid;
                }
            }
        }
    }
</style>
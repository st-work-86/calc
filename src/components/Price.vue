<template>
    <div class="price-button-wrapper">
        <div class="calculate-price-button" v-on:click="showTable = true">Рассчитать</div>
        <div v-if="showTable" class="price-table">
            <table class="fence-results-table">
                <tr>
                    <th colspan="2">Наименование</th>
                    <th>Количество</th>
                    <th>Вес</th>
                    <th>Цена</th>
                    <th>Сумма</th>
                </tr>
               <tr v-if="options.peak">
                    <td>КОНЕК</td>
                    <td>{{ peak.name }}</td>
                    <td>{{ getSectionQuantity() }}</td>
                    <td>{{ peak.weight }}</td>
                    <td>{{ peak.price }}</td>
                    <td>{{ getSectionSum(peak.price) }}</td>
                </tr>                
                <tr v-for="(item, index) in this.fence" :key="index">
                    <td>{{ index+1 }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ getSectionQuantity() }}</td>
                    <td>{{ item.weight }}</td>
                    <td>{{ item.price }}</td>
                    <td>{{ getSectionSum(item.price) }}</td>
                </tr>
               <tr v-if="options.base">
                    <td>ЦОКОЛЬ</td>
                    <td>{{ base.name }}</td>
                    <td>{{ getSectionQuantity() }}</td>
                    <td>{{ base.weight }}</td>
                    <td>{{ base.price }}</td>
                    <td>{{ getSectionSum(base.price) }}</td>
                </tr>
               <tr>
                    <td>СТОЛБ</td>
                    <td>{{ pillar.name }}</td>
                    <td>{{ getPillarQuantity() }}</td>
                    <td>{{ pillar.weight }}</td>
                    <td>{{ pillar.price }}</td>
                    <td>{{ getPillarSum(pillar.price) }}</td>
                </tr>                             
                <tr>
                    <td colspan="6" class="total">
                        <div class="total-col">
                            <div class="total-row">общая стоимость:</div>
                            <div class="total-row">общий вес:</div>
                        </div>
                        <div class="total-col">
                            <div class="total-row num"><span>{{ getTotalPrice() }}</span> грн</div>
                            <div class="total-row num"><span>{{ getTotalWeight() }}</span> кг</div>                            
                        </div>
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
import { EventBus } from './../main';

export default {
    data() {
        return {
            showTable: false,
            options: {},
            fence: {},
            peak: {},
            base: {},
            pillar: {}
        }
    },
    methods: {
        getTotalWeight() {
            let result = 0;
            result += this.getPillarWeight(this.pillar.weight);
            for(const item of this.fence) {
                result += this.getSectionWeight(item.weight);
            }         
            if(this.options.peak) {
                result += this.getSectionWeight(this.peak.weight);
            }
            if(this.options.base) {
                result += this.getSectionWeight(this.base.weight);
            }            
            return result;
        },
        getTotalPrice() {
            let result = 0;
            result += this.getPillarSum(this.pillar.price);
            for(const item of this.fence) {
                result += this.getSectionSum(item.price);
            }  
            if(this.options.peak) {
                result += this.getSectionSum(this.peak.price);
            }
            if(this.options.base) {
                result += this.getSectionSum(this.base.price);
            }
            return result;
        },
        getSectionQuantity() {
            return Math.ceil(this.options.length/2);
        },
        getPillarQuantity() {
            return Math.ceil(this.options.length/2)+1;
        },
        getSectionSum(price) {
            return Math.ceil((price*this.getSectionQuantity())*100)/100;
        },
        getPillarSum(price) {
            return Math.ceil((price*(this.getSectionQuantity()+1)*100))/100;
        },
        getSectionWeight(weight) {
            return weight*this.getSectionQuantity();
        },
        getPillarWeight(weight) {
            return weight*this.getPillarQuantity();
        }
    },
    created() {
        EventBus.$on("fenceUpdated", (data) => {
            this.options = data.options,
            this.fence = data.fence,
            this.peak = data.peak,
            this.base = data.base,
            this.pillar = data.pillar
        })
    }
}
</script>

<style lang="scss" scoped>
    .price-button-wrapper {
        display: block;
        text-align: center;
        padding: 30px;
        .calculate-price-button {
            display: inline-block;
            width: 200px;
            height: 50px;
            line-height: 50px;
            background: #a10000;
            -webkit-border-radius: 10px;
            border-radius: 10px;
            -webkit-transition: all 0.3s ease;
            -moz-transition: all 0.3s ease;
            -ms-transition: all 0.3s ease;
            -o-transition: all 0.3s ease;
            transition: all 0.3s ease;
            cursor: pointer;
            font-weight: bold;
            text-transform: uppercase;
            color: #fff;
            margin-bottom: 1em;
            &:hover {
                background-color: #750000;
            }
        }
    }
    .fence-results-table {
        border-spacing: 2px;
        border-collapse: separate;
        width: 762px;
        margin: 0 auto;
        tr {
            th {
                padding: 10px;
                background: #cecece;
                text-align: center;
                font-weight: bold;
                text-transform: uppercase;
                color: #4e4e4e;
                font-size: 16px;
            }
            td {
                padding: 10px;
                background: #e9e9e8;
                color: #4e4e4e;
                font-size: 16px;
                text-align: center; 
                &.total {
                    background: #fff;
                    text-align: right;
                    .total-col {
                        display: inline-block;
                        text-align: right;
                        margin-right: 10px;
                        .total-row {
                            color: #4e4e4e;
                            font-size: 14px;
                            text-transform: uppercase;
                            height: 28px;
                            line-height: 28px;
                            &.num {
                                text-align: left;
                                text-transform: none;
                                span {
                                    font-weight: bold;
                                    font-size: 21px;
                                }
                            }
                        }
                    }
                }               
            }
        }
    }
</style>
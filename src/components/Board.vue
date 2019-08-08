<template>
    <div class="game-board">
        <div v-for="(row, index1) of matrix" :key="index1">
            <Button @btnClick="btnClick" v-for="(item, index2) of row" :icon="checkItem(item, [index1, index2])" :key="index2" :index="[index1, index2]"></Button>
        </div>
    </div>
</template>
<script>
    import Button from './Button';
    import config from "../config";

    export default {
        data () {
            return {
                emptyIndex: []
            }
        },
        props: {
            matrix: {
                type: Array,
                default: []
            }
        },
        components: {
            Button
        },
        methods: {
            btnClick (val) {
                const index1 = Math.abs(this.emptyIndex[0] - val[0]);
                const index2 = Math.abs(this.emptyIndex[1] - val[1]);

                if((index1 === 0 && index2 === 1) || (index1 === 1 && index2 === 0)) {
                    let newMatrix = this.matrix.slice(0);
                    newMatrix[this.emptyIndex[0]][this.emptyIndex[1]] = newMatrix[val[0]][val[1]];
                    newMatrix[val[0]][val[1]] = 0;
                    this.$emit('changed', newMatrix)
                }
            },
            checkItem (val, index) {
                if(val === 0) {
                    this.emptyIndex = index;
                    return config.iconEmpty;
                }

                return val;
            }
        }
    }
</script>
<style lang="scss">
    .game-board {}
</style>

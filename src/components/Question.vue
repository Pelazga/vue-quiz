<template>
        <div class="panel panel-default">
        <div class="panel-heading">
            <h3 class="panel-title text-center">{{ firstOperand }} {{ randomOperation }} {{ secondOperand }} = ?</h3>
        </div>
        <div class="panel-body">
            <div class="col-xs-12 col-sm-6 text-center"  v-for="variant in variants" :key="variant">
                <button class="btn btn-primary btn-lg" style="margin: 10px" @click="checkVariant(variant)">{{ variant }}</button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'app',
        data () {
            return {
                firstOperand: 0,
                secondOperand: 0,
                result: 'result',
                randomOperation: '+',
                variants: []
            }
        },
    methods: {
        calcResult (){
            this.firstOperand = this.getRandom(1, 100);
            this.secondOperand= this.getRandom(1, 100);

            this.result = this.firstOperand + this.secondOperand;

            for (let i = 0; i < 4; i++) {
                this.getWrongAnswers(this.result-10, this.result+10, i);
                // console.log(this.variants);
                
            };

            this.variants[this.getRandom(0, 3)] = this.result;

        },
        checkVariant(variant){               
            if (variant === this.result){
                this.$emit("answered", true);
                this.variants = [];
                this.calcResult();
            } else {
                this.$emit("answered", false)
            }
        },
        getWrongAnswers(min, max, i){
            this.variants[i] = this.getRandom(min, max);
            if (this.variants[i] == this.result ){
                this.getWrongAnswers(min, max, i);
                
            } else {
                for (let g = 0; g < this.variants.length-1; g++) {
                    if (this.variants[i] == this.variants[g]){
                        this.getWrongAnswers(min, max, i)
                    }
                }
            }
        },
        getRandom (min, max){
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }
    },
    created() {
        this.calcResult();
    }

}
</script>
<style>

</style>
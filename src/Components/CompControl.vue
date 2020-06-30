<template>
    <div class="list-control">
        <button class="control btn-new" v-on:click="startNewGame"><i class="ion-ios-plus-outline"></i>Game mới</button>
        <button class="control btn-roll" v-on:click="rollDice"><i class="ion-ios-loop"></i>Xoay xúc xắc</button>
        <button class="control btn-hold" v-on:click="holdScore"><i class="ion-ios-download-outline"></i>Lấy điểm</button>
        <input type="number" min="0" placeholder="Final score" v-bind:value="finalScore" class="final-score" v-bind:class="disableInput" ref="finalScore"/>
    </div>
</template>
<script>
export default {
    name : "comp-control",
    props : {
        finalScore : {
            type    : Number,
            default : 0
        }
    },
    data() {
        return {
        }
    },
    methods: {
        startNewGame(e){
            let finalScore = parseInt(this.$refs.finalScore.value);
            if(finalScore != NaN && finalScore > 0){
                let data = {
                    finalScore : finalScore
                }
                this.$emit('changeFinalScoreEvent',data);
                this.$emit('starNewGameEvent');
            } else {
                alert('Vui lòng chọn điểm final');
            }
        },
        rollDice(e){
            this.$emit('rollDiceEvent');
        },
        holdScore(e) {
            this.$emit('holdScoreEvent');
        }
    },
    computed: {
        disableInput(){
            return {
                disable : this.finalScore > 0
            }
        },
        
    },
}
</script>
<style lang="">
    
/**********************************************
*** Control
**********************************************/
.control {
    position: absolute;
    width: 200px;
    left: 50%;
    transform: translateX(-50%);
    color: #9a9898;
    background: none;
    border: none;
    font-family: cursive;
    font-size: 16px;
    text-transform: uppercase;
    cursor: pointer;
    font-weight: 300;
    transition: background-color 0.3s, color 0.3s;
}
.control.disable {
    pointer-events: none;
}

.control:hover { font-weight: 600; }
.control:hover i { margin-right: 20px; }

.control:focus {
    outline: none;
}

.control i {
    color: #42b983;
    display: inline-block;
    margin-right: 15px;
    font-size: 32px;
    line-height: 1;
    vertical-align: text-top;
    margin-top: -4px;
    transition: margin 0.3s;
}

.btn-new { top: 45px;}
.btn-roll { top: 403px;}
.btn-hold { top: 467px;}

.final-score {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    top: 520px;
    color: #555;
    font-size: 18px;
    font-family: 'Lato';
    text-align: center;
    padding: 10px;
    width: 160px;
    text-transform: uppercase;
}

.final-score:focus { outline: none; }
.final-score.disable {
    pointer-events: none;
}
</style>
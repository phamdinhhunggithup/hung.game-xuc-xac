<template>
    <div class="wp-game wrapper">
        <comp-player
            v-bind:listLayer="listLayer"
            v-bind:layerActive="layerActive"
        />
        <comp-control
            v-on:starNewGameEvent="handleStartNewGame"
            v-bind:finalScore="finalScore"
            v-on:changeFinalScoreEvent="handleChangeFinalScore"
            v-on:rollDiceEvent="handleRollDice"
            v-on:holdScoreEvent="handleHoleScore"
        />
        <comp-dice
            v-bind:listDice="listDice"
        />
        <comp-popupModal
            v-if="openModal"
            v-bind:modalName="modalName"
            v-on:getInfoLayerEvent="handleGetInfoLayer"
            v-on:getInfoRuleEvent="handleInfoRule"
            v-bind:winner="winner"
            v-on:resetGameEvent="resetGame"
        />
    </div>
</template>
<script>
import CompPlayer from './CompPlayer.vue';
import CompControl from './CompControl.vue';
import CompDice from './CompDice.vue';
import CompPopupModal from './CompPopupModal.vue';
export default {
    name : "comp-game",
    data() {
        return {
            listLayer  : [
                { id : 1 , name : "Người chơi 1" , gender : "" , score : 0 ,currentScore : 0 },
                { id : 2 , name : "Người chơi 2" , gender : "" , score : 0 ,currentScore : 0 },
            ],
            listDice   : [
                { id : 1, numDice : 1 },
                { id : 2, numDice : 1 },
            ],
            finalScore   : 0,
            currentScore : 0,
            agreeRule    : false,
            openModal    : false,
            modalName    : "",
            title        : "name dice hello word",
            actionGame   : "start",
            layerActive  : -1,
            winner       : "",
        }
    },
    methods: {
        handleRollDice() {
            if( this.actionGame == "playing" ){
                let numDice1 = Math.floor(1 + Math.random() * 6);
                let numDice2 = Math.floor(1 + Math.random() * 6);
                this.listDice[0].numDice = numDice1;
                this.listDice[1].numDice = numDice2;
                this.checkRule();
            } else {
                alert('Bạn chưa nhấn bắt đầu');
            }
        },
        checkRule() {
            let numDice1 = this.listDice[0].numDice;
            let numDice2 = this.listDice[1].numDice;
            if ( numDice1 == 1 || numDice2 == 1 ) {
                this.resetLayer();
                this.nextLayer();
            } else if ( numDice1 != 1 && numDice2 != 1 ) {
                this.plusCurrScore();
            }
        },
        plusCurrScore() {
            let currentScore = this.listDice[0].numDice + this.listDice[1].numDice;
            this.listLayer[this.layerActive].currentScore += currentScore;
            this.currentScore = this.listLayer[this.layerActive].currentScore;
        },
        resetLayer() {
            this.listLayer[this.layerActive].score = 0;
            this.listLayer[this.layerActive].currentScore = 0;
            this.currentScore = 0;
        },
        nextLayer() {
            let lastLayer = this.listLayer.length - 1;
            if ( this.layerActive >= lastLayer ) {
                this.layerActive = 0;
            } else {
                this.layerActive ++;
            }
            this.currentScore = 0;
        },
        notification(msg){
            setTimeout( function() {
                alert(msg);
            },500 );
        },
        resetGame() {
            this.listLayer.forEach( (l, idx) => {
                l.name = 'Layer ' + (idx + 1);
                l.gender = "";
                l.score = 0;
                l.currentScore = 0;
            } );
            this.finalScore = 0;
            this.actionGame = "start";
            this.openModal = false;
            this.layerActive  = -1;
        },
        handleStartNewGame(data){
            if( this.actionGame === "start" ) {
                if ( this.finalScore > 0 ) {
                    this.openModal  = true;
                    this.modalName  = "info";
                }
            } else if ( this.actionGame === "playing" ) {
                let selectAgain = confirm('Nhấn ok để hủy game và chơi lại từ đầu');
                if (selectAgain) {
                    this.resetGame();
                }
            } else {// action = finish
                this.resetGame();
            }
        },
        handleGetInfoLayer(data) {
            this.modalName = "rule";
            this.listLayer[0].name   = data.nameA.length > 0 ? data.nameA : "Layer 1";
            this.listLayer[0].gender = data.genderA;
            this.listLayer[1].name   = data.nameB.length > 0 ? data.nameB : "Layer 2";
            this.listLayer[1].gender = data.genderB;
            this.layerActive         = 0;
        },
        handleInfoRule(data){
            if(data.agree) {
                this.agreeRule  = true;
                this.openModal  = false;
                this.actionGame = "playing";
            }
        },
        handleChangeFinalScore(data) {
            this.finalScore = data.finalScore;
        },
        handleHoleScore(){
            if ( this.actionGame == "playing" ) {
                if( this.currentScore > 0 ) {
                    console.log(this.layerActive);
                    console.log(this.listLayer[this.layerActive]);
                    this.listLayer[this.layerActive].score += this.currentScore;
                    this.listLayer[this.layerActive].currentScore = 0;
                    this.checkWinner();
                    this.nextLayer();
                } else {
                    alert('Bạn chưa xoay xúc xắc');
                }
            } else {
                alert('Bạn chưa nhấn bắt đầu');
            }
        },
        checkWinner(){
            if ( this.listLayer[this.layerActive].score >= this.finalScore ){
                console.log('winner');
                this.modalName = "winner";
                this.openModal = true;
                this.winner    = this.listLayer[this.layerActive].name;
            }
        }
    },
    computed: {
        
    },
    components : {
        CompPlayer,
        CompControl,
        CompDice,
        CompPopupModal
    }
}
</script>
<style>
/**********************************************
*** GENERAL
**********************************************/
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(../assets/back.jpg);
    background-size: cover;
    background-position: center;
    font-family: cursive;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #7b7b7b;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}

</style>
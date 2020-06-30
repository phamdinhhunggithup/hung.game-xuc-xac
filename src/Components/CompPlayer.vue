<template>
    <div class="list-player clearfix">
        <div class="player-panel" v-for="(item, index) in listLayer" v-bind:key="index"  v-bind:class="{active : index == layerActive}" v-bind:data-id-layer="item.id">
            <div class="player-name">
                <span>{{ item.name }}</span>
                <span class="gender-layer" v-html="getGender(item.gender)"></span>
            </div>
            <div class="player-score">{{ item.score }}</div>
            <div class="player-current-box">
                <div class="player-current-label">Current</div>
                <div class="player-current-score">{{ item.currentScore }}</div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name : "comp-player",
    props : {
        listLayer : Array,
        layerActive : Number,
    },
    data() {
        return {
            title : "List player hello word",
            nameLayerA : "",
            nameLayerB : ""
        }
    },
    methods: {
        getGender(gender) {
            if(gender == 'male') {
                return `--(👱)--`;
            } else if (gender == 'female') {
                return `--(👩)--`;
            } else {
                return `--(Giới tính)--`;
            }
        },
    },
}
</script>
<style lang="">

/**********************************************
*** PLAYERS
**********************************************/
.player-panel {
    width: 50%;
    float: left;
    height: 600px;
    padding: 100px;
    transition: all .3s ease;
    background-color: #fff;
}
.player-panel .deadline {
    text-align: center;
    font-size: .8rem;
}
.player-name {
    font-family: monospace;
    font-size: 30px;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: 100;
    margin-top: 20px;
    margin-bottom: 10px;
    position: relative;
}
.player-name .gender-layer {
    font-size: .7rem;
    display: block;
}
.player-score {
    text-align: center;
    font-size: 80px;
    font-weight: 100;
    color: #42b983;
    margin-bottom: 130px;
}

.active { background-color: #f7f7f7; }
.active .player-name { font-weight: 300; }

.active .player-name::after {
    content: "\2022";
    font-size: 47px;
    position: absolute;
    color: #42b983;
    top: -7px;
    right: 10px;
    
}

.player-current-box {
    background-color: #42b983;
    color: #fff;
    width: 40%;
    margin: 0 auto;
    padding: 12px;
    text-align: center;
}

.player-current-label {
    text-transform: uppercase;
    margin-bottom: 10px;
    font-size: 12px;
    color: #222;
}

.player-current-score {
    font-size: 30px;
}

.winner { background-color: #f7f7f7; }
.winner .player-name { font-weight: 300; color: #42b983; }

</style>
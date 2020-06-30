<template>
    <div class="rule-game">
        <div class="popup-modal rule" v-if="modalName=='rule'">
            <div class="modal-header">
                <h4>Luật chơi<span class="sub-title">(Tuân theo luật để trận đấu công bằng nhé)</span></h4>
            </div>
            <div class="modal-body">
                <p class="desc">
                    <span style="display: block; font-size: .85rem; margin-bottom: 3px;">(+) Mỗi lược chỉ được phép một người xoay xúc xắc</span>
                    <span style="display: block; font-size: .85rem; margin-bottom: 3px;">(+) Mỗi người sẽ có số lược xoay là tự do</span>
                    <span style="display: block; font-size: .85rem; margin-bottom: 3px;">(+) Nếu trong mỗi lược xoay, có quân xúc xắc nào bằng 1 thì điểm sẽ bị reset lại</span>
                    <span style="display: block; font-size: .85rem; margin-bottom: 3px;">(+) Nếu điểm tổng của người chơi lớn hơn hoặc bằng điểm final thì người đó là người chiến thắng</span>
                </p>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn-allow" v-on:click="handleAgreeRule">Đồng ý</button>
            </div>
        </div>
        <div class="popup-modal info" v-if="modalName=='info'">
            <div class="modal-header">
                <h4>Thông tin người chơi<span class="sub-title">(Không bắt buộc)</span></h4>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label for="layer1">Thông tin bên A</label>
                    <input type="text" ref="nameA" name="layer1" id="layer1" class="layer1" value="" placeholder="Tên người chơi" autocomplete="off" spellcheck="false">
                    <select ref="genderA">
                        <option value="">Giới tính</option>
                        <option value="male">Nam</option>
                        <option value="female">Nữ</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="layer2">Thông tin bên B</label>
                    <input type="text" ref="nameB" name="layer2" id="layer2" class="layer2" value="" placeholder="Tên người chơi" autocomplete="off" spellcheck="false">
                    <select ref="genderB">
                        <option value="">Giới tính</option>
                        <option value="male">Nam</option>
                        <option value="female">Nữ</option>
                    </select>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn-allow" v-on:click="getInfoLayer">Tiếp theo</button>
            </div>
        </div>
        <div class="popup-modal winner" v-if="modalName=='winner'">
            <div class="modal-header">
                <h4>
                    <span class="winner">{{ winner }}</span>
                    <span>( WINNER )</span>
                </h4>
            </div>
            <div class="modal-body">
                <div class="thumb-img">
                    <img src="../assets/winner.gif"></img>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn-allow" v-on:click="resetGame">Chơi lại</button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name : "comp-popupModal",
    props : {
        modalName : String,
        winner : {
            type : String,
            default : "Layer"
        }
    },
    data() {
        return {
            
        }
    },
    methods: {
        getInfoLayer(e){
            let data = {
                nameA   : this.$refs.nameA.value,
                genderA : this.$refs.genderA.value,
                nameB   : this.$refs.nameB.value,
                genderB : this.$refs.genderB.value
            }
            let numInfo = 0;
            for(var info in data) {
                if(data[info] != ""){
                    numInfo ++;
                }
            }
            // if( numInfo >= 4 ){
            
            this.$emit('getInfoLayerEvent',data);
            // } else {
            //     alert('Vui lòng điền đầy đủ thông tin người chơi');
            // }
        },
        resetGame() {
            this.$emit('resetGameEvent');
        },
        handleAgreeRule(e){
            let data = {
                agree : true
            }
            this.$emit('getInfoRuleEvent',data);
        }
    },
}
</script>
<style lang="">
    .rule-game {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background-color: rgba(0,0,0,0.3);
    }
    .popup-modal {
        display: inline-block;
        position: absolute;
        width: 400px;
        background-color: #fff;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.61);
        padding: 10px 20px;
        left: 20%;
        top: 20%;
        position: relative;
    }
    .popup-modal.winner {
        width: 500px;
        left: 16%;
        top: 10%;
    }

    .popup-modal .modal-header {
        font-family: cursive;
        font-size: 1.3rem;
        font-weight: bold;
        color: #767676;
        text-align: center;
    }
    .popup-modal .modal-header span.sub-title {
        font-size: .7rem;
        color: #777070;
        font-weight: 300;
        display: block;
    }
    .popup-modal .modal-header h4 {
        position: relative;
    }
    .popup-modal .modal-header h4 span.winner {
        color: #42b983;
        display: block;
        font-size: 2.5rem;
        margin-bottom: 10px;
        animation-name: showWinner;
        animation-duration: .25s;
        animation-iteration-count: infinite;
        animation-timing-function: ease;
        animation-direction: alternate;
        animation-play-state: running;
    }
    @keyframes showWinner {
        from {
            transform: rotate(4deg) scale(1.2);
        } to {
            transform: rotate(-4deg) scale(1);
        }
    }
    .popup-modal .modal-header h4::after {
        content: "";
        position: absolute;
        top: 49px;
        left: 174px;
        width: 8px;
        height: 8px;
        border-radius: 100%;
        background-color: #8b8b8b;
    }
    .popup-modal.winner .modal-header h4::after {
        display: none;
    }
    .popup-modal .modal-header::after, 
    .popup-modal .modal-header::before {
        content: "";
        position: absolute;
        height: 2px;
        width: 125px;
        background-color: #9f9f9f;
        top: 62px;
    }
    .popup-modal.winner .modal-header::after,
    .popup-modal.winner .modal-header::before {
        display: none;
    }

    .popup-modal .modal-header::after {
        left: 60px;
    }
    .popup-modal .modal-header::before {
        right: 65px;
    }

    .popup-modal .modal-body {
        line-height: 1.4;
        text-align: justify;
        padding: 20px 0;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .popup-modal.info .modal-body {
    }
    .popup-modal .modal-body .form-group {
        padding: 15px 0;
    }
    .popup-modal .modal-body label {
        display: block;
        font-size: .8rem;
        cursor: pointer;
    }
    .popup-modal .modal-body input {
        padding: 2px 5px;
        box-sizing: border-box;
        border: none;
        border-bottom: 1px solid #7b7b7b;
        outline: none;
        
    }
    .popup-modal .modal-body select {
        border: none;
        border-bottom: 1px solid #7b7b7b;
        outline: none;
        padding: 2px 5px;
        color: #7b7b7b;
    }
    .popup-modal .modal-footer {
        text-align: center;
    }
    .popup-modal .modal-footer button {
        border: none;
        padding: 7px 21px;
        box-shadow: 0 0 4px rgba(0,0,0,0.5);
        border-radius: 5px;
        background-color: #fcfcfc;
        cursor: pointer;
        outline: none;
        font-family: cursive;
        color: #989898;
        font-weight: bold;
        position: relative;
    }
</style>
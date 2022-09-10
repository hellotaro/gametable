<template>
<div>
    <div v-if="pokerGameInfo != null">
        <div>PlayerNum: {{pokerGameInfo.PlayerNum}}</div>
        <div>GamePhase: {{pokerGameInfo.Phase}}</div>
        <div>GameFee: {{pokerGameInfo.Fee}}</div>

        <div class="frame-area">
            <div v-if="pokerGameInfo.Phase == 0">
                <h4>プレイヤー数を入力してください。</h4>
                <input v-model="playerNum" />人
                <button class="action-button" v-on:click="postPokerActionNext">決定({{playerNum}}人)</button>
            </div>
            <div v-if="pokerGameInfo.Phase == 1">
                ゲームを作成しました。
                <button class="action-button" v-on:click="postPokerActionNext">次へ</button>
            </div>
            <div v-if="pokerGameInfo.Phase == 2">
                <div v-for="log in getLastPlayerHistory(4)">
                    <div v-if="log.type == 'raise'">{{log.player}} has raised.</div>
                    <div v-if="log.type == 'check'">{{log.player}} has checked.</div>
                    <div v-if="log.type == 'hold'">{{log.player}} has held.</div>
                    <div v-if="log.type == 'all_in'">{{log.player}} has all in.</div>
                </div>
                アクションを選択してください。(１ターン目)
            </div>
            <div v-if="pokerGameInfo.Phase == 5">
                <div v-for="log in getLastPlayerHistory(7)">
                    <div v-if="log.type == 'raise'">{{log.player}} has raised.</div>
                    <div v-if="log.type == 'check'">{{log.player}} has checked.</div>
                    <div v-if="log.type == 'hold'">{{log.player}} has held.</div>
                    <div v-if="log.type == 'all_in'">{{log.player}} has all in.</div>
                </div>
                アクションを選択してください。(２ターン目)
            </div>
            <div v-if="pokerGameInfo.Phase == 8">
                <div v-for="log in getLastPlayerHistory(10)">
                    <div v-if="log.type == 'raise'">{{log.player}} has raised.</div>
                    <div v-if="log.type == 'check'">{{log.player}} has checked.</div>
                    <div v-if="log.type == 'hold'">{{log.player}} has held.</div>
                    <div v-if="log.type == 'all_in'">{{log.player}} has all in.</div>
                </div>
                アクションを選択してください。(３ターン目)
            </div>
            <div v-if="pokerGameInfo.Phase == 11">
                掛け金を配分しました。
                <button class="action-button" v-on:click="postPokerActionNext">次へ</button>
            </div>
        </div>
    </div>

    <div v-if="pokerGameInfo != null && pokerGameInfo.Phase >= 2">
        <GameSetTable v-if="table" :table="table" />
        <button class="action-button" v-on:click="switchHintFlg">ヒント</button>
        <div v-if="hintFlg">
            <h4>Table Chance({{table != null && table.Cards != null ? tableNextCards.length + "/" + (13 * 4 - 2- table.Cards.length) : "-"}})</h4>
            <div class="flex justify-center">
                <GameSetTrumpCard v-for="(card, cidx) in tableNextCards" :key="'card-' + cidx" :card="card" />
            </div>
            <h4>Chance Cards</h4>
            <div class="flex justify-center">
                <GameSetTrumpCard v-for="(card, cidx) in chanceCards" :key="'card-' + cidx" :card="card" />
            </div>
        </div>
        <GameSetPlayer v-for="(player, pidx) in players" :is_me="player.Name==playerName" :key="player.Name" :player="player" :actionHandler="postPlayerAction" />
    </div>
</div>
</template>

<style lang="postcss" scoped>
.action-button {
    @apply inline-block rounded bg-blue-600 px-6 py-2.5 text-xs font-medium uppercase leading-tight text-white shadow-md transition duration-150 ease-in-out;
    &:hover {
        @apply bg-blue-700;
    }
}
.frame-area {
    border: 1px solid #FAA;
}
</style>

<script>
import axios from "axios"

export default {
    name: "PokerGame",
    data() {
        return {
            pokerGameInfo: null,
            table: null,
            players: [],
            playerName: "Alice",
            playerNum: "4",
            predMyHandMatrix: null,
            predTableHandMatrix: null,
            chanceCards: [],
            tableNextCards: [],
            hintFlg: false,
        }
    },
    mounted() {
        this.setPokerInfo().then((res) => {});
    },
    methods: {
        postPokerActionNext() {
            let meta = {}
            meta = {
                ActionType: "action",
                Value: "check",
            }
            const phase = this.pokerGameInfo.Phase;
            if(phase == "0") {
                meta = {
                    ActionType: "setPlayerNum",
                    Value: this.playerNum,
                }
            }

            this.postPokerAction(meta).then((res) => {
                this.setPokerInfo().then((res) => {})
            })
        },
        postPlayerAction(action) {
            this.postPokerAction(action).then((res) => {
                this.setPokerInfo().then((res) => {})
            })
        },
        encodeCardPredStr(predStr) {
            predStr = predStr.substring(2,predStr.length-2)
            const predAry = predStr.split("} {")
            for(let idx=0;idx<predAry.length;idx++) {
                predAry[idx] = predAry[idx].substring(2).split("}] ")
                predAry[idx][0] = predAry[idx][0].split(" ")
            }
            const predMatrix = []
            for(let idx=0;idx<4;idx++) {
                const row = []
                for(let nidx=0;nidx<13;nidx++) {
                    row.push(-1)
                }
                predMatrix.push(row)
            }
            for(let idx=0;idx<predAry.length;idx++) {
                const r = Number(predAry[idx][0][0])
                const c = Number(predAry[idx][0][1])
                predMatrix[r][c] = Number(predAry[idx][1])
            }
            return predMatrix
        },
        getLastPlayerHistory(phase) {
            const gameHistory = this.pokerGameInfo.GameHistory;
            const players = this.pokerGameInfo.Players;
            const logs = [];
            for(let idx=0;idx<gameHistory.length;idx++) {
                if(gameHistory[idx].Phase == phase) {
                    logs.push({
                        type: gameHistory[idx].ActionType,
                        player: players[gameHistory[idx].PlayerID].Name,
                    })
                }
            }
            return logs;
        },
        switchHintFlg() {
            this.hintFlg = !this.hintFlg;
        },
        async setPokerInfo() {
            const pokerInfo = await this.getPokerInfo()
            console.log(pokerInfo)
            const pokerGameInfo = pokerInfo.data
            this.pokerGameInfo = pokerGameInfo
            this.table = pokerGameInfo.Table
            if(pokerGameInfo.Players != null) {
                this.players = pokerGameInfo.Players
            }

            const pairScore = Math.pow(15, 3);
            const threeCardsScore = Math.pow(15, 4);
            const straightScore = Math.pow(15, 5);
            if(pokerGameInfo.NextMyHandPred) {
                this.predMyHandMatrix = this.encodeCardPredStr(pokerGameInfo.NextMyHandPred)
                let chanceCards = []
                if(this.players.length > 0) {
                    for(let sidx=0;sidx < 4;sidx++) {
                        for(let nidx=0;nidx < 13;nidx++) {
                            const score = this.predMyHandMatrix[sidx][nidx]
                            if(score >= threeCardsScore && score < straightScore) {
                                const card = {Suite: sidx, Number: nidx, Score: score, Color: "#DD9",}
                                chanceCards.push(card)
                            }
                            if(score >= straightScore) {
                                const card = {Suite: sidx, Number: nidx, Score: score, Color: "#D9D",}
                                chanceCards.push(card)
                            }
                        }
                    }
                }
                this.chanceCards = chanceCards
            }
            if(pokerGameInfo.NextTableHandPred) {
                this.predTableHandMatrix = this.encodeCardPredStr(pokerGameInfo.NextTableHandPred)
                let tableNextCards = []
                if(this.players.length > 0) {
                    for(let sidx=0;sidx < 4;sidx++) {
                        for(let nidx=0;nidx < 13;nidx++) {
                            const score = this.predTableHandMatrix[sidx][nidx]
                            if(score >= threeCardsScore && score < straightScore) {
                                const card = {Suite: sidx, Number: nidx, Score: score, Color: "#DD9",}
                                tableNextCards.push(card)
                            }
                            if(score >= straightScore) {
                                const card = {Suite: sidx, Number: nidx, Score: score, Color: "#D9D",}
                                tableNextCards.push(card)
                            }
                        }
                    }
                }
                this.tableNextCards = tableNextCards
            }

            return pokerGameInfo
        },
        async getPokerInfo() {
            const pokerGame = axios.post("/api/game/poker/info")
            return pokerGame
        },
        async postPokerAction(actionMeta) {
            const pokerGame = axios.post("/api/game/poker/action", actionMeta)
            return pokerGame
        },
    }
}
</script>



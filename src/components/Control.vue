<template>
    <div class="control">

        <h2>仮想マシンの設定</h2>

        <p v-text="statusText">ここにステータス</p>

        <form id="server_config">

            <div class="box6">

                <div id="a">
                    <label for="cpu">CPUコア数：　　　　</label>
                    <input type="number" id="cpu" min="1" max="2" value="1" required/>
                </div>

                <div id="b">
                    <label for="mem">メモリ容量(MB)：　　</label>
                    <input type="number" id="mem" min="1024" max="4096" step="1024" value="1024" required/>
                </div>

                <div id="c">
                    <label for="hdd">ストレージ容量(GB)：</label>
                    <input type="number" id="hdd" min="8" max="16" value="8" required/>
                </div>

                <div id="d">
                    <label for="os">オペレーティングシステム：　　</label>
                    <select id="os" name="os" required>
                        <option value="debian">Debian</option>
                        <option value="netbsd" selected>NetBSD</option>
                    </select>
                </div>

            </div>

            <input type="button" id="e" value="仮想マシンを作成" @click="formSubmit"/>

        </form>

    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "Control",
        data() {
            return {
                statusText: "VM（チカラ）がほしいか…？　ならばスペックをキメよ",
                dataset: {},
                headers: {
                    'Content-Type': 'text/json; charset=UTF-8',
                    'Access-Control-Allow-Origin': '*',
                }
            }
        },
        methods: {
            test: function () {
                alert("PUSH")
                this.statusText = "もう一度強く願え"
            },

            formSubmit: function () {
                var cpus = document.getElementById('cpu').value
                var mem = document.getElementById('mem').value
                var hdd = document.getElementById('hdd').value
                var os = document.getElementById('os').value

                axios
                    .post("CGI", {
                        headers: this.headers
                    })
                    .finally(this.statusText = cpus + "core, メモリ=" + mem + "MB, HDD=" + hdd + "GB, OS=" + os + "でVMを作成しています...")
                    .then(response => {
                        this.statusText = cpus + "core, メモリ=" + mem + "MB, HDD=" + hdd + "GB, OS=" + os + "でVMを作成しました"
                        console.log(JSON.stringify(response.data.name))
                    })
                    .catch(err => {
                        this.statusText = "願いは聞き届けられなかった"
                        console.dir(err.toString())
                    })
            }
        }
    }
</script>

<style scoped>
    #a, #b, #c, #d, #e {
        margin-top: 10px;
        display: flow;
        flex-flow: column wrap;
        justify-content: center;
        align-items: center;
    }

    .box6 {
        padding: 0.5em 1em;
        margin: 2em 0;
        background: #f0f7ff;
        border: dashed 2px #5b8bd0;/*点線*/
    }
    .box6 p {
        margin: 0;
        padding: 0;
    }
</style>
<script>
    export default {
        name: "vue-type-text",
        props: {
            text: {type: String, default: ''},
            cursor: {type: String, default: '|'},
            interval: {type: Number, default: 50},
            eraseInterval: {type: Number, default: 20},
            cursorInterval: {type: Number, default: 700},
        },
        data: function() {
            return {
                currentText: '',
                showCursor: false,
            }
        },
        mounted() {
            this.updateText(this.text);
            if (this.cursor) {
                setInterval(() => this.showCursor = !this.showCursor, this.cursorInterval); // Cursor Flash
            }
        },
        methods: {
            updateText: function(newText) {
                if (newText === null || typeof(newText) === 'undefined') {
                    return;
                }
                let i1 = setInterval(() => {
                    if (this.currentText.length) {
                        this.currentText = this.currentText.substring(0, this.currentText.length - 1);
                    } else {
                        clearInterval(i1);
                        let chars = newText.split('');
                        let index = 0;
                        let i2 = setInterval(() => {
                            if (this.currentText !== newText && chars && chars[index]) {
                                this.currentText += chars[index++];
                            } else {
                                clearInterval(i2);
                            }
                        }, this.eraseInterval);
                    }
                }, this.interval);
            },
        },
        watch: {
            text: function(x) {
                this.updateText(x);
            },
        }
    }
</script>
<template>
    <div id="container">
        <span id="text" v-text="currentText"/>
        <span id="cursor" :style="{opacity: showCursor ? 0.8 : 0}" v-text="cursor"/>
    </div>
</template>
<style scoped>
    #container {
        display: flex;
        align-items: center;
    }
    #cursor {
        margin-left: 3px;
        margin-top: -2px;
        font-weight: 800;
        font-size: 1.2em;
    }
</style>
<template>
    <canvas ref="canvas"></canvas>
</template>

<script>
    export default {
        name: 'Canvas-Component',
        data() {
            return {
                textStrings: [],
                noOfTextStrings: 250,
                fontSize: 12,
                fontColor: '#3a3', // Green color for text
                fontFamily: 'monospace',
                letters: "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789あアかカさサたタなナはハまマやヤらラわワいイきキしシちチにニひヒみミりリゐヰうウくクすスつツぬヌふフむムゆユるルえエけケせセてテねネへヘめメれレゑヱおオこコそソとトのノほホもモよヨろロをヲんンゃゅょ",
            };
        },
        mounted() {
            this.initCanvas();
            for (let i = 0; i < this.noOfTextStrings; i++) {
                this.addTextString(this.generateText(), Math.random() * window.innerWidth, Math.random() * window.innerHeight, 2 + Math.random() * 10);
            }
            // this.addTextString('Hello, Matrix!', 0, 0, 10);
            var repeat = setInterval(() => {
                // console.log('Drawing text...');
                this.drawText();
            }, 100);
            console.log(repeat);
        },
        methods: {
            initCanvas() {
                const canvas = this.$refs.canvas;
                if (canvas) {
                    const ctx = canvas.getContext('2d');
                    canvas.width = window.innerWidth;
                    canvas.height = window.innerHeight;
                    ctx.fillStyle = '#000';
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    // ctx.scale(-1, 1);
                }
            },
            addTextString(text, x, y, speed) {
                this.textStrings.push({ text, x, y, speed });
            },
            drawText() {
                const canvas = this.$refs.canvas;
                if (canvas) {
                    const ctx = canvas.getContext('2d');
                    ctx.clearRect(0, 0, canvas.width, canvas.height);
                    ctx.fillStyle = '#000';
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    ctx.fillStyle = this.fontColor; // Green color for text
                    this.textStrings.forEach((textString) => {
                        ctx.font = this.fontSize + 'px ' + this.fontFamily;
                        ctx.scale(-1, 1);
                        textString.text.split('').forEach((char, index) => {
                            const text = char === ' ' ? ' ' : char; // Keep spaces as is
                            ctx.fillText(text, -textString.x, textString.y + index * this.fontSize);
                        });
                        textString.y += textString.speed; // Move text to the right
                        if (textString.y > canvas.height) {
                            textString.y = textString.text.length * -1 * this.fontSize; // Reset position if it goes off screen
                            textString.x = Math.random() * canvas.width; // Randomize x position
                        }
                    });
                }
            },
            generateText() {
                let text = '';
                let length = Math.floor(Math.random() * 20) + 5; // Random length between 1 and 10
                for (let i = 0; i < length; i++) {
                    text += this.letters.charAt(Math.floor(Math.random() * this.letters.length));
                }
                return text;
            }
        }
    };
</script>

<style>
    html, body {
        margin: 0;
        padding: 0;
        overflow: hidden; /* Prevent scrollbars */
    }
</style>
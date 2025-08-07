<template>
    <canvas ref="canvas"></canvas>
</template>

<script>
    export default {
        name: 'Canvas-Component',
        data() {
            return {
                textStrings: [],
                noOfTextStrings: 300,
                fontSize: 12,
                fontColor: '#3a3', // Green color for text
                fontFamily: 'monospace',
                letters: "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789あアかカさサたタなナはハまマやヤらラわワいイきキしシちチにニひヒみミりリゐヰうウくクすスつツぬヌふフむムゆユるルえエけケせセてテねネへヘめメれレゑヱおオこコそソとトのノほホもモよヨろロをヲんンゃゅょ",
                specialMessages: [
                    'Hello, World!',
                    'Welcome to Matrix Saver',
                    'Enjoy the Matrix Effect',
                    'Keep Calm and Code On',
                    'Stay Safe, Stay Home',
                    'Matrix Reloaded',
                    'Code is Poetry',
                    'Embrace the Matrix'
                ]
            };
        },
        mounted() {
            this.initCanvas();
            for (let i = 0; i < this.noOfTextStrings; i++) {
                this.addTextString(this.generateText(), Math.random() * window.innerWidth, Math.random() * window.innerHeight, 2 + Math.random() * 10, Math.random() < 0.5);
            }
            console.log(this.textStrings.map((textString) => textString.flipped).filter(flipped => flipped).length + ' flipped text strings');
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
            addTextString(text, x, y, speed, flipped) {
                const special = Math.random() < 0.1; // 10% chance to add a special message
                if (special) {
                    text = this.specialMessages[Math.floor(Math.random() * this.specialMessages.length)];
                    flipped = false;
                }
                this.textStrings.push({ text, x, y, speed, flipped });
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
                        ctx.scale(-1, 1)
                        if (textString.flipped) {
                            const gradient = ctx.createLinearGradient(textString.x, textString.y, textString.x + this.fontSize, textString.y + textString.text.length * this.fontSize);
                            gradient.addColorStop(0, '#333'); // Green color for text
                            gradient.addColorStop(1, this.fontColor); // Lighter green for gradient
                            ctx.fillStyle = gradient; // Apply gradient
                                
                            textString.text.split('').forEach((char, index) => {
                                // ctx.fillStyle = this.fontColor; // Set color for each character
                                let text = char === ' ' ? ' ' : char; // Keep spaces as is
                                text = Math.random() < 0.1 ? this.letters.charAt(Math.floor(Math.random() * this.letters.length)) : text; // Randomize characters
                                if (index === textString.text.length - 1) {
                                    ctx.fillStyle = '#3f3'; // Change color for the last character
                                    ctx.fillText(text, -textString.x, textString.y + index * this.fontSize);
                                } else {
                                    ctx.fillText(text, -textString.x, textString.y + index * this.fontSize);
                                }
                            });
                            ctx.fillStyle = this.fontColor;
                        } else {
                            ctx.font = this.fontSize + 'px ' + this.fontFamily;
                            const gradient = ctx.createLinearGradient(textString.x, textString.y, textString.x + this.fontSize, textString.y + textString.text.length * this.fontSize);
                            gradient.addColorStop(0, '#333'); // Green color for text
                            gradient.addColorStop(1, this.fontColor); // Lighter green for gradient
                            ctx.fillStyle = gradient; // Apply gradient
                            textString.text.split('').forEach((char, index) => {
                                let text = char === ' ' ? ' ' : char; // Keep spaces as is
                                text = Math.random() < 0.05 ? this.letters.charAt(Math.floor(Math.random() * this.letters.length)) : text; // Randomize characters
                                if (index === textString.text.length - 1) {
                                    ctx.fillStyle = '#3f3'; // Reset color for the next text string
                                    ctx.fillText(text, textString.x, textString.y + index * this.fontSize);
                                } else {
                                    ctx.fillText(text, textString.x, textString.y + index * this.fontSize);
                                }
                            });
                        }
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

    canvas {
        display: block; /* Remove inline-block spacing */
        width: 100vw; /* Full viewport width */
        height: 100vh; /* Full viewport height */
    }
</style>
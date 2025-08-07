<template>
    <div id="stage">
        <canvas ref="canvas"></canvas>
        <canvas ref="backgroundCanvas"></canvas>
    </div>
</template>

<script>
    export default {
        name: 'Canvas-Component',
        data() {
            return {
                textStrings: [], // Array to hold text strings
                noOfTextStrings: 200, // Number of text strings to generate
                fontSize: 12, // Font size for text
                fontColor: '#3a3', // Green color for text
                tailColor: '#333', // Color for the tail effect
                highlightColor: '#3f3', // Color for highlighted text
                fontFamily: 'monospace', // Font family for text
                // letters to use for random text generation
                letters: "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789あアかカさサたタなナはハまマやヤらラわワいイきキしシちチにニひヒみミりリゐヰうウくクすスつツぬヌふフむムゆユるルえエけケせセてテねネへヘめメれレゑヱおオこコそソとトのノほホもモよヨろロをヲんンゃゅょ",
                // Array of special messages to display
                specialMessages: [
                    'Hello, World!',
                    'Welcome to Matrix Saver',
                    'Enjoy the Matrix Effect',
                    'Keep Calm and Code On',
                    'Stay Safe, Stay Home',
                    'Matrix Reloaded',
                    'Code is Poetry',
                    'Embrace the Matrix'
                ],
                // Offscreen canvas for performance
                offscreenCanvas: document.createElement('canvas'),
            };
        },
        mounted() {
            this.initCanvas(); // Initialize the canvas
            // Generate initial text strings
            for (let i = 0; i < this.noOfTextStrings; i++) {
                this.addTextString(this.generateText(), Math.floor(Math.random() * window.innerWidth), Math.floor(Math.random() * window.innerHeight), Math.floor(1 + Math.random() * 5), Math.random() < 0.5);
            }
            // Set up offscreen canvas
            this.offscreenCanvas.width = window.innerWidth;
            this.offscreenCanvas.height = window.innerHeight;
            
            let self = this;
            // Start the animation loop
            function draw () {
                self.drawText();
                window.requestAnimationFrame(draw);
            }
            window.requestAnimationFrame(draw);
        },
        methods: {
            // Initialize the canvas
            initCanvas() {
                const canvas = this.$refs.canvas;
                if (canvas) {
                    const ctx = canvas.getContext('2d');
                    canvas.width = window.innerWidth;
                    canvas.height = window.innerHeight;
                    ctx.fillStyle = '#000';
                }
            },
            // Add a new text string to the array
            addTextString(text, x, y, speed, flipped) {
                const special = Math.random() < 0.1; // 10% chance to add a special message
                if (special) {
                    text = this.specialMessages[Math.floor(Math.random() * this.specialMessages.length)];
                    flipped = false;
                }
                this.textStrings.push({ text, x, y, speed, flipped });
            },
            // Draw the text strings on the canvas
            drawText() {
                const canvas = this.$refs.canvas;
                if (canvas) {
                    const ctx = this.offscreenCanvas.getContext('2d'); // Use offscreen canvas for performance
                    ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear the offscreen canvas
                    ctx.fillStyle = this.fontColor; // Green color for text
                    this.textStrings.forEach((textString) => {
                        ctx.font = this.fontSize + 'px ' + this.fontFamily;
                        ctx.scale(-1, 1)
                        if (textString.flipped) {
                            const gradient = ctx.createLinearGradient(textString.x, textString.y, textString.x + this.fontSize, textString.y + textString.text.length * this.fontSize);
                            gradient.addColorStop(0, this.tailColor); // Green color for text
                            gradient.addColorStop(1, this.fontColor); // Lighter green for gradient
                            ctx.fillStyle = gradient; // Apply gradient
                                
                            textString.text.split('').forEach((char, index) => {
                                // ctx.fillStyle = this.fontColor; // Set color for each character
                                let text = char === ' ' ? ' ' : char; // Keep spaces as is
                                text = Math.random() < 0.1 ? this.letters.charAt(Math.floor(Math.random() * this.letters.length)) : text; // Randomize characters
                                if (index === textString.text.length - 1) {
                                    ctx.fillStyle = this.highlightColor; // Change color for the last character
                                    ctx.fillText(text, -textString.x, textString.y + index * this.fontSize);
                                } else {
                                    ctx.fillText(text, -textString.x, textString.y + index * this.fontSize);
                                }
                            });
                            ctx.fillStyle = this.fontColor;
                        } else {
                            ctx.font = this.fontSize + 'px ' + this.fontFamily;
                            const gradient = ctx.createLinearGradient(textString.x, textString.y, textString.x + this.fontSize, textString.y + textString.text.length * this.fontSize);
                            gradient.addColorStop(0, this.tailColor); // Green color for text
                            gradient.addColorStop(1, this.fontColor); // Lighter green for gradient
                            ctx.fillStyle = gradient; // Apply gradient

                            textString.text.split('').forEach((char, index) => {
                                let text = char === ' ' ? ' ' : char; // Keep spaces as is
                                text = Math.random() < 0.05 ? this.letters.charAt(Math.floor(Math.random() * this.letters.length)) : text; // Randomize characters
                                if (index === textString.text.length - 1) {
                                    ctx.fillStyle = this.highlightColor; // Reset color for the next text string
                                    ctx.fillText(text, textString.x, textString.y + index * this.fontSize);
                                } else {
                                    ctx.fillText(text, textString.x, textString.y + index * this.fontSize);
                                }
                            });
                        }

                        textString.y += textString.speed; // Move text to the bottom
                        if (textString.y > canvas.height) {
                            textString.y = textString.text.length * -1 * this.fontSize; // Reset position if it goes off screen
                            textString.x = Math.floor(Math.random() * canvas.width); // Randomize x position
                        }
                    });

                    // Draw the offscreen canvas onto the main canvas
                    canvas.getContext('2d').clearRect(0, 0, canvas.width, canvas.height);
                    canvas.getContext('2d').drawImage(this.offscreenCanvas, 0, 0);
                }
            },
            generateText() {
                let text = '';
                let length = Math.floor(Math.random() * 25) + 5; // Random length between 1 and 10
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
        background: rgba(0, 0, 0, 255); /* Transparent background */
    }
</style>
<script>
export default {
    data() {
        return {
            score: 0
        }
    },
    mounted() {
    },
    methods: {
        start(event) {
            let compArr = []
            let youArr = []
            const squares = document.querySelectorAll('.square');
            const screen = document.querySelector('.screen');
            let speed = parseInt(event.currentTarget.getAttribute('data-speed'));
            let active_period = 200

            screen.style.display = 'none'
            this.score = 0;

            function startGame() {
                for (let per = 1; per <= 4; per++) {
                    squares.forEach(square => {
                        square.classList.add('disabled');
                    });

                    let num = Math.floor(Math.random() * 4) + 1;
                    let block = document.getElementById('square' + num);
                    compArr.push(block.id)

                    document.querySelector('.game h1').textContent = 'Следите за последовательностью';

                    ((per) => {
                        setTimeout(() => {
                            block.classList.add('active');
                            setTimeout(() => {
                              block.classList.remove('active');
                            }, active_period); // периодичность загорания   
                            if (per === 4) {
                                document.querySelector('.game h1').textContent = 'Повторите последовательность';
                                squares.forEach(square => {
                                      square.classList.remove('disabled');
                                });
                            } 
                        }, speed * per); // скорость игры
                    })(per);
                }
            }

            console.log(speed)

            startGame()

            document.querySelectorAll('.square').forEach(square => {
                square.addEventListener('click', () => {
                    youArr.push(square.id)
                    if (youArr.length > 3 && youArr.toString() === compArr.toString()) {
                        screen.style.display = 'none';
                        this.score++;
                        youArr.splice(0, youArr.length)
                        compArr.splice(0, compArr.length)
                        startGame()
                        switch(true) {
                            case (speed <= 1500 && speed > 800): speed = speed - 100; break;
                            case (speed <= 800 && speed > 500): speed = speed - 50; break;
                            case (speed <= 500 && speed > 60): speed = speed - 20; break;
                            case (speed == 60): speed = speed; break;
                            
                            case (speed <= 300 && speed > 140): active_period = 100; break;
                            case (speed <= 140): active_period = 80; break;
                        }
                    } 
                    else if (youArr.length > 3 && youArr.toString() !== compArr.toString()){
                        screen.style.display = 'flex';
                        document.querySelector('.screen h2').style.display = 'block';
                        youArr.splice(0, youArr.length);
                        compArr.splice(0, compArr.length);
                    }
                });
            });
        }
    }
}
</script>

<template>
    <div class="game">
        <div class="score">Очки: {{ score }}</div>
        <h1>Следите за последовательностью</h1>
        <div class="game-keyboard">
            <div class="square" id="square1"></div>
            <div class="square" id="square2"></div>
            <div class="square" id="square3"></div>
            <div class="square" id="square4"></div>
        </div>
    </div>
    <div class="screen">
        <div class="wrapper">
            <h2 style="display: none;">Игра окончена! Вы заработали {{ score }} очков</h2>
            <h1>Выберите сложность</h1>
            <div class="content">
                <button type="submit" @click="start" data-speed="1500">Лёгкий</button>
                <button type="submit" @click="start" data-speed="1000">Средний</button>
                <button type="submit" @click="start" data-speed="400">Сложный</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.game {
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.score {
    position: absolute;
    left: 30px;
    top: 30px;
    font-size: 30px;
}
.game-keyboard {
    display: flex;
    margin-top: 50px;
}
.square {
    cursor: pointer;
    background-color: #fff;
    width: 200px;
    height: 200px;
    border-radius: 10px;
    margin-right: 20px;
}
.square:active, .active {
    background-color: #00ffe5;
}
.disabled {
    pointer-events: none;
}
.screen {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    background: linear-gradient(rgba(0,0,0,0.8), rgba(0,0,0,0.8));
    display: flex;
    justify-content: center;
    align-items: center;
}
.content {
    display: flex;
    margin-top: 100px;
}
.screen .content button {
    margin: 0 30px;
}

@media (max-width: 880px) {
    .square {
        width: 130px;
        height: 130px;
    }
}
@media (max-width: 600px) {
    .game-keyboard {
        flex-direction: column;
    }
    .square {
        width: 150px;
        height: 150px;
        margin: 0 0 20px 0;
    }
    .content {
        flex-direction: column;
    }
    .content button {
        margin: 0 0 30px 0;
    }
}
</style>

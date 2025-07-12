<template>
  <div class="timer-container" :style="{ backgroundColor: backgroundColor }" @click="handleClick">
    <div class="timer-display" :style="{ fontSize: fontSize }">
      <span>{{ time.toFixed(1) }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "PcupTimer",
  props: {
    threshold: {
      type: Number,
      default: 30, // 30秒を初期値として設定
    },
  },
  data() {
    return {
      time: 0.0,
      isRunning: false,
      timerInterval: null,
      backgroundColor: "blue", // 初期背景色
      audio: null, // 音声ファイルの参照
      fontSize: "10kvw", // 動的フォントサイズ
    };
  },
  mounted() {
    this.updateFontSize();
    window.addEventListener('resize', this.updateFontSize);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.updateFontSize);
  },
  methods: {
    updateFontSize() {
      const screenWidth = window.innerWidth;
      const fontSize = screenWidth * 0.35; // 画面幅の35%
      this.fontSize = fontSize + 'px';
    },
    handleClick(event) {
      const pageHeight = window.innerHeight;
      if (event.clientY < pageHeight / 2) {
        // 上半分を押した場合
        if (this.isRunning) {
          this.backgroundColor = "red"; // 動作中なら背景を赤色に変更
          if (Math.floor(this.time) >= this.threshold) {
            this.resetAndStartTimer(); // タイマーをリセットして再スタート
          }
        } else {
          this.startTimer(); // タイマーを開始
        }
      } else {
        // 下半分を押した場合
        if (this.isRunning) {
          this.resetAndStartTimer(); // タイマーをリセットして再スタート
        }
      }
    },
    startTimer() {
      if (!this.isRunning) {
        this.isRunning = true;
        this.backgroundColor = "yellow"; // タイマー開始時の背景色
        this.timerInterval = setInterval(() => {
          this.time += 0.1;
          if (Math.floor(this.time) === this.threshold) {
            this.handleThresholdReached(); // 設定された閾値(デフォルト30秒)に達したときの処理
          }
        }, 100);
      }
    },
    resetAndStartTimer() {
      this.resetTimer();
      this.startTimer();
    },
    resetTimer() {
      clearInterval(this.timerInterval);
      this.time = 0.0;
      this.isRunning = false;
    },
    handleThresholdReached() {
      this.playSound(); // 音声を再生
      // 他の処理が必要であればここに追加可能
      this.backgroundColor = "blue"; // 閾値に達したときの背景色
    },
    playSound() {
      if (!this.audio) {
        this.audio = new Audio(require("@/assets/test.mp3")); // 音声ファイルの読み込み
      }
      this.audio.play(); // 音声を再生
    },
  },
};
</script>

<style scoped>
.timer-container {
  height: 100vh; /* ページ全体の高さ */
  width: 100vw; /* ページ全体の幅 */
  display: flex;
  align-items: center;
  justify-content: flex-start; /* 左寄せに変更 */
  padding-left: 5vw; /* 画面幅の25%だけ左から余白を作って視覚的に中央に */
  margin: 0;
  overflow: hidden;
}

.timer-display {
  font-weight: bold;
  color: black;
  text-align: left;
  background-color: rgba(255, 255, 255, 0.7); /* 背景を半透明に */
  padding: 2vw 4vw; /* パディングも可変に */
  border-radius: 1vw; /* 角の丸みも可変に */
}
</style>

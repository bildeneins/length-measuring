<template>
  <div id="image-viewer">
    <canvas id="canvas" />
  </div>
</template>

<script>
export default {
  name: "ImageViewer",
  props: {
    length: Number,
  },
  data() {
    return {
      image: null,
      imgWidth: null,
      imgHeight: null,
      // imgWidth_cm: this.length, //もともと30.0
      canvasWidth: null,
      canvasHeight: null,
      clickX: null,
      clickY: null,
    };
  },

  computed: {
    actualImgWidth() {
      return this.canvasWidth;
    },
    actualImgHeight() {
      return (this.canvasWidth * this.imgHeight) / this.imgWidth;
    },
    imgWidth_cm() {
      return this.length;
    },
  },
  mounted() {
    // 画像読み込み処理
    this.image = new Image();
    this.image.onload = () => {
      // 読み込み完了時の処理を指定しておく
      console.log("画像を読み込みました.");
      this.imgWidth = this.image.naturalWidth;
      this.imgHeight = this.image.naturalHeight;
      this.draw();
    };
    this.image.src = "IMG_8488.JPG"; // パスを指定すると読み込み開始
    // キャンバスサイズ取得
    this.updateCanvasSize();
    // マウスクリックを監視
    const canvas = document.getElementById("canvas");
    canvas.addEventListener("click", (e) => {
      this.clickX = e.clientX - e.target.getBoundingClientRect().left;
      this.clickY = e.clientY - e.target.getBoundingClientRect().top;
      console.log(this.clickX, this.clickY);
      console.log(this.length, this.imgWidth_cm);
      this.draw();
    });
    // ウィンドウサイズ変更を監視
    window.addEventListener("resize", this.updateCanvasSize);
  },
  beforeUpdate() {
    this.imgWidth_cm = this.length;
  },
  methods: {
    draw() {
      // 全ての要素を描画
      const canvas = document.getElementById("canvas");
      const context = canvas.getContext("2d");
      context.clearRect(0, 0, this.canvasWidth, this.canvasHeight);
      this.drawPicture();
      this.drawRuler();

      // 親要素にcanvasを送信
      this.$emit("update", canvas);
    },
    drawPicture() {
      // 検査品の画像を描画
      const canvas = document.getElementById("canvas");
      const context = canvas.getContext("2d");
      context.drawImage(
        this.image,
        0,
        0,
        this.imgWidth,
        this.imgHeight,
        0,
        (this.canvasHeight - this.actualImgHeight) / 2,
        this.actualImgWidth,
        this.actualImgHeight
      );
    },
    drawRuler() {
      if (this.clickX == null) return;
      const sx = this.clickX;
      const sy = this.clickY;
      // 定規を描画
      const canvas = document.getElementById("canvas");
      const context = canvas.getContext("2d");

      context.fillStyle = "rgb(200,50,60)";
      context.fillRect(sx - 10, sy, this.canvasWidth, 60);

      // 画面端まで定規を描画
      for (let i = 0; i < 1000; i++) {
        context.beginPath();
        const x = sx + i * ((this.canvasWidth / this.imgWidth_cm) * 0.5); // 5mm 刻み
        if (x > this.canvasWidth) {
          break;
        }
        context.moveTo(x, sy);
        if (i % 20 === 0) {
          context.lineTo(x, sy + 40);
        } else if (i % 2 === 0) {
          context.lineTo(x, sy + 20);
        } else {
          context.lineTo(x, sy + 10);
        }

        if (i % 2 === 0) {
          context.fillStyle = "white";
          context.fillText(i / 2, x, sy + 50);
        }

        context.stroke();
        context.strokeStyle = "white";
      }
    },

    updateCanvasSize() {
      // canvasの外側のdiv要素を取得
      const container = document.getElementById("image-viewer");
      this.canvasWidth = container.clientWidth;
      this.canvasHeight = container.offsetHeight;
      // canvas要素を取得
      this.canvas = document.getElementById("canvas");
      this.canvas.width = this.canvasWidth;
      this.canvas.height = this.canvasHeight;
      console.log(
        `キャンバス 幅:${this.canvasWidth}, 高さ:${this.canvasHeight}`
      );
      // 画像が読み込まれていれば描画
      if (this.image.complete) {
        this.draw();
      }
    },
  },
};
</script>

<style scoped>
#image-viewer {
  padding: 0.5rem;
  border: 1px #425066;
  background-color: #425066;
  border-radius: 5px;
  width: 1300px;
  height: 500px;
}
#canvas {
  width: 100%;
  height: 100%;
}
</style>

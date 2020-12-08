<template>
  <div class="container-fluid hello">
    <div class="row">
      <div class="form-group col">
        <label for="input1" class="">ワークの長さ</label>
        <div class="row">
          <div class="col"></div>
          <div class="col-5">
            <input
              type="number"
              class="form-control"
              id="input1"
              style="text-align: right"
              v-model.number="length"
            />
            <small id="passwordHelpInline1" class="text-muted float-right">
              cm
            </small>
          </div>
          <div class="col"></div>
        </div>
      </div>
      <div class="form-group col">
        <label for="input2" class="">露光時間</label>
        <div class="row">
          <div class="col"></div>
          <div class="col-5">
            <input
              type="text"
              class="form-control"
              id="input2"
              style="text-align: right"
              v-model="time"
            />
            <small id="passwordHelpInline2" class="text-muted float-right">
              ms
            </small>
          </div>
          <div class="col"></div>
        </div>
      </div>
    </div>

    <div class="row d-flex justify-content-center p-2 mr-2 ml-2">
      <button
        class="btn btn-primary mr-2"
        type="submit"
        key="button1"
        v-on:click="onclick1"
      >
        開始
      </button>
      <button
        class="btn btn-danger mr-2"
        type="submit"
        key="button2"
        v-on:click="onclick2"
      >
        中断
      </button>
      <button
        class="btn btn-success mr-2"
        type="submit"
        key="button3"
        v-on:click="onclick3"
      >
        プレビュー
      </button>
    </div>

    <div class="row">
      <image-viewer v-on:update="updateCanvas" v-bind:length="length">
      </image-viewer>
    </div>

    <div class="row d-flex justify-content-center ">
      <button
        class="btn btn-secondary mr-2"
        type="submit"
        key="button4"
        v-on:click="onclick4"
      >
        保存
      </button>
      <button
        class="btn btn-info mr-2 "
        type="submit"
        key="button5"
        v-on:click="onclick5"
      >
        出力
      </button>
    </div>
  </div>
</template>

<script>
import ImageViewer from "@/components/ImageViewer";
import fs from "fs";
import os from "os";
import dataUriToBuffer from "data-uri-to-buffer";
import path from "path";
import { remote } from "electron"; //electron公式がremoteから移行していこうとしているので再検討の余地あり

const { dialog } = remote.require("electron");

// ファイルの保存先
const desktopDirName = "Desktop";
const imageFileName = "my-canvas.png";
const homeDirPath = os.homedir();
const desktopDirPath = path.join(homeDirPath, desktopDirName);
const imageFilePath = path.join(desktopDirPath, imageFileName);

export default {
  name: "HelloWorld",
  components: {
    ImageViewer,
  },
  data() {
    return {
      length: 10,
      time: "10",
      imageCanvas: null, // ImageViewerに入っているCanvas要素
    };
  },
  methods: {
    updateCanvas(canvas) {
      this.imageCanvas = canvas;
    },
    onclick1: function() {
      console.log("開始");
    },
    onclick2: function() {
      console.log("中断");
    },
    onclick3: function() {
      console.log("プレビュー");
    },
    onclick4: function() {
      const options = {
        title: "ファイル保存",
        defaultPath: imageFilePath,
        message: "ファイル名を入力してください。",
        nameFieldLabel: "ファイル名",
        properties: ["createDirectory", "showOverwriteConfirmation"],
      };
      const enteredFilePath = dialog.showSaveDialogSync(null, options);
      if (enteredFilePath === undefined) {
        //ダイアログでキャンセルを選択するとundifinedが返される
        console.log("Cancel selected in dialog");
      } else {
        const canvasDataUrl = this.imageCanvas.toDataURL();
        const decoded = dataUriToBuffer(canvasDataUrl);
        fs.writeFile(enteredFilePath, decoded, (err) => {
          if (err) {
            // window.alert("ファイルの保存に失敗しました");
            dialog.showErrorBox("エラー", "ファイルを保存できません。");
            console.log(err);
          } else {
            // window.alert("ファイルを保存しました");

            dialog.showMessageBox(null, {
              type: "info",
              message: "ファイルを保存しました",
            });
          }
        });
        console.log("保存");
      }
    },
    onclick5: function() {
      console.log("出力");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* .hello {
  padding: 1rem;
  margin: 2rem;
} */
.form-group {
  margin: 5px;
  padding: 1rem;
  background-color: #425065;
  border-radius: 5px;
}
.row {
  margin-bottom: 1rem;
}
</style>

<template>
  <div class="hello">
 <div class="container">
<div class="row d-flex bd-highlight mb-3">
  <div class="form-group row m-0 p-0">
    <label for="input1" class="col-5 col-form-label">ワークの長さ</label>
    <div class="col-4" style="padding: 0;">
      <input type="text" class="form-control" id="input1" style="text-align:right" v-model="length">
      <small id="passwordHelpInline1" class="text-muted float-right">
      cm
    </small>
    </div>
  </div>
  <div class="form-group row pl-1 pr-1">
    <label for="input2" class="col-5 col-form-label">露光時間</label>
    <div class="col-5">
      <input type="text" class="form-control" id="input2" style="text-align:right" v-model="time">
      <small id="passwordHelpInline2" class="text-muted float-right">
      ms
    </small>
    </div>
  </div>
  
    <div class="float-right ml-auto p-2 bd-highlight mr-0">
      <button class="btn btn-outline-primary mr-2 rounded-0" type="submit" key="button1" v-on:click="onclick1">開始</button>
      <button class="btn btn-outline-danger mr-2 rounded-0" type="submit" key="button2" v-on:click="onclick2">中断</button>
      <button class="btn btn-outline-success mr-2 rounded-0" type="submit" key="button3" v-on:click="onclick3">プレビュー</button>
    </div>
    </div>
    </div>
    <br><br>


    <image-viewer> </image-viewer>
    
    <br><br>
    <div class="float-right">
      <button class="btn btn-outline-secondary mr-2 rounded-0" type="submit" key="button4" v-on:click="onclick4">保存</button>
      <button class="btn btn-outline-info mr-2 rounded-0" type="submit" key="button5" v-on:click="onclick5">出力</button>
      <button class="btn col-3 disabled mr-2 rounded-0 " type="submit" key="button5" v-on:click="onclick5"></button>
      <button class="btn col-3 disabled mr-2 rounded-0 " type="submit" key="button5" v-on:click="onclick5"></button>
      <button class="btn col-3 disabled mr-2 rounded-0 " type="submit" key="button5" v-on:click="onclick5"></button>
      <button class="btn col-3 disabled mr-2 rounded-0 " type="submit" key="button5" v-on:click="onclick5"></button>
      <button class="btn col-3 disabled mr-2 rounded-0 " type="submit" key="button5" v-on:click="onclick5"></button>
    </div>  
     
  </div>
</template>

<script>
import ImageViewer from "@/components/ImageViewer"
export default {
  name: 'HelloWorld',
  components: {
    ImageViewer
  },
  data(){
    return{
      length:'10',
      time:'10'
    }
  },
  methods:{  
    onclick1: function(){
      console.log('開始');
    },
    onclick2: function(){
      console.log('中断');
    },
    onclick3: function(){
      console.log('プレビュー');
    },
    onclick4: function(){
      console.log('保存');
    },
    onclick5: function(){
      console.log('出力');
    }
  }
};


</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

<script setup>
import { ref, nextTick } from 'vue';

const props = defineProps({
  portNumber: { type: Number, required: true },
  fileName: { type: String, required: true }
});
const emit = defineEmits(['hideAlert', 'fileDownload']);

const onKeyDown = (event) => {
  // ESC 키를 눌렀을 때 hideAlert 이벤트 발생
  if (event.key === 'Escape') {
    emit('hideAlert', props.portNumber);
  }
};

let refLayerAlert = ref(null);
nextTick(() => {
  refLayerAlert.value.focus();
});
  
</script>

<template>
  <div
    ref="refLayerAlert"
    tabindex="0"
    class="layer_alert"
    @click.self="$emit('hideAlert', portNumber)"
    @keydown="onKeyDown">
    <div class="inner_layer">
      <div class="layer_body">
        <strong>파일을 다운로드 하시겠습니까?</strong>
        <p>{{ fileName }}</p>
      </div>
      <div class="layer_foot">
        <button
          type="button"
          @click="$emit('fileDownload', portNumber)"
          class="btn_confirm">
          확인
        </button>
        <button
          type="button"
          @click="$emit('hideAlert', portNumber)"
          class="btn_cancel">
          취소
        </button>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.layer_alert{
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,.7);
  .inner_layer{
    width: 280px;
    padding: 30px 20px 30px;
    background-color: #fff;
    border-radius: 4px;
    .layer_body{
      strong{
        display: block;
        font-size: 16px;
        text-align: center;
      }
      p{
        margin-top: 10px;
        padding: 5px;
        border: 1px solid #c4c4c4;
        font-weight: bold;
        font-size: 12px;
        text-align: center;
      }
    }
    .layer_foot{
      display: flex;
      justify-content: center;
      column-gap: 10px;
      padding-top: 20px;
      [class*="btn_"]{
        padding:8px 20px;
        border-radius: 4px;
        font-weight: bold;
        background-color: $color-btn;
        transition: background-color .2s;
      }
      [class*="btn_"]:hover{
        background-color: darken($color-blue, 10%);
        transition: background-color .3s;
      }
      .btn_confirm{
        background: royalblue;
        color:#fff
      }
      .btn_cancel:hover{
        background-color: darken($color-btn, 10%);
      }
    }
  }
}
</style>
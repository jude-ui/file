<script setup>
import { ref } from 'vue'
import AlertLayer from './AlertLayer.vue'

const isAlertVisible = ref(false)
const portNumber = ref(0)
const fileName = ref('')
const fileNames = [
  'portfolio_description.pdf',
  'account-2019.zip',
  'approval-2019.zip',
  'fill-2021.zip',
  'guide.zip',
  'kakaogames.zip',
  'kakaotv-2021.zip',
  'kakaowork-2020.zip',
  'mtop-2021.zip',
  'pctop-2021.zip'
]
let repoName = import.meta.env.MODE === 'production' ?  '/file' : ''

const showAlert = (portnumber, filename) => {
  isAlertVisible.value = true
  portNumber.value = portnumber
  fileName.value = filename
}
let refLinkDownload = []

const setDownloadLinkRef = (idx) => {
  return (el) => {
    refLinkDownload[idx] = el
  }
}

const hideAalert = (portNum) => {
  isAlertVisible.value = false
  refLinkDownload[portNum].focus()
}

const fileDownload = (portNum) => {
  const link = document.createElement('a')
  link.href = `${repoName}/portfolio/${fileNames[portNum]}`
  link.download = ''
  link.click()
  isAlertVisible.value = false
  refLinkDownload[portNum].focus()
}
</script>

<template>
  <div class="wrap">
    <h1 class="tit_port">
      경력증명 포트폴리오 파일 다운로드
    </h1>
    <table class="tbl_port">
      <caption class="ir_caption">
        포트폴리오 파일 목록
      </caption>
      <colgroup>
        <col>
        <col style="width:40%">
      </colgroup>
      <tbody>
        <tr
          v-for="(file, index) in fileNames"
          :key="index">
          <td>{{ file }}</td>
          <td>
            <a
              :ref="setDownloadLinkRef(index)"
              href="#"
              class="link_download"
              @click.prevent="showAlert(index, file)">
              download
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <AlertLayer
    v-if="isAlertVisible"
    :port-number="portNumber"
    :file-name="fileName"
    @file-download="fileDownload"
    @hide-alert="hideAalert" />
</template>
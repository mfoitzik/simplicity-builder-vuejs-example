<script setup lang="ts">
import { ref } from 'vue'
import {SimplicityBuilder} from '@simplicitywebtools/simplicity-builder-vue/lib'
// @ts-ignore
import helloWorldConfig from "../public/config/config_helloworld.js";
// @ts-ignore
import basicConfig from "../public/config/config_basic.js";
// @ts-ignore
import template_helloworld from "../public/templates/template_helloworld/template_helloworld.js";
// @ts-ignore
import template_basic from "../public/templates/template_basic/template_basic.js";
const currentHost = window.location.protocol + "//" + window.location.host;
const sbRef = ref<HTMLSimplicityBuilderElement | null>(null);
const output = ref<string>("");

function sbReady() {
  setHelloWorld(null);
}
function setHelloWorld(event: Event | null) {
  event?.preventDefault();
  let config = JSON.stringify(helloWorldConfig);
  const re = /##BASEADDRESS##/g;
  sbRef.value?.$el.setConfig(JSON.parse(config.replace(re, currentHost)));
  sbRef.value?.$el.setContent(template_helloworld.replace(re, currentHost));
}
function setBasic(event: Event) {
  event.preventDefault();
  let config = JSON.stringify(basicConfig);
  const re = /##BASEADDRESS##/g;
  sbRef.value?.$el.setConfig(JSON.parse(config.replace(re, currentHost)));
  sbRef.value?.$el.setContent(template_basic.replace(re, currentHost));
}
function sbSave(event: CustomEvent) {
  output.value = event.detail;
}
function sbPublish(event: CustomEvent) {
  output.value = event.detail;
}
</script>

<template>
  <div class="wrapper">
      <div class="left">
          <h1>Simplicity Builder&trade; Example</h1>
          <p><a href="" @click="setHelloWorld">Hello World Example</a></p>
          <p><a href="" @click="setBasic">Basic Example</a></p>
          <label>Save / Publish Output:</label>
          <textarea v-model="output"></textarea>
      </div>
      <div class="right">
          <simplicity-builder ref="sbRef" sbid="unique1" @ready="sbReady" @save="sbSave" @publish="sbPublish"></simplicity-builder>
      </div>
  </div>
</template>

<style scoped>

</style>

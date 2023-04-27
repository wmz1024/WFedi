<template>
  <div class="container">
    <div class="left-container">
      <div
        v-for="(item, index) in htmlData"
        :key="index"
        :data-tag="item.tag"
        class="draggable-item"
        :draggable="true"
        @dragstart="dragStart($event)"
      >
        {{ item.label }}
      </div>
    </div>
    <div class="mid-container">
      <h2>预览区域</h2>
      <div
        class="drop-area"
        @dragover.prevent
        @drop="drop($event)"
        v-html="htmlCode"
      ></div>
    </div>
    <div class="right-container">
      <h2>源码</h2>
      <pre>{{ htmlCode }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  name: "DraggableEditor",
  data() {
    return {
      htmlData: [
        { label: "<h1>标题1</h1>", tag: "h1" },
        { label: "<p>段落文本</p>", tag: "p" },
        { label: "<img src='' alt='图片' />", tag: "img" }
      ],
      htmlCode: ""
    };
  },
  methods: {
    dragStart(event) {
      event.dataTransfer.setData("text/plain", event.target.dataset.tag);
    },
    drop(event) {
      event.preventDefault();
      let tag = event.dataTransfer.getData("text/plain");
      let code = "";
      switch (tag) {
        case "h1":
          code = "<h1>标题1</h1>";
          break;
        case "p":
          code = "<p>段落文本</p>";
          break;
        case "img":
          code = "<img src='' alt='图片' />";
          break;
      }
      this.htmlCode += code;
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

.container {
  width: 100vw;
  height: 100vh;
  display: flex;
}

.left-container {
  width: 25%;
  height: 100%;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(3, 1fr);
  gap: 10px;
  padding: 20px;
  overflow-y: auto;
}

.draggable-item {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  cursor: move;
}

.draggable-item:hover {
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
}

.mid-container {
  width: 50%;
  height: 100%;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.drop-area {
  flex: 1;
  padding: 10px;
  border: 2px dashed #ccc;
  background-color: #f5f5f5;
  overflow-y: auto;
}

.drop-area:hover {
  background-color: #fafafa;
}

.right-container {
  width: 25%;
  height: 100%;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-left: 1px solid #ccc;
}

pre {
  font-size: 14px;
  white-space: pre-wrap;
  word-wrap: break-word;
  overflow-y: auto;
}

h2 {
  font-size: 20px;
  margin-bottom: 10px;
}

</style>

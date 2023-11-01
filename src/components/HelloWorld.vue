<template>
  <div id="app">
    <h1>欢迎使用软件体系结构教学软件！</h1>
    <form id="fileUploadForm" @submit.prevent="processFile">
      <div id="top">
        <label for="file">请选择文件：</label>
        <input type="file" name="file" id="file" accept=".txt" v-on:change="onFileChange">
      </div>
      <div id="bottom">
        <label for="method">请选择处理方式：</label>
        <select name="method" id="method" v-model="method">
          <option value="1">主程序-子程序</option>
          <option value="2">面向对象</option>
          <option value="3">事件系统</option>
          <option value="4">管道-过滤</option>
        </select>
      </div>
      <button type="submit">处理文件</button>
    </form>
    <!-- 显示结果的元素 -->
    <div id="result">
      <pre>{{ fileText }}</pre>
    </div>
    <!-- 新增区域 -->
    <div id="additionalSection">
      <h2>附加说明</h2>
      <button @click="showExplanation('button1')">主程序-子程序说明</button>
      <button @click="showExplanation('button2')">面向对象说明</button>
      <button @click="showExplanation('button3')">事件系统说明</button>
      <button @click="showExplanation('button4')">管道-过滤说明</button>
    </div>
    <!-- 显示附加说明的区域 -->
    <div id="explanation">
      <p>{{ explanationText }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      file: null, // 选择的文件
      method: "1", // 选择的处理方式
      fileText: "", // 文件内容
      explanationText: "" ,// 附加说明的内容
    };
  },
  methods: {
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.file = files[0];
    },
    processFile() {
      const formData = new FormData();
      formData.append("file", this.file);
      formData.append("method", this.method);

      axios.defaults.withCredentials = true;
      axios
        .post('http://localhost:8081/processFile', formData)
        .then(response => {
          this.fileText = response.data; // 将文件内容赋值给 fileText 变量
        })
        .catch(error => {
          console.error(error);
        });
    },
    showExplanation(button) {
      // 在这里可以添加按钮点击后的处理逻辑
      if (button === 'button1') {
        this.explanationText = "主程序-子程序软件体系结构在设计上使用层次化的划分方法，该体系结构中使用由编程语言直接支持的单一的控制线程。子程序的结构是明确的，子程序通常组成程序模块。子程序的调用呈现层次状，其正确与否往往取决于其调用的子程序的正确与否。相比于非结构化编程，主程序-子程序软件体系结构能够较好地支持系统的可改变性和可伸缩性等性能，其采用分而治之的策略";
      } else if (button === 'button2') {
        this.explanationText = "面向对象的软件体系结构是一种常见的设计模式，它以对象为中心，将数据和操作封装在一起，形成一个有机的实体。这种体系结构的主要组成部分包括：1.对象：对象是类的实例，每个对象都有自己的状态和行为。2.类：类是对现实世界事物的抽象，它定义了一组具有相同属性和行为的对象。3.封装：封装是将数据和操作捆绑在一起，形成一个独立的实体，这样可以隐藏内部实现细节，提高代码的可读性和可维护性。4.继承：继承允许一个类继承另一个类的属性和方法，这样可以实现代码重用，减少冗余。5.多态：多态允许一个接口被多种不同类型的对象实现，这样可以提高代码的灵活性。6.消息传递：对象之间通过消息传递进行交互，这样可以降低系统各部分之间的耦合度。";
      } else if (button === 'button3') {
        this.explanationText = "事件系统软件体系结构是一种特殊的软件设计模式，它侧重于事件的生成和处理。在这种体系结构中，生成事件通知的元素不需要感知接收方组件。事件系统主要分为隐式调用和显示调用两种。";
      } else if (button === 'button4') {
        this.explanationText = "在管道/过滤器风格的软件体系结构中，每个构件都有一组输入和输出，构件读输入的数据流，经过内部处理，然后产生输出数据流。这个过程通常通过对输入流的变换及增量计算来完成，所以在输入被完全消费之前，输出便产生了。因此，这里的构件被称为过滤器，这种风格的连接件就像是数据流传输的管道，将一个过滤器的输出传到另一过滤器的输入。此风格特别重要的 过滤器必须是独立的实体，它不能与其它的过滤器共享数据，而且一个过滤器不知道它上游和下游的标识。一个管道/过滤器网络输出的正确性并不依赖于过滤器进 行增量计算过程的顺序。";
      }
    }
  }
};
</script>

<style scoped>
#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 10px;
}

button {
  margin-right: 10px;
}

#result {
  margin-top: 20px;
  background-color: #f5f5f5;
  padding: 10px;
}

#additionalSection {
  margin-top: 20px;
}

#explanation {
  margin-top: 20px;
  background-color: #f5f5f5;
  padding: 10px;
}
</style>
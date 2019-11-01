<template>
  <div class="demo-wrapper">
    <h1>
      Vue iOS 13 Segmented Control Demo
    </h1>
    
    <ios13-segmented-control class="segmented-control--wrapper" v-model="value" :elementName="elementName" :segments="segments"/>


    <h2>Selected Element</h2>
    <code>{{segments.find(seg => seg.id === value)}}</code>

    <section>

    <h2>Edit segments</h2>
      <ul>
        <li v-for="segment of segments" :key="segment.id" class="segment-editor-row">
          <input type="text" :id="`${elementName}--${segment.id}--title`" v-model="segment.title">

          <button class="btn-danger" @click="deleteSegment(segment.id)">⨉</button>
        </li>
      </ul>

      <div class="segment-editor-row" style="margin-top:16px;">
        <input type="text" v-model="newSegmentTitle">
        <button class="btn-success" @click="addSegment">+</button>
      </div>
    </section>

    <section class="footer">
      <h2>Quickstart</h2>
      <code>npm i vue-ios13-segmented-control</code>
      <p><a href="https://github.com/AnalyzePlatypus/vue-ios13-segmented-control">Read the docs on GitHub ›</a></p>
      <a class="github-button" href="https://github.com/AnalyzePlatypus/vue-ios13-segmented-control" data-size="large" data-show-count="true" aria-label="Star AnalyzePlatypus/vue-ios13-segmented-control on GitHub">Star</a>
    </section>
  </div>
</template>

<script>

import Vue from 'vue';
import iOS13SegmentedControl from "vue-ios13-segmented-control";

Vue.component('ios13-segmented-control', iOS13SegmentedControl);

function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      elementName: "music-service-picker",
      value: "42",
      newSegmentTitle: "",
      segments: [
        {
          title: "Apple Music",
          id: "42"
        },
        {
          title: "Spotify",
          id: "93"
        },
        {
          title: "Deezer",
          id: "11"
        },
      ]
    }
  },
  methods: {
    deleteSegment(id) {
      this.segments = this.segments.filter(seg => seg.id !== id);
      if(this.value === id) this.value = this.segments[0].id;
    },
    addSegment() {
      if(!this.newSegmentTitle) return;
      this.segments.push({
        title: this.newSegmentTitle,
        id: getRandomInt(100)
      })
      this.newSegmentTitle = "";
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

@media screen and (min-width: 600px) {
  .demo-wrapper {
    margin-top: 32px;
  }

  h1 {
    margin-bottom: 16px;
  }
}


@media screen and (max-width: 400px) {
  input[type='text'] {
    font-size: 1.4rem;
    padding: 10px 8px;
    border-radius: 8px;
  }

  button {
    font-size: 1.4rem;
    border-radius: 8px;
    padding: 10px 14px;
    margin-left: 4px;
  }
}

@media screen and (min-width: 401px) {
  input[type='text'] {
    font-size: 1.4rem;
    padding: 4px 6px;
    padding-left: 5px;
    border-radius: 4px;
  }

  button {
    font-size: 1rem;
    border-radius: 4px;
    padding: 6px 10px;
  }
}

.demo-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

h1 {
   margin-bottom: 8px;
}

h2 {
  margin-top: 32px;
  margin-bottom: 8px;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}


.segmented-control--wrapper {
  max-width:800px;
  margin-top: 16px;
}

.segment-editor-row {
  margin-bottom: 8px;
  display: flex;
  align-items: center;
}

input[type='text'] {
  border: 2px solid lightgray;
}

button {
  background: lightgray;
  font-weight: 600;
  border: 2px solid lightgray;
  margin-left: 4px;
}

button.btn-danger {
  background: coral;
  border-color: coral;
}

button.btn-danger:hover {
  background: rgb(251, 111, 59);
  border-color: rgb(251, 111, 59);
}

button.btn-success {
  background: #90EE90;
  border-color: #90EE90;
}

button.btn-success:hover {
  background: rgb(127, 236, 127);
  border-color: rgb(127, 236, 127);
}

.footer {
  margin-bottom: 32px;
}

</style>

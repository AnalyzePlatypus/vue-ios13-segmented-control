<template>
<div>
    <div class="vue-ios13-segmented-control">
      <span class="selection-pill" :style="pillTransformStyles"></span>

      <div v-for="segment of segments" :key="segment.id" class="option">
				<input type="radio" 
          :id="segment.id" 
          :name="elementName" 
          :value="segment.id"
          v-model="selectedSegmentId">
				<label :for="segment.id">
          <span>{{segment.title}}</span>
        </label>
			</div>
    </div>
  </div>
</template>

<script>

// Taken from https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}

export default {
  name: 'iOS13SegmentedControl',
  props: {
    value: {
      required: true,
      type: [Number, String]
    },
    segments: {
      required: true,
      type: Object
    },
    elementName: {
      type: String,
      required: false,
      default: () => getRandomInt(100000)
    }
  },
  data() {
    return {
      selectedSegmentWidth: 0,
    };
  },

  mounted() {
    window.addEventListener('resize', this.recalculateSelectedSegmentWidth);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.recalculateSelectedSegmentWidth);
  },

  computed: {
    selectedSegmentId: {
      get() {
        return this.value;
      },
      set(segmentId) {
        // this.value = segmentId;
        this.$emit("change", segmentId);
      }
    },
    selectedSegmentIndex() {
      return this.segments.findIndex(segment => segment.id === this.value);
    },
    pillTransformStyles() {
      this.recalculateSelectedSegmentWidth();
      return "transform:translateX(" + (this.selectedSegmentWidth * this.selectedSegmentIndex) + "px)" + `;width:${this.selectedSegmentWidth}px;`;
    }
  },

  methods: {
    recalculateSelectedSegmentWidth() {
      const segmentElement = document.querySelector(`input[type='radio'][value='${this.value}']`);
      this.selectedSegmentWidth = segmentElement && segmentElement.scrollWidth;
      this.selectedSegmentPositionLeft = segmentElement && segmentElement.getBoundingClientRect().left;
    }
  }
}
</script>

<style lang="scss">	
  // Root CSS class
	.vue-ios13-segmented-control {
    font-family: -apple-system,system-ui,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,sans-serif;
    text-rendering: geometricPrecision;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		-webkit-overflow-scrolling: touch!important;
		touch-action: manipulation!important;
		-webkit-tap-highlight-color: rgba(0, 0, 0, 0);

    --background: rgba(239,239,240,1);
		background: var(--background);
		
		border-radius: 9px;
		margin: 0;
		padding: 2px;
		border: none;
		outline: none;
		display: grid;
		grid-auto-flow: column;
		grid-auto-columns: 1fr;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;

    overflow-x: scroll;

    label {
      cursor:inherit;
    }

    .option {
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      cursor: pointer;

      &:hover input:not(:checked) + label span,
      &:focus input:not(:checked) + label span,
      &:active input:not(:checked) + label span {
        opacity: .2;
      }

      &:active input:not(:checked) + label span {
        transform: scale(.95);
      }

      &:first-of-type {
        grid-column: 1;
        grid-row: 1;
        box-shadow: none;

        input {
          &::before
          {
            opacity: 0;
          }
        }
      }

       &:last-of-type {
        input {
          &::after
          {
            opacity: 0;
          }
        }
      }

      // Hide the underlying radio button
      input {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        width: 100%;
        height: 100%;
        padding: 0;
        margin: 0;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        outline: none;
        border: none;
        

        &:checked+label {
          cursor: default;

          &::before, 
          &::after {
            background: var(--background);
            z-index: 1;
          }
        }

        // Grey dividers between segments
        &::before, 
        &::after {
          content: '';
          width: 1px;
          background: rgba(142,142,147,.15) !important;
          position: absolute;
          top: 10%;
          bottom: 10%;
          border-radius: 10px;
          will-change: background;
          -webkit-transition: background .2s ease;
          transition: background .2s ease;
        }

        &::before {
          left: 0;
          transform: translateX(-.5px);
        }

        &::after {
          right: 0;
          transform: translateX(.5px);
        }
      }

      label {
        position: relative;
        display: block;
        text-align: center;
        padding: 3px 6vmin;
        margin-bottom: 2px;
        background: rgba(255,255,255,0);
        font-weight: 500;
        color: rgba(0,0,0,1);
        font-size: 14px;
        
        // Child node that contans the actual text
        span {
          display: block;
          position: relative;
          z-index: 2;
          -webkit-transition: all .2s ease;
          transition: all .2s ease;
          will-change: transform;
        }
      }
    }

  // Background pill
  .selection-pill {
		background: rgba(255,255,255,1);
		border: .5px solid rgba(0,0,0,0.04);
		box-shadow: 0 3px 8px 0 rgba(0,0,0,0.12), 0 3px 1px 0 rgba(0,0,0,0.04);
		border-radius: 7px;
		grid-column: 1;
		grid-row: 1;
		z-index: 2;
		will-change: transform;
		-webkit-transition: transform .2s ease;
		transition: transform .2s ease;
	}
}
</style>

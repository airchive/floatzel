<template>
  <div id="element" class="element" ref="element">
    <div
      id="element-subelement"
      class="element__subelement"
      @mousedown="dragMouseDown"
    />
  </div>
</template>

<script>
export default {
  name: "FloatingElement",

  data: function () {
    return {
      coordinates: {
        x: 0,
        y: 0,
        offsetX: 0,
        offsetY: 0,
      },
    };
  },

  methods: {
    dragMouseDown: function (event) {
      event.preventDefault();
      this.coordinates.x = event.clientX;
      this.coordinates.y = event.clientY;
      document.onmousemove = this.elementDrag;
      document.onmouseup = this.closeDragElement;
    },

    elementDrag: function (event) {
      event.preventDefault();
      let element = this.$refs.element;

      this.coordinates.offsetX = this.coordinates.x - event.clientX;
      this.coordinates.offsetY = this.coordinates.y - event.clientY;
      this.coordinates.x = event.clientX;
      this.coordinates.y = event.clientY;

      this.$refs.element.style.top =
        element.offsetTop - this.coordinates.offsetY + "px";

      this.$refs.element.style.left =
        element.offsetLeft - this.coordinates.offsetX + "px";
    },

    closeDragElement: function () {
      document.onmouseup = null;
      document.onmousemove = null;
    },
  },
};
</script>

<style>
.element {
  right: 25px;
  bottom: 25px;
  z-index: 9998;
  position: fixed;
}

#element-subelement.element__subelement {
  width: 60px;
  color: white;
  height: 60px;
  z-index: 9999;
  background: #1ed392;
}
</style>
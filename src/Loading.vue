<template>
  <div class="loading">
    <div class="loading-overlay"></div>
    <div class="loading-bars">
      <div></div>
      <div></div>
      <div></div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
$scale: 4;
.loading {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 10;
  transition: opacity 300ms;
  opacity: 0;
  pointer-events: none;

  &-overlay {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: #fff;
  }

  &-visible {
    opacity: 1;
    pointer-events: auto;
  }

  &-bars {
    display: inline-grid;
    grid-template-columns: repeat(3, $scale * 3px);
    grid-column-gap: $scale * 1px;
    position: relative;

    div {
      display: block;
      width: 100%;
      height: $scale * 10px;
      background-color: #edf2f7;
      border: 2px solid #cbd5e0;
      animation-duration: 1s;
      animation-iteration-count: infinite;

      &:nth-child(1) {
        animation-name: loading-1;
      }
      &:nth-child(2) {
        animation-name: loading-2;
      }
      &:nth-child(3) {
        animation-name: loading-3;
      }
    }
  }
}

@keyframes loading-1 {
  0% {
    transform: scaleY(1);
  }
  15% {
    transform: scaleY(1.6);
  }
  30% {
    transform: scaleY(1);
  }
}
@keyframes loading-2 {
  15% {
    transform: scaleY(1);
  }
  30% {
    transform: scaleY(1.6);
  }
  45% {
    transform: scaleY(1);
  }
}
@keyframes loading-3 {
  30% {
    transform: scaleY(1);
  }
  45% {
    transform: scaleY(1.6);
  }
  60% {
    transform: scaleY(1);
  }
}

// .loading div:nth-child(1) {
//   left: 6px;
//   animation-delay: -0.24s;
// }
// .loading div:nth-child(2) {
//   left: 26px;
//   animation-delay: -0.12s;
// }
// .loading div:nth-child(3) {
//   left: 45px;
//   animation-delay: 0;
// }
// @keyframes loading-1 {
//   0% { top: 6px; height: 51px; }
//   50%, 100% { top: 19px; height: 26px; }
// }
</style>

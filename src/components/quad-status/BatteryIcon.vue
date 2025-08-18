<template>
  <div class="battery-icon">
    <div class="quad-status-contents">
      <div
        class="battery-status"
        :class="classes"
        :style="{ width: batteryWidth + '%' }"
      />
    </div>
  </div>
</template>
<script>
const NO_BATTERY_VOLTAGE_MAXIMUM = 1.8; // Maybe is better to add a call to MSP_BATTERY_STATE but is not available for all versions

export default {
  props: {
    voltage: {
      type: Number,
      default: 0,
    },
    vbatmincellvoltage: {
      type: Number,
      default: 1,
    },
    vbatmaxcellvoltage: {
      type: Number,
      default: 1,
    },
    vbatwarningcellvoltage: {
      type: Number,
      default: 1,
    },
  },
  computed: {
    nbCells() {
      let nbCells = Math.floor(this.voltage / this.vbatmaxcellvoltage) + 1;
      if (this.voltage === 0) {
        nbCells = 1;
      }
      return nbCells;
    },
    min() {
      return this.vbatmincellvoltage * this.nbCells;
    },
    max() {
      return this.vbatmaxcellvoltage * this.nbCells;
    },
    warn() {
      return this.vbatwarningcellvoltage * this.nbCells;
    },
    isEmpty() {
      return (
        this.voltage < this.min && this.voltage > NO_BATTERY_VOLTAGE_MAXIMUM
      );
    },
    classes() {
      if (this.batteryState) {
        return {
          "state-ok": this.batteryState === 0,
          "state-warning": this.batteryState === 1,
          "state-empty": this.batteryState === 2,
          // TODO: BATTERY_NOT_PRESENT
          // TODO: BATTERY_INIT
        };
      }
      const isWarning = this.voltage < this.warn;
      return {
        "state-empty": this.isEmpty,
        "state-warning": isWarning,
        "state-ok": !this.isEmpty && !isWarning,
      };
    },
    batteryWidth() {
      return this.isEmpty
        ? 100
        : ((this.voltage - this.min) / (this.max - this.min)) * 100;
    },
  },
};
</script>

<style>
.quad-status-contents {
  display: inline-block;
  margin-top: 10px;
  margin-left: 14px;
  height: 10px;
  width: 31px;
}

.quad-status-contents progress::-webkit-progress-bar {
  height: 12px;
  background-color: #eee;
}

.quad-status-contents progress::-webkit-progress-value {
  background-color: #bcf;
}

.battery-icon {
    background-image: url("data:image/svg+xml,%3c?xml%20version='1.0'%20encoding='utf-8'?%3e%3c!--%20Generator:%20Adobe%20Illustrator%2019.1.0,%20SVG%20Export%20Plug-In%20.%20SVG%20Version:%206.00%20Build%200)%20--%3e%3csvg%20version='1.1'%20id='Ebene_1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%20x='0px'%20y='0px'%20viewBox='0%200%2085%2056.7'%20style='enable-background:new%200%200%2085%2056.7;'%20xml:space='preserve'%3e%3cstyle%20type='text/css'%3e%20.st0{fill:%23838281;}%20%3c/style%3e%3cg%3e%3cpath%20class='st0'%20d='M7,43.6c1,1,2.1,1.5,3.5,1.5h59.9c1.4,0,2.5-0.5,3.5-1.5c1-1,1.5-2.1,1.5-3.5v-6h2.3c1.3,0,2.3-1,2.3-2.3v-6.1%20c0-1.3-1-2.3-2.3-2.3h-2.3v-5.8c0-1.4-0.5-2.5-1.5-3.5c-1-1-2.1-1.5-3.5-1.5l-59.9,0c-1.4,0-2.6,0.5-3.5,1.4c-1,1-1.4,2.2-1.4,3.5%20v22.4C5.5,41.4,6,42.6,7,43.6L7,43.6z%20M9.1,16.7c0-0.3,0.1-0.4,0.4-0.4l61.9,0c0.2,0,0.4,0.2,0.4,0.4v24.4c0,0.2-0.2,0.4-0.4,0.4%20l-61.9,0c-0.3,0-0.4-0.1-0.4-0.4V16.7z%20M9.1,16.7'/%3e%3c/g%3e%3c/svg%3e");
    background-size: contain;
    background-position: center;
    display: inline-block;
    height: 30px;
    width: 60px;
    transition: none;
    margin-top: 4px;
    margin-left: -4px;
    background-repeat: no-repeat;
}

.battery-status {
  height: 11px;
}

@keyframes error-blinker {
  0% {
    background-color: transparent;
  }
  50% {
    background-color: var(--error);
  }
}

.battery-status.state-ok {
  background-color: #59aa29;
}
.battery-status.state-warning {
  background-color: var(--error);
}

.battery-status.state-empty {
  animation: error-blinker 1s linear infinite;
}
</style>

<template lang="html">
<div class="snap-tooltipster tooltip-container">
    <slot></slot>
</div>
</template>

<script>
require('tooltipster');
require('tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-shadow.min.css');
require('tooltipster/dist/css/tooltipster.bundle.min.css');

export default {
  name: 'SnapTooltipster',
  props: {
    targetSelector: {
      type: String,
      required: true
    },
    templateSelector: {
      type: String,
      required: false
    },
    customTheme: {
      type: String,
      required: false,
      validator: (value) => {
        return value.includes('tooltipster-');
      }
    },
    pluginConfig: {
      type: Object,
      required: false,
      default: () => {
        return {};
      }
    },
    absoluteLeft: {
      type: Number,
      required: false,
      default: 0
    }
  },
  data () {
    return {
      defaultPluginConfig: {
        arrow: false,
        IEmin: 9,
        trackOrigin: false,
        side: ['right', 'left'],
        offsetTop: 0,
        offsetLeft: 0,
        functionPosition: (instance, helper, position) => {
          var offsetTop = this.pluginConfig.offsetTop || 0;
          var offsetLeft = this.pluginConfig.offsetLeft || 0;
          position.coord.top += offsetTop;
          position.coord.left += offsetLeft;
          if (this.absoluteLeft) {
            position.coord.left = this.absoluteLeft;
          }
          return position;
        }
      }
    };
  },
  created () {
    this.$nextTick(() => {
      var forcedConfig = {
        theme: ['tooltipster-shadow', this.customTheme],
        content: this.pluginConfig.content || $(this.templateSelector)
      };
      var finalConfig = Object.assign(this.defaultPluginConfig, this.pluginConfig, forcedConfig);
      $(this.targetSelector).tooltipster(finalConfig);
      console.log('select:', this.targetSelector);
      console.log('template:', this.templateSelector);
      console.log('templateContent:', $(this.templateSelector));
      console.log('computedcontent:', this.pluginConfig.content);
      console.log(finalConfig);
    });
  },
  updated () {
    // 里边内容变化超出tooltip时需要reposition
    $(this.targetSelector).tooltipster('reposition');
  }
};
</script>

<style lang="scss" scoped>
// 该div虽然不会处于body下的插件内容中，但是会控制插件内容显示是否正常
.tooltip-container {
  display: none;
}
</style>

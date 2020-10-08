<template>
  <input v-bind="$props" type="hidden">
</template>

<script>
  import uploadcare from 'uploadcare-widget';
  
  export default {
    props: {
      value: {
        type: String,
        required: false,
        default: ""
      },
    },
    data() {
      return {
        uploadcareUrl: "",
        widget: ''
      }
    },
    mounted() {
      this.initWidget();
    },
    watch: {
      uploadcareUrl() {
        this.updatePropUrl();
      },
      value() {
        this.updateWidgetValue();
      }
    },
    methods: {
      initWidget() {       
        this.widget = uploadcare.Widget(this.$el);
        this.updateWidgetValue();
        this.widget.onUploadComplete((info) => {
          this.uploadcareUrl = info.cdnUrl;
        });
      },
      updatePropUrl() {
        this.$emit('update:url', this.uploadcareUrl);
      },
      updateWidgetValue() {
        this.widget && this.widget.value(this.value);
      },
    }
  };
</script>

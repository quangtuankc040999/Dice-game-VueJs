<template>
  <div class="wrapper-popup" v-bind:class="getClassPopup">
    <div class="rule">
      <h3>Rule</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Blanditiis ut
        facilis temporibus distinctio officiis qui nam nesciunt ad, ipsam dolore
        dolor deserunt quasi aperiam, facere incidunt modi excepturi. Quidem,
        officia?
      </p>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptatem
        possimus natus, cum quam ducimus nisi eos aliquid quisquam fuga
        doloribus corrupti ipsum, provident temporibus, illum quae magni saepe
        minus quis.
      </p>
      <button v-on:click="confirm" class="confirm">
        I understand this rule
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: "propRule",
  props:{
      isOpenPopup:{
          type: Boolean,
          default: false
      }
  },
  methods:{
    confirm(){
        this.$emit('handleConfirm')
    }
  },
  computed: {
      getClassPopup: function(){
          return{
              'open-popup': this.isOpenPopup
          }
      }
  }

};
</script>
<style scoped>

    .wrapper-popup {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.4);
        opacity: 0;
        visibility: hidden;
        transition: all .3s ease;
    }
    .wrapper-popup.open-popup {
        opacity: 1;
        visibility: visible;
    }
    .rule {
        width: 350px;
        padding: 20px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        background-color: #fff;
        position: absolute;
        font-family: Arial, Helvetica, sans-serif;
        transition: all .3s ease;
    }
    .open-popup .rule {
        transform: translate(-50%, -50%) scale(1);
    }
    /* scale -> class */
    .rule h3 {
        margin-bottom: 10px;
    }
    .rule .confirm {
        cursor: pointer;
        margin-top: 20px;
        padding: 8px 15px;
        border: 2px solid #333;
        background-color: #fff;
        transition: all .3s ease;
    }
    .rule .confirm:hover {
        color: #fff;
        background-color: #333;
    }
</style>
<template>
    <div class="vue-modal"  v-show="open">
        <div class="vue-modal-inner">
            <div class="vue-mode-content">
                 <slot/>
            <button class="btn" type="button" @click="$emit('close')" >close</button>
            </div>
           
        </div>
    </div>
</template>

<script>
export default {
    props: {
        open: {
            type: Boolean,
            required: true
        }
    }
}
</script>

<style scoped>
@keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}
.vue-modal {
    /* css that applies the fade in animation*/
    backdrop-filter: blur(4px);
    animation: fadein 0.6s; /* Safari, Chrome and Opera > 12.1 */
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    padding: 5%;
    border-radius: 12px;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
}
.vue-modal-inner {
    font-size: larger;
    backdrop-filter: none;
}

.btn {
  background-color: #861aa1;
  border: 0px;
  border-radius: 8%;
  color: aliceblue;
  padding: 2% 3% 2%;
  font-size: large;
  transition: 0.1s;
}
.btn:hover{
  background-color: #a23ebb;
  box-shadow: 0 0 10px #d8a0e6;
}


</style>
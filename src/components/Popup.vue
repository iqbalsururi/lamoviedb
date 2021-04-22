<template>
  <div class="popup">
      <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-body">

                  <img :src="srcId" alt="Movie" srcset="" @click="$emit('close')" />
              
              </div>
            </div>
          </div>
        </div>
  </div>
</template>

<script>
export default {
props: ["srcId"] 
}
</script>

<style lang="scss">
@import '@/assets/modal.scss' 
</style>
<template>
  <div class="content">
    <form class="columns" v-on:submit.prevent="addDevice">
      <div class="column col-6 col-xs-12">
        <div class="form-group">
          <label class="form-label" for="input-name">Name</label>
          <input :class="{ 'form-input': true, 'is-error': errors.has('name') }" v-validate="'required'" v-model="name" name="name" type="text" id="input-name" placeholder="Device name">
          <p v-show="errors.has('name')" class="form-input-hint">{{ errors.first('name') }}</p>
        </div>
        <div class="form-group">
          <label class="form-label" for="input-hash">Hash</label>
          <input :class="{ 'form-input': true, 'is-error': errors.has('hash') }" v-validate="{ required: true, exist: { type: 'devices', key: 'hash' }}" v-model="hash" type="text" name="hash" id="input-hash" placeholder="Hash">
          <p v-show="errors.has('hash')" class="form-input-hint">{{ errors.first('hash') }}</p>
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary">Add new</button>
        </div>
      </div>
    </form>
    <hr />
    <div class="columns">
      <div v-for="item in list" class="column col-3">
        <device :device="item"></device>
      </div>
    </div>
  </div>
</template>
<script>
  import Device from './devices/device.vue'

  export default {
    components: {
      'device': Device
    },
    data: function(){
      return {
        name: '',
        hash: ''
      }
    },
    methods: {
      addDevice() {
        var self = this;
        this.$validator
            .validateAll()
            .then(function(result){
              if (result) {
                self.$store
                    .dispatch('devices/addNew', { name: self.name, hash: self.hash })
                    .then(function(){
                      self.name = '';
                      self.hash = '';
                      self.$validator.reset();
                    });
              }
            })
      }
    },
    computed: {
      list() {
        return this.$store.state.devices.list
      }
    },
  }
</script>
<style>
.column {
  margin-top: 8px;
}
</style>
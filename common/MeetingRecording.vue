<template>
  <div>
    <h1 class="page-title text-left mt-0">Meeting <span>Recordings</span></h1>
    <div class="table-responsive">
      <table class="table table-striped">
        <tr>
          <th>Meeting Id</th>
          <th>File Type</th>
          <th>Download</th>
          <th>Play</th>
        </tr>
        <tr v-if="meetingRecordingLength" v-for="r in meetingRecordingsList" v-bind:key="r.id">
          <td>{{ r.meeting_id }}</td>
          <td>{{ r.file_type }}</td>
          <td><a :href="r.download_url">Download</a></td>
          <td><a :href="r.play_url" target="_blank">Play</a></td>
        </tr>
        <tr v-if="!meetingRecordingLength">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import DatePicker from 'vue2-datepicker'
import 'vue2-datepicker/index.css'
import {Vue2TinymceEditor} from 'vue2-tinymce-editor'

export default {
  name: 'AdminMeetingRecording',
  mixins: [AppMixin],
  components: {
    Vue2TinymceEditor, DatePicker
  },
  data () {
    return {}
  },
  methods: {},
  mounted () {
    this.getMeetingRecordingsList(this.$route.params.id)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<template>
  <v-container>
    <v-row>
      <v-col>
        <v-form ref="form" v-model="valid">
          <v-text-field
            v-model="destination"
            outlined
            color="white"
            label="Destination URL"
            :rules="urlRules"
            hint="This is the destination for the tag"
            required
          ></v-text-field>

          <v-text-field
            v-model="source"
            color="white"
            outlined
            :rules="required"
            label="Source"
            hint="from where?"
            required
          ></v-text-field>

          <v-select
            v-model="medium"
            outlined
            :rules="required"
            item-text="name"
            item-value="value"
            :items="items"
            label="Medium"
            color="white"
            chips
            small-chips
            multiple
          />

          <v-text-field
            v-model="campaign"
            :rules="required"
            color="white"
            outlined
            label="Campaign Name"
            hint="Give it an awesome campaign name"
            required
          ></v-text-field>

          <v-btn :disabled="!valid" color="green" class="mr-4" @click="makeTag">
            Check it!
          </v-btn>

          <v-btn color="red" class="mr-4" @click="reset">
            Wreck it!
          </v-btn>
        </v-form>
        <v-list class="mt-12" v-if="tagList.length > 0">
          <v-list-item v-for="(item, i) in tagList" :key="i">
            <v-list-item-content>
              <v-list-item-title>
                <span>
                  <span class="blue--text text--lighten-2">{{
                    item.destination.trim()
                  }}</span
                  ><span class="green--text text--lighten-2">{{
                    '?utm_source=' + item.source
                  }}</span
                  ><span class="yellow--text text--lighten-2">{{
                    '&utm_medium=' + item.medium
                  }}</span
                  ><span class="purple--text text--lighten-2">{{
                    '&utm_campaign=' + item.campaign
                  }}</span>
                </span>
              </v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn
                depressed
                color="blue lighten-2"
                light
                v-clipboard:copy="url"
                @click="handleCopy"
                >Copy it!</v-btn
              >
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
    <v-snackbar v-model="success" color="teal darken-2">
      Your url has been copied to your clipboard. Just paste it somewhere.
      <v-btn light text @click="success = false">
        Super!
      </v-btn>
    </v-snackbar>
  </v-container>
</template>

<script>
/* eslint-disable prettier/prettier */
/* eslint-disable no-useless-escape */
export default {
  components: {},
  data: () => ({
    valid: true,
    success: false,
    urlRules: [
      (v) => !!v || 'URL is required',
      (v) =>
        /[(http(s)?):\/\/(www\.)?a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)/.test(
          v
        ) || 'Please enter a valid URL'
    ],
    required: [(v) => !!v || 'It is required.'],
    destination: '',
    tagList: [],
    source: '',
    medium: '',
    campaign: '',
    items: [{ name: 'Display', value: 'display' }]
  }),
  computed: {
    url() {
      let finishedUrl = ''
      if (this.tagList.length > 0) {
        const tags = this.tagList[0]
        const mapped = Object.keys(tags).map((item) => tags[item])
        const url =
          mapped[0] +
          '?utm_source=' +
          mapped[1] +
          '&utm_medium=' +
          mapped[2] +
          '&utm_campaign=' +
          mapped[3]
        finishedUrl = url
      }
      return finishedUrl
    }
  },
  methods: {
    reset() {
      this.destination = ''
      this.source = ''
      this.medium = ''
      this.campaign = ''
      this.valid = true
    },
    handleCopy() {
      this.success = true
      this.reset()
    },
    makeTag() {
      this.tagList.push({
        destination: this.destination.trim(),
        source: this.source.trim(),
        medium: this.medium[0].trim(),
        campaign: this.campaign.trim()
      })
    }
  }
}
</script>

<template>
  <span>
    <b-container fluid class="mb-4">
      <b-row>
        <b-col cols="12" lg="2">
          <EditSave
            :editable="editable"
            :form="{ data: armorClass, route: '/api/character/summary/ac' }"
            :character-id="characterId"
            @make-editable="makeEditable"
            @refresh="refresh"
        /></b-col>
        <b-col cols="12" lg="10"></b-col>
      </b-row>
      <div class="mt-4">
        <b-container fluid>
          <b-row>
            <b-col cols="12" md="2">
              <small>Total AC</small>
              <div>
                {{
                  10 +
                    parseInt(armorClass.armor_bonus) +
                    parseInt(armorClass.natural_bonus) +
                    parseInt(armorClass.size_bonus) +
                    parseInt(armorClass.misc_bonus) +
                    Math.floor(
                      (parseInt(armorClass.score) +
                        parseInt(armorClass.temp_score) -
                        10) /
                        2
                    ) || 0
                }}
              </div>
            </b-col>
            <b-col cols="12" md="2">
              <small>Dexterity Bonus</small>
              <div>
                {{
                  Math.floor(
                    (parseInt(armorClass.score) + parseInt(armorClass.temp_score) - 10) /
                      2
                  ) || 0
                }}
              </div>
            </b-col>
            <b-col cols="12" md="2">
              <small>Armor Bonus</small>
              <b-form-input
                v-model="armorClass.armor_bonus"
                :readonly="!editable"
                type="number"
                required
              />
            </b-col>
            <b-col cols="12" md="2">
              <small>Natural Bonus</small>
              <b-form-input
                v-model="armorClass.natural_bonus"
                :readonly="!editable"
                type="number"
                required
              />
            </b-col>
            <b-col cols="12" md="2">
              <small>Size Bonus</small>
              <b-form-input
                v-model="armorClass.size_bonus"
                :readonly="!editable"
                type="number"
                required
              />
            </b-col>
            <b-col cols="12" md="2">
              <small>Misc Bonus</small>
              <b-form-input
                v-model="armorClass.misc_bonus"
                :readonly="!editable"
                type="number"
                required
              />
            </b-col>
          </b-row>
        </b-container>
      </div>
    </b-container>
  </span>
</template>

<script>
import axios from 'axios';
import { mapMutations } from 'vuex';
import { mapGetters } from 'vuex';
import breadcrumbs from './../breadcrumb.js';
import EditSave from '../../../components/EditSave';

export default {
  components: { EditSave },
  props: {
    armorClass: {
      type: Object,
      default: () => {
        return {};
      },
    },
    characterId: {
      type: Number,
      default: 0,
    },
    editable: {
      type: Boolean,
    },
  },
  data: () => {
    return {};
  },
  computed: {
    // map `this.env` to `this.$store.getters.env`
    ...mapGetters({
      user: 'user',
      env: 'env',
    }),
  },
  methods: {
    updateView(breadcrumb) {
      this.$emit('update-view', {
        breadcrumb: breadcrumbs[breadcrumb],
        activeView: breadcrumb,
      });
    },
    makeEditable() {
      this.$emit('make-editable');
    },
    refresh() {
      this.$emit('refresh');
    },
  },
};
</script>
<style lang="scss" scoped>
input {
  text-align: center;
  font-size: 1rem;
}
</style>

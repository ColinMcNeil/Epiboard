<template>
  <div id="lastfm">
    <v-btn
      v-if="!loading && showTip && itemsLength"
      :dark="!$store.state.settings.theme.light"
      :light="$store.state.settings.theme.light"
      color="primary"
      class="ma-0"
      block
      flat small @click.stop="showTip=false">
      <span>{{ $t('LastFm.tip') }}</span>
      <v-spacer/>
      <v-icon>close</v-icon>
    </v-btn>
    <v-card-text v-if="loading">
      <v-progress-linear indeterminate/>
    </v-card-text>
    <div v-else-if="itemsLength" class="top-grid">
      <div
        :title="$t('onboarding.previous')"
        class="prev" @click.stop="active > 0 ? active -= 1 : active = itemsLength - 1"/>
      <v-tabs v-model="active" grow hide-slider>
        <v-tab v-for="(item, key) in items" :key="key"/>
        <v-tab-item v-for="(item, key) in items" :key="key">
          <v-layout wrap>
            <v-flex xs6>
              <div class="cover">
                <img :src="item.data[0].image.extralarge">
                <a :href="item.data[0].url" target="_blank" class="overlay">
                  <h4 class="subheading">{{ item.data[0].name }}</h4>
                  <span v-if="item.data[0].artist">{{ item.data[0].artist.name }}</span>
                  <p>{{
                    $tc('LastFm.plays', item.data[0].playcount, { nb: item.data[0].playcount })
                  }}</p>
                </a>
              </div>
            </v-flex>
            <v-flex v-for="i in 2" v-if="item.data[i]" :key="i" xs3>
              <v-layout row wrap>
                <div v-for="j in [i, i + 2]" v-if="item.data[j]" :key="j" class="cover">
                  <img :src="item.data[j].image.large">
                  <a :href="item.data[j].url" target="_blank" class="overlay">
                    <h4 class="caption">{{ item.data[j].name }}</h4>
                    <span v-if="item.data[j].artist">{{ item.data[j].artist.name }}</span>
                    <p>{{
                      $tc('LastFm.plays', item.data[j].playcount, { nb: item.data[j].playcount })
                    }}</p>
                  </a>
                </div>
              </v-layout>
            </v-flex>
          </v-layout>
        </v-tab-item>
      </v-tabs>
      <div
        :title="$t('onboarding.next')"
        class="next" @click.stop="active < itemsLength - 1 ? active += 1 : active = 0"/>
    </div>
    <v-card-text v-else-if="!user || !user.length" class="text-xs-center">
      <v-icon x-large="">library_music</v-icon>
      <h2 class="subheading">{{ $t('LastFm.need_user') }}</h2>
    </v-card-text>
    <v-card-text v-else class="text-xs-center">
      <v-icon x-large="">library_music</v-icon>
      <h2 class="subheading">{{ $t('LastFm.empty') }}</h2>
    </v-card-text>
  </div>
</template>
<script src="./main.js"></script>
<style lang="scss" rel='stylesheet/scss' src="./style.scss" scoped></style>

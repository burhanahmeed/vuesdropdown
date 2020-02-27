<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Vuesdropdown"/>
    <vues-dropdown placeholder="Tes" :options="options" :categories="categories" v-model="current"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import { Vuesdropdown } from './index.js'

export default {
  name: 'app',
  components: {
    HelloWorld,
    'vues-dropdown': Vuesdropdown
  },
  data () {
    return {
      current: {
        value: {
          title: 'Website Adds To Cart',
          value: 'offsite_conversion.fb_pixel_add_to_cart'
        },
        category: {
          title: 'Campaign',
          value: 'campaign'
        }
      },
      categories: [{
        title: 'Campaign',
        value: 'campaign'
      },{
        title: 'Adset',
        value: 'adset'
      },{
        title: 'Ad',
        value: 'ad'
      }],
      options: [
        {
      title: 'Most Common',
      subs: [
      {
        title: 'Website Adds To Cart',
        value: 'offsite_conversion.fb_pixel_add_to_cart',
      },
      {
        title: 'Website Leads',
        value: 'offsite_conversion.fb_pixel_lead',
        warning: 'The total value of on facebook conversion'
      },
      {
        title: 'Website Purchases',
        value: 'offsite_conversion.fb_pixel_purchase',
      },
      {
        title: 'Daily budget',
        value: 'daily_budget', //*
      },
      {
        title: 'Spent',
        value: 'spent',
        exclude: ['adset', 'campaign']
      },
      {
        title: 'Cost per website purchase',
        value: 'cost_per_purchase_fb',
      },
      {
        title: 'Web purhcase ROAS',
        value: 'website_purchase_roas',
      },
      {
        title: 'Purhcase ROAS',
        value: 'purchase_roas',
      },
      {
        title: 'Hours since creation',
        value: 'hours_since_creation', //*
      },
      {
        title: 'Active adset in a campaign',
        value: 'active_adset_in_campaign', //*
      },
      {
        title: 'Adset in a campaign',
        value: 'adset_in_campaign', //*
      },
      {
        title: 'Time less than',
        value: 'time_less_than', //* done
      },
      {
        title: 'Time greater than',
        value: 'time_greater_than', //* done
      }
    ]
  },
  {
    title: 'General',
    subs: [
      {
        title: 'CPM',
        value: 'cpm',
      },
      {
        title: 'Bid amount',
        value: 'bid_amount'
      },
      {
        title: 'lifetime budget',
        value: 'lifetime_budget',
      },
      {
        title: 'Frequency',
        value: 'frequency',
      },
      {
        title: 'Impressions',
        value: 'impressions',
      },
      {
        title: 'Reach',
        value: 'reach',
      },
      {
        title: 'Remaining budget',
        value: 'budget_remaining', //*
      }
    ]
  },
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

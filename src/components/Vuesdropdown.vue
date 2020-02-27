<template>
  <div style="position: relative">
    <span class="select--outter select--container">
      <template v-if="open">
        <input ref="fields" type="text" class="field inside" v-model="search" @keyup="doSearch()" autofocus>
        <img src="../assets/up.svg" class="arrow">
      </template>
      <template v-else>
        <span :style="{'display': block, 'color': exacValue ? '#4f4e4e': ''}" class="field inside" @click="setOpen(!open)">{{ exacValue || placeholder }}</span>
        <img src="../assets/down.svg" class="arrow">
      </template>
    </span>
    <span class="dropdown inside" :class="{'open' : open}">
      <span class="span-result">
        <div v-if="categories" class="option-static">
          <span class="option-static-title">Categories</span>
          <ul class="static-selection">
            <li @click="changeCategory(cid)" v-for="(c, cid) in categories" :key="cid" :class="{'selection-from-upper': true, 'selection-active': activeCategory == c.value}">{{ c.title }}</li>
          </ul>
        </div>
        <ul class="suggestion" :style="{ 'margin-top': categories ? '46px': '' }">
          
          <template v-if="filteredOptions.length == 0">
            <span>No data found</span>
          </template>
          <template v-for="(op, idx) in filteredOptions">
            <template v-if="op.subs">
              <List @selectOption="selectOption" @toggleOpen="toggleOpen" :props="{'val': val,'options': op, 'idx': idx, 'active': activeNow, 'activeCategory': activeCategory }" :key="idx" />
            </template>
            <template v-else>
              <li-comp :key="idx" :props="{'key': idx, 'subs': op, 'activeCategory': activeCategory, 'val': val }" @selectOption="selectOption"></li-comp>
            </template>
          </template>
        </ul>
      </span>
    </span>
  </div>
</template>

<script>
import List from './ListComponents.vue';
import LiComp from './LiComponent';
export default {
  components: {
    List,
    LiComp
  },
  props: ['options', 'placeholder', 'value', 'categories'],
  data () {
    return {
      selectedItem: 'tes',
      search: '',
      selectedOption: null,
      open: false,
      filteredOptions: this.options,
      activeNow: null,
      val: null,
      activeCategory: null,
      selectedCategory: {}
    }
  },
  methods: {
    setInit () {
      this.filteredOptions = this.options;
      this.val = this.value.value.value;
      this.selectedCategory = this.value.category;
      for (let i in this.options) {
        for (let x in this.options[i].subs) {
          if (this.options[i].subs[x].value == this.value.value.value) {
            this.activeNow = i;
          }
        }
      }
    },
    setOpen (isOpen) {
      this.open = isOpen
      this.setInit();
      setTimeout(() => {
        this.$refs.fields.focus();
      }, 500);
    },
    selected (id) {
      this.open = false
      this.$emit('input', id)
    },
    doSearch () {
      let newArray = [];
      let master_option = this.options;
      for (var index=0; index < master_option.length; index++) {
        if (master_option[index].subs) {
          let subs = master_option[index].subs;
          for (var i = 0; i < subs.length; i++) {
            newArray.push(subs[i]);
          }
        } else {
          newArray.push(master_option[index]);
        }
      }
      this.filteredOptions = this.filterBy('title', this.search, newArray);
      if (this.search == '') {
        this.filteredOptions = master_option;
      }
    },
    filterBy (attr, value, data) {
      return data.filter(function (elem) {
        return elem[attr].toLowerCase().indexOf(value) !== -1;
      });
    },
    filter (args) {
      this.adaccounts = this.adaccount_master.filter(f => f.ads_status === args)
    },
    toggleOpen (idx) {
      if (this.activeNow == idx) {
        this.activeNow = null;
      } else {
        this.activeNow = idx;
      }
    },
    selectOption (val) {
      this.open = false;
      this.search = '';
      this.$emit('input', {value: val, category: this.selectedCategory});
    },
    changeCategory (val) {
      this.selectedCategory = this.categories[val];
    }
  },
  mounted () {
    let self = this;
    document.addEventListener('click', function(e) {
        var node = e.target;
        var inside = false;
        while (node) {
            if (node.classList.contains('inside')) {
                inside = true;
                break;
            }
            node = node.parentElement;
        }

       if (!inside) {
          self.open = false;
           // click was outside
           // alert('outside')
       }
    });
    this.setInit ()
  },
  watch: {
    parent (val) {
      this.activeNow = val;
    },
    'value' (val) {
      this.val = val.value.value;
    },
    selectedCategory (val) {
      this.activeCategory = val.value;
    }
  },
  computed: {
    parent () {
      for (let i in this.options) {
        for (let x in this.options[i].subs) {
          if (this.options[i].subs[x].value == this.value.value.value) {
            return i;
          }
        }
      }
      return null;
    },
    exacValue () {
      if (this.value.value) {
        return this.value.value.title
      }
      return false;
    }
  }
}
</script>

<style scoped>
.bg-dark .dropdown {
  background-color: #1c1c3b;
}
.item:hover {
  background-color: #5897fb;
  color: white;
}
.item {
  cursor: pointer;;
  padding: 6px 20px;
}
.suggestion {
  padding: 5px;
  list-style: none;
  text-align: left;
  max-height: 250px;
  overflow: auto;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
}
.span-result {
  display: block;
  max-height: 300px;
}
.dropdown.open{
  display: block;
}
.dropdown {
  background-color: white;
  border: 1px solid #aaa;
  border-top-color: rgb(170, 170, 170);
  border-right-color: rgb(170, 170, 170);
  border-bottom-color: rgb(170, 170, 170);
  border-bottom-style: solid;
  border-bottom-width: 1px;
  border-left-color: rgb(170, 170, 170);
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  display: none;
  position: absolute;
  /* left: -100000px; */
  width: 100%;
  min-width: 250px;
  z-index: 1051;
}
.bg-dark .select--outter {
  background-color: transparent;
}
.select--outter {
  border-radius: 2px;
  cursor: text;
  border: 1px solid #aaa;
  background-color: white;
}
.select--container {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  cursor: pointer;
  display: flex;
  min-height: 32px;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  -webkit-user-select: none;
}
.field {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  border: none;
  font-size: 100%;
  margin-top: 5px;
  padding: 0 5px;
  width: 100%;
  background-color: transparent;
  color: #aaa;
  text-align: left;
}
.arrow {
  height: 10px;
  padding: 9px;
}
.small {
  height: 8px;
}
.option-static {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  height: 40px;
  border-bottom: solid 1px rgba(67, 62, 63, 0.2);
  -webkit-align-items: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: space-between;
  -webkit-justify-content: space-between;
  -ms-flex-pack: space-between;
  justify-content: space-between;
  padding: 0 15px;
}
.option-static-title {
    line-height: normal;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.05em;
    white-space: nowrap;
    -webkit-text-transform: uppercase;
    text-transform: uppercase;
    color: #A19F9F;
}
.static-selection {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  height: 100%;
  margin: 0;
  padding: 0;
}
.selection-from-upper {
    height: 40px;
    line-height: 40px;
    margin: 0 10px;
    cursor: pointer;
    color: #A19F9F;
    padding: 0;
}
.selection-active {
  color: #433F3E;
  border-bottom: 1px solid #FA7061;
}
</style>
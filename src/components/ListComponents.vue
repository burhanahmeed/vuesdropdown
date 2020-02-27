<template>
<li>
	<div style="cursor: pointer;" @click="open(props.idx)">
		<img src="../assets/down.svg" :class="{'arrow small': true, 'openchild': props.idx == props.active, 'closedchild': !(props.idx == props.active)}">
		<span class="title">{{ props.options.title }}</span>
	</div>
	<ul v-if="props.idx == props.active" class="child-span">
    <template v-for="(sub, idx_1) in props.options.subs">
      <li v-if="checkExclusion(sub.exclude)" :key="idx_1" class="item-child" @click="selectOption(idx_1)">
         <span :class="{'child-item': true, 'selected': sub.value == props.val}">{{ sub.title }}</span>
         <div class="warn-box" v-if="sub.warning">
             <span class="inner-box" :title="sub.warning">
                 <img style="height: 12px; color: grey" src="../assets/warn.svg" alt="">
             </span>
         </div>
      </li>
      <li class="item-child" v-else :key="idx_1">
          <span :class="{'child-item': true}" style="color: grey">{{ sub.title }}</span>
          <div class="warn-box">
             <span class="inner-box" style="color: grey; font-size: 12px">
                 disabled
             </span>
         </div>
      </li>
    </template>
	</ul>
</li>	
</template>

<script>
	export default {
		props: ['props'],
		data () {
			return {

			}
		},
		methods: {
			open (key) {
				this.$emit('toggleOpen', key);
			},
			selectOption (val) {
				this.$emit('selectOption', this.props.options.subs[val]);
            },
            checkExclusion (array) {
                let arrays = array ? true : false;
                if (arrays) {
                    for (let e of array) {
                        if (this.props.activeCategory == e) {
                            return false;
                        }
                    }
                }
                return true;
            }
		}
	}	
</script>

<style type="text/css">
li {
	list-style: none;
	padding: 5px;
	cursor: pointer;
}
.arrow {
  height: 15px;
  padding: 6px;
  vertical-align: text-top;
}
.small {
  height: 8px;
}
.openchild {
	-webkit-transform: rotate(-90deg);
    -ms-transform: rotate(-90deg);
    transform: rotate(-90deg);
    -webkit-transition: -webkit-transform 0.3s ease;
    -webkit-transition: transform 0.3s ease;
    transition: transform 0.3s ease;
    -webkit-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    transform: rotate(0deg);
	color: #433F3E;
}
.closedchild {
	-webkit-transform: rotate(-90deg);
    -ms-transform: rotate(-90deg);
    transform: rotate(-90deg);
    -webkit-transition: -webkit-transform 0.3s ease;
    -webkit-transition: transform 0.3s ease;
    transition: transform 0.3s ease;
	color: #433F3E;
}
.title {
	margin-left: 12px;
    font-weight: 700;
    color: #433F3E;
}
.child-span {
	display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    overflow: hidden;
    max-height: 0;
    padding: 0;
    margin: 0;
    -webkit-flex-direction: column;
    -ms-flex-direction: column;
    flex-direction: column;
    list-style-type: none;
    max-height: 100%;
}
.item-child {
	display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-align-items: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: space-between;
    -webkit-justify-content: space-between;
    -ms-flex-pack: space-between;
    justify-content: space-between;
    padding: 0 20px 0 60px;
    cursor: pointer;
}
.item-child:hover {
	background-color: #F5F6F7;
}
span .child-item {
	cursor: pointer;
    height: 41px;
    line-height: 41px;
    color: #433F3E;
    overflow: hidden;
    white-space: nowrap;
    -webkit-text-overflow: ellipsis;
    text-overflow: ellipsis;
}
.selected {
	font-weight: 700;
}
.warn-box {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-align-items: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
}
.inner-box {
    display: -webkit-inline-box;
    display: -webkit-inline-flex;
    display: -ms-flexbox;
    display: inline-flex;
    position: relative;
    z-index: 5;
}
</style>
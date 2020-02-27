<template>
<li>
	<div style="cursor: pointer;" @click="open(props.idx)">
		<img src="../assets/down.svg" :class="{'arrow small': true, 'openchild': props.idx == props.active, 'closedchild': !(props.idx == props.active)}">
		<span class="title">{{ props.options.title }}</span>
	</div>
	<ul v-if="props.idx == props.active" class="child-span">
    <template v-for="(sub, idx_1) in props.options.subs">
      <li-comp :key="idx_1" :props="{'key': idx_1, 'subs': sub, 'activeCategory': props.activeCategory, 'val': props.val}" @selectOption="selectOption"></li-comp>
    </template>
	</ul>
</li>	
</template>

<script>
    import LiComp from './LiComponent';
	export default {
        components: {
            'li-comp': LiComp
        },
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
				this.$emit('selectOption', val);
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

</style>
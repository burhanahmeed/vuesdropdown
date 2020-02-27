<template>
        <li v-if="checkExclusion(props.subs.exclude)" :key="props.key" class="item-child" @click="selectOption()">
            <span :class="{'child-item': true, 'selected': props.subs.value == props.val}">{{ props.subs.title }}</span>
            <div class="warn-box" v-if="props.subs.warning">
                <span class="inner-box" :title="props.subs.warning">
                    <img style="height: 12px; color: grey" src="../assets/warn.svg" alt="">
                </span>
            </div>
        </li>
        <li class="item-child" v-else :key="props.key">
            <span :class="{'child-item': true}" style="color: grey">{{ props.subs.title }}</span>
            <div class="warn-box">
                <span class="inner-box" style="color: grey; font-size: 12px">
                    disabled
                </span>
            </div>
        </li>
</template>

<script>
export default {
    props: ['props'],
    methods: {
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
        },
        selectOption () {
            this.$emit('selectOption', this.props.subs);
        },
    }
    
}
</script>

<style scoped>
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
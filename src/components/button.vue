<template>
    <a href="javascript:void(0)"
        :class="classes"
        @click="handleClick">
        <span v-if="showSlot" ref="slot"><slot></slot></span>
    </a>
</template>
<script>
    const prefixCls = 'jf-btn'
    export default {
        name: 'Button',
        props: ['type', 'shape', 'size', 'htmlType', 'long'],
        data () {
            return {
                showSlot: true
            };
        },
        computed: {
            classes () {
                if(this.type === 'delete'){
                    return 'delete_btn round_icon_btn';
                }else if(this.type === 'primary'){
                    return 'button btn_text caps_font button_medium cancle theme_color theme_colorHover font_weight600';
                }else{
                    return [
                        `${prefixCls}`,
                        {
                            [`${prefixCls}-${this.type}`]: !!this.type,
                            [`${prefixCls}-long`]: this.long,
                            [`${prefixCls}-${this.shape}`]: !!this.shape,
                            [`${prefixCls}-${this.size}`]: !!this.size
                        }
                    ];
                }
            }
        },
        methods: {
            handleClick (event) {
                this.$emit('click', event);
            }
        },
        mounted () {
            this.showSlot = this.$slots.default !== undefined;
        }
    };
</script>

<style lang="postcss">
    @import "../styles/button.css";
</style>

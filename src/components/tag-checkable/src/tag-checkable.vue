<template>
    <div :class="cls"  @click="handleChange">
        <span class="x-tag-text">
            <slot>{{name}}</slot>
        </span>
    </div>
</template>

<script>
    export default {
        name: 'XTagCheckable',
        props: {
            color: String,
            checked: false,
            disabled: {
                type: Boolean,
                default: false
            },
            tagKey: String,
            name: String
        },
        data() {
            return {
                visible: true,
                prefixCls: 'x-tag x-tag-checkable',
                internalChecked: this.checked
            };
        },
        watch: {
            checked(val) {
                this.internalChecked = val;
            }
        },
        computed: {
            cls() {
                let ret = [];
                ret.push(this.prefixCls);
                if (this.internalChecked) {
                    ret.push('x-tag-checked');
                }
                if (this.color && !this.isPresetColor(this.color)) {
                    ret.push('x-tag-has-color');
                }
                if (this.isPresetColor(this.color)) {
                    ret.push(`x-tag-${this.color}`);
                }
                if (this.disabled) {
                    ret.push('x-tag-disabled');
                }
                return ret.join(' ');
            }
        },
        methods: {
            isPresetColor(color) {
                return /^(pink|red|yellow|orange|cyan|green|blue|purple)(-inverse)?$/.test(color);
            },
            handleChange(e) {
                if (this.disabled) {
                    return;
                }
                this.internalChecked = !this.internalChecked;
                this.$emit('change', this, this.internalChecked);
            }
        }
    };
</script>

<template>
    <div 
        class="ax-picklist" 
        :class="{
                'open': open,
                'disabled': disabled,
                'filterable': filterable,
                'clearable': clearable,
                'multiple': multiple,
            }"
        v-click-outside="closeOptions"
    >
        <input 
            type="text"
            readonly="true" 
            autocomplete="off"
            :name="name" 
            :value="modelValue"                    
        >
        <div class="ax-select" @click="openOptions">
            <div class="ax-selected">
                <span class="ax-label">
                    <template v-if="Array.isArray(values)">
                        <span class="value" v-for="value in values" :key="value.value">{{ value.label }}</span>
                    </template>
                    <template v-else>
                        {{ values }}
                    </template>                    
                </span>
                <span class="ax-control" @click.stop="actionControl"></span>
            </div>
            <input 
                v-model="filter"
                type="text"
                ref="filter"                
                :placeholder="values"
            >
        </div>
        <div class="ax-content">            
            <div class="list">
                <ul v-if="filtredOptions.length > 0">
                    <li 
                        v-for="option in filtredOptions" 
                        :key="option.value"
                        :class="{ 'disabled': option.disabled, 'selected': multiple && modelValue.includes(option.value) }"
                        @click="selectOption(option)"
                    >{{ option.label }}</li>
                </ul>
                <div class="no-match" v-else>No matching data</div>
            </div>
        </div>
    </div>
</template>

<script>
import vClickOutside from "click-outside-vue3";

const NO_SELECTED = 'No selected';

export default {
    name: 'AxPicklist',
    directives: {
      clickOutside: vClickOutside.directive
    },
    emits: ['update:modelValue'],
    props: {
        name: {
            type: String,
            required: true,
        },
        options: {
            type: Array,
            required: true,
        },
        modelValue: {
            type: [String, Number, Array],
            default: null,
        },
        disabled: {
            type: Boolean,
            default: false,
        },
        filterable: {
            type: Boolean,
            default: false,
        },
        clearable: {
            type: Boolean,
            default: false,
        },
        multiple: {
            type: Boolean,
            default: false,
        },        
    },
    data() {
        return {
            open: false,
            filter: '',
            optionsMultple: [],
        }
    },
    methods: {
        openOptions() {
            if (!this.disabled) {
                this.open = true;

                this.$nextTick(() => {
                    this.$refs.filter.focus()
                })
            }            
        },
        closeOptions() {
            this.open = false;
            this.clearFilter();                  
        },
        clearFilter() {
            this.filter = '';
        },
        selectOption(option) {
            if (!option.disabled) {

                let options;
                
                if (this.multiple) {
                    this.optionsMultple = this.optionsMultple.map(item => {
                        return (item.value === option.value)
                            ? {...item, selected: !item.selected}
                            : item;
                    });

                    options = this.optionsMultple
                        .filter(item => item.selected)
                        .map(item => item.value);
                } else {
                    options = option.value;
                }

                this.$emit('update:modelValue', options);

                if (!this.multiple) {
                    this.closeOptions();
                }
                
            }
        },
        actionControl() {
            if (this.clearable) {
                this.$emit('update:modelValue', null);
                this.closeOptions();
            }
        },
    },
    mounted() {
        if (this.multiple) {
            this.optionsMultple = this.options.map(item => {
                return {
                    value: item.value,
                    selected: this.modelValue.includes(item.value),
                }
            });            
        }
    },
    computed: {
        values() {
            if (this.multiple) {
                const options = this.options
                    .filter(item => this.modelValue.includes(item.value));

                return options.length > 0 ? options : NO_SELECTED;
            } else {
                const option = this.options.find(item => item.value === this.modelValue);
                return option ? option.label : NO_SELECTED;
            }
            
        },
        filtredOptions() {
            if (this.filter) {
                return this.options.filter(item => item.label.toLowerCase().indexOf(this.filter.toLowerCase()) >= 0);
            }

            return this.options;
        },
    }
}
</script>

<style lang="less" scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap');

.ax-picklist {
    position: relative;
    font-family: 'Roboto', sans-serif;
    font-weight: 500;
    font-size: 13px;

    &>input {
        display: none;
    }

    &.open {
        .ax-select {
            border-color: #409eff;

            .ax-selected {
                .ax-control {
                    &:before{
                        transform: rotate(-45deg);
                    }

                    &:after{
                        transform: rotate(45deg);
                    }
                }
            }
        }

        .ax-content {
            display: block;
        }
    }

    &.disabled {
        .ax-select {
            background-color: #eee; 
            cursor: not-allowed;
        }
    }

    &.filterable.open {
        .ax-select {
            & > input {
                display: block;
            }
        }
    }

    &.clearable {
        .ax-select:hover {
            .ax-selected {
                .ax-control {
                    border: 1px solid #ddd;
                    border-radius: 50%;

                    &:hover {
                        background-color: #eee;
                    }

                    &:before{
                        left: 3px;
                        top: 6px;
                        width: 8px;
                    }

                    &:after{
                        right: 3px;
                        top: 6px;
                        width: 8px;
                    }
                }
            }
        }
    }

    &.multiple {
        .ax-select .ax-selected .ax-label {
            padding: 7px;
        }
    }

    .ax-select {        
        background-color: #fff;
        border-radius: 4px;
        border: 1px solid #dcdfe6;
        position: relative;
        
        cursor: pointer;        

        & > input {
            font-family: inherit;
            background-color: #fff;
            border: none;
            border-radius: 5px;
            outline: none;
            padding: 9px 15px;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            display: none;

            &::placeholder {
                color: #bbb;
            }            
        }

        .ax-selected {    
            height: 35px;        
            display: flex;
            justify-content: space-around;
            align-items: center;            

            .ax-label {
                flex-grow: 1;
                padding: 10px 10px;
                display: flex;
                flex-wrap: wrap;
                gap: 5px;

                .value {
                    border: 1px solid #ddd;
                    border-radius: 3px;
                    padding: 2px 5px;
                    background-color: #eee;
                    position: relative;                    
                }
            }

            .ax-control {
                display: flex;
                align-items: center;
                position: relative;
                width: 14px;
                height: 14px;
                margin: 0 10px;

                &:before{
                    content: '';
                    display: block;
                    position: absolute;
                    left: 1px;
                    width: 7px;
                    border-bottom: 1px solid #aaa;
                    transform: rotate(45deg);
                    transition: all .3s;
                }

                &:after{
                    content: '';
                    display: block;
                    position: absolute;
                    right: 1px;
                    width: 7px;
                    border-bottom: 1px solid #aaa;
                    transform: rotate(-45deg);
                    transition: all .3s;
                }
            }
        }        
    }

    .ax-content {
        background-color: #fff;
        box-shadow: 1px 1px 3px #ddd;
        border-radius: 4px;
        border: 1px solid #dcdfe6;
        position: absolute;
        z-index: 10;
        top: 50px;
        left: 0px;
        right: 0;
        max-height: 175px;
        overflow-y: auto;
        display: none;

        &::-webkit-scrollbar {
            width: 6px;
        }

        &::-webkit-scrollbar-track {
            background: #fff;
        }

        &::-webkit-scrollbar-thumb {
            background: #bdbdbd;
            border-radius: 3px;
        }        

        ul {
            list-style: none;
            padding-left: 0;
            margin: 0;

            li {
                padding: 10px 15px;
                cursor: pointer;

                &.disabled { 
                    cursor: not-allowed;
                    color: #aaa;
                }

                &.selected {
                    color: #409eff;
                    position: relative;

                    &:before {
                        content: '';
                        display: inline-block;
                        position: absolute;
                        width: 6px;
                        top:20px;
                        right: 19px;
                        border-bottom: 1px solid #409eff;
                        transform: rotate(45deg);
                    }

                    &:after {
                        content: '';
                        display: inline-block;
                        position: absolute;
                        width: 11px;
                        top:19px;
                        right: 11px;
                        border-bottom: 1px solid #409eff;
                        transform: rotate(-45deg);
                    }
                }

                &:hover:not(.disabled) {
                    background-color: #f3f2f2;                    
                }
            }
        }

        .no-match {
            padding: 10px 15px;
            color: #aaa;
        }
    }
}
</style>
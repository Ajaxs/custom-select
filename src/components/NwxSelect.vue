<template>
    <div 
        class="nwx-picklist" 
        :class="{
                'open': open,
                'disabled': disabled,
                'filterable': filterable,
                'clearable': clearable && values,
                'multiple': multiple,
            }"
        v-click-outside="closeDropdown"
    >
        <input 
            type="hidden"
            readonly="true" 
            autocomplete="off"
            :name="name" 
            :value="modelValue"                    
        >
        <div class="nwx-select" @click="clickDropdown">
            <div class="nwx-selected">
                <div class="nwx-label">
                    <template v-if="values">
                        <template v-if="multiple">
                            <span class="value" v-for="value in values" :key="value.value">
                                {{ value.label }}
                                <span class="remove" @click.stop="removeValue(value)">&times;</span>
                            </span>
                        </template>
                        <template v-else>
                            {{ values }}
                        </template>               
                    </template>
                    <template v-else>
                        <span class="nwx-placeholder">{{ placeholder }}</span>
                    </template>           
                </div>
                <div class="nwx-control" @click.stop="actionControl"></div>
            </div>
            
        </div>
        <div class="nwx-content-wrapper">  
            <div class="nwx-content">  
                <div class="filtrable" v-if="filterable">
                    <input 
                        v-model="filter"
                        type="text"
                        ref="filter"
                        placeholder="Search"
                    >
                </div>    
                <div class="list">
                    <ul v-if="filtredOptions.length > 0">
                        <li 
                            v-for="option in filtredOptions" 
                            :key="option.value"
                            :class="{
                                'disabled': option.disabled,
                                'selected': isSelectedOption(option.value)
                            }"
                            @click="selectOption(option)"
                        >
                            <slot :option="option">
                                <div class="option-default">
                                    <span class="option-label">{{ option.label }}</span>
                                    <span class="option-icon"></span>
                                </div>
                            </slot>
                        </li>
                    </ul>
                    <div class="no-match" v-else>No matching data</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import vClickOutside from "click-outside-vue3";

export default {
    name: 'NwxSelect',
    directives: {
      clickOutside: vClickOutside.directive
    },
    emits: ['update:modelValue'],
    props: {
        name: {
            type: String,
            required: true,
        },
        placeholder: {
            type: String,
            default: 'No selected',
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
        limit: {
            type: [Number, String],
            default: 0,
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
        clickDropdown() {
            if (!this.disabled) {
                if (this.open) {
                    this.closeDropdown();
                } else {
                    this.openDropdown();
                }
            }
        },
        openDropdown() {
            this.open = true;

            this.$nextTick(() => {
                this.focusFilter();
            });                     
        },
        closeDropdown() {
            this.open = false;
            this.clearFilter();                  
        },
        focusFilter() {
            if (this.filterable) {
                this.$refs.filter.focus();
            }
        },
        clearFilter() {
            this.filter = '';
        },
        updateModel(value) {
            this.$emit('update:modelValue', value);
        },
        removeValue(option) {
            this.optionsMultple = this.optionsMultple.map(item => {
                return (item.value === option.value)
                    ? {...item, selected: false}
                    : item;
            });

           const options = this.optionsMultple
                .filter(item => item.selected)
                .map(item => item.value);

            this.updateModel(options);
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

                    this.clearFilter();
                    this.focusFilter();   
                } else {
                    options = option.value;
                }

                this.updateModel(options);

                if (!this.multiple) {
                    this.closeDropdown();
                }
                
            }
        },
        isSelectedOption(value) {
            if (this.multiple && Array.isArray(this.modelValue)) {
                return this.modelValue.includes(value);
            } else {
                return this.modelValue === value;
            }

        },
        actionControl() {
            if (this.clearable) {
                const model = this.multiple ? [] : null;
                this.updateModel(model);
                this.closeDropdown();
            }
        },
    },
    mounted() {
        if (this.multiple && Array.isArray(this.modelValue)) {
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
            if (this.multiple && Array.isArray(this.modelValue)) {
                const options = this.options
                    .filter(item => this.modelValue.includes(item.value));

                return options.length > 0 ? options : null;
            } else {
                const option = this.options.find(item => item.value === this.modelValue);
                return option ? option.label : null;
            }
            
        },
        filtredOptions() {
            let options;

            if (this.filter) {
                options = this.options.filter(item => item.label.toLowerCase()
                .indexOf(this.filter.toLowerCase()) >= 0);
            } else {
                options = this.options;
            }

            if (
                this.multiple
                && this.limit
                && Array.isArray(this.values)
                && this.values.length >= this.limit
            ) {
                return options.map(item => {
                    if (!this.modelValue.includes(item.value)) {
                        return { ...item, disabled: true };
                    }

                    return item;
                });
            }

            return options;
        },
    }
}
</script>

<style lang="less" scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap');

div, input {
    box-sizing: border-box;
}

.nwx-picklist {
    position: relative;
    font-family: 'Roboto', sans-serif;
    font-weight: 500;
    font-size: 13px;

    &.open {
        .nwx-select {
            border-color: #409eff;

            .nwx-selected {
                .nwx-control {
                    &:before{
                        transform: rotate(-90deg);
                    }
                }
            }
        }

        .nwx-content-wrapper {
            .nwx-content {            
                display: block;         
            }
        }
    }

    &.disabled {
        .nwx-select {
            background-color: #eee; 
            cursor: not-allowed;
        }
    }

    &.clearable {
        .nwx-select:hover {
            .nwx-selected {
                .nwx-control {                    

                    &:hover:before {
                        border-color: #888;
                        color: #888;
                    }

                    &:before{
                        content: '\2716';
                        display: inline-block;
                        width: 14px;
                        height: 14px;
                        text-align: center;
                        transition: none;
                        position: absolute;
                        transform: rotate(0);
                        left: 2px;
                        top: -1px;
                        font-size: 9px;
                        color: #ccc;
                        border: 1px solid #ccc;
                        border-radius: 50%;
                    }
                }
            }
        }
    }

    &.multiple {
        .nwx-select .nwx-selected .nwx-label {
            padding: 7px;
        }
    }

    .nwx-select {        
        background-color: #fff;
        border-radius: 4px;
        border: 1px solid #dcdfe6;
        position: relative;
        
        cursor: pointer;        

        

        .nwx-selected {    
            min-height: 35px;        
            display: flex;
            justify-content: space-around;
            align-items: center;            

            .nwx-label {
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
                    cursor: text; 
                    
                    .remove {
                        margin-left: 5px;
                        cursor: pointer;
                    }
                }

                .nwx-placeholder {
                    color: #888;
                }
            }

            .nwx-control {
                display: flex;
                align-items: center;
                justify-content: center;
                position: relative;
                width: 20px;
                height: 14px;
                margin: 0 10px;

                &:before{
                    content: '\276F';
                    left: 1px;
                    transform: rotate(90deg);
                    transition: transform .3s;
                }
            }
        }        
    }

    .nwx-content-wrapper {
        height: 0px;
        position: relative;        

        .nwx-content {
            margin-top: 10px;
            background-color: #fff;
            box-shadow: 1px 1px 3px #ddd;
            border-radius: 4px;
            border: 1px solid #dcdfe6;            
            position: absolute;
            z-index: 10;
            top: 0px;
            left: 0px;
            right: 0;
            display: none;
            
            .filtrable {
                padding: 10px;

                input {
                    width: 100%;
                    font-family: inherit;
                    background-color: #fff;
                    border: 1px solid #ddd;
                    border-radius: 5px;
                    outline: none;
                    padding: 9px 15px;

                    &::placeholder {
                        color: #bbb;
                    }            
                }
            }

            ul {
                max-height: 175px;
                overflow-y: auto;
                list-style: none;
                padding-left: 0;
                margin: 0;

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

                li {
                    padding: 10px 15px;
                    cursor: pointer;                

                    &.disabled { 
                        cursor: not-allowed;
                        color: #aaa;
                    }

                    &.selected {                    
                        .option-default {
                            color: #409eff;

                            .option-icon {
                                width: 10px;
                                height: 10px;
                                position: relative;

                                &:before {
                                    content: '';
                                    display: inline-block;
                                    position: absolute;
                                    width: 6px;
                                    top:5px;
                                    right: 8px;
                                    border-bottom: 1px solid #409eff;
                                    transform: rotate(45deg);
                                }

                                &:after {
                                    content: '';
                                    display: inline-block;
                                    position: absolute;
                                    width: 11px;
                                    top:4px;
                                    right: 0px;
                                    border-bottom: 1px solid #409eff;
                                    transform: rotate(-45deg);
                                }
                            }
                        }
                    }

                    &:hover:not(.disabled) {
                        background-color: #f3f2f2;                    
                    }

                    .option-default {
                        display: flex;
                        justify-content: space-between;
                        align-items: center;                    
                    }                
                }
            }

            .no-match {
                padding: 10px 15px;
                color: #aaa;
            }
        }
    }
}
</style>
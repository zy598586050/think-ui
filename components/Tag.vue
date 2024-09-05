<template>
    <span class="tk-tag" :class="[
        size ? `tk-tag-${size}` : '',
        type ? `tk-tag-${type}` : ''
    ]" :disabled="disabled">
        <slot />
        <svg v-if="closable" @click="$emit('close')" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
    </span>
</template>

<script setup lang="ts">
defineProps({
    type: {
        type: String,
        default: ''
    },
    size: {
        type: String,
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    },
    closable: {
        type: Boolean,
        default: false
    }
})
</script>

<style scoped lang="scss">
.tk-tag {
    font-size: var(--textSizeSmall);
    padding: 2px 6px;
    color: var(--textColor);
    border-radius: 3px;
    background-color: var(--backgroundColor);
    cursor: default;
    display: flex;
    align-items: center;

    svg {
        width: 12px;
        height: 12px;
        margin-left: 4px;

        &:hover {
            cursor: pointer;
            border-radius: 2px;
            background-color: rgba(0, 0, 0, .15);
        }
    }

    &.tk-tag-mi {
        padding: 2px 6px 1px 6px;
        font-size: var(--textSizeMini);
    }

    &.tk-tag-sm {
        font-size: var(--textSizeSmall);
    }

    &.tk-tag-md {
        padding: 4px 8px;
        font-size: var(--textSizeMedium);
    }

    &.tk-tag-lg {
        padding: 6px 10px;
        font-size: var(--textSizeLarge);
    }

    /* primary */
    &.tk-tag-primary {
        background-color: var(--primaryColor);
        color: #FFFFFF;
    }

    &.tk-tag-primary {
        svg:hover {
            background-color: var(--primaryColorHover);
        }
    }

    /* info */
    &.tk-tag-info {
        background-color: var(--infoColor);
        color: #FFFFFF;
    }

    &.tk-tag-info {
        svg:hover {
            background-color: var(--infoColorHover);
        }
    }

    /* success */
    &.tk-tag-success {
        background-color: var(--successColor);
        color: #FFFFFF;
    }

    &.tk-tag-success {
        svg:hover {
            background-color: var(--successColorHover);
        }
    }

    /* warning */
    &.tk-tag-warning {
        background-color: var(--warningColor);
        color: #FFFFFF;
    }

    &.tk-tag-warning {
        svg:hover {
            background-color: var(--warningColorHover);
        }
    }

    /* error */
    &.tk-tag-error {
        background-color: var(--errorColor);
        color: #FFFFFF;
    }

    &.tk-tag-error {
        svg:hover {
            background-color: var(--errorColorHover);
        }
    }

    /* 禁用 */
    &.tk-tag[disabled="true"] {
        cursor: not-allowed;
        opacity: 0.5;
    }
}
</style>
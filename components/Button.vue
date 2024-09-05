<template>
    <button class="tk-btn"
        :class="[
            btnStyle,
            size ? `tk-btn-${size}` : '',
            { 'tk-btn-circle': circle },
            { 'tk-btn-square': square },
            { 'tk-btn-round': round }
        ]"
        :disabled="disabled">
        <span class="tk-btn-loading" v-if="loading"></span>
        <slot />
    </button>
</template>

<script setup lang="ts">
const props = defineProps({
    type: {
        type: String,
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    },
    loading: {
        type: Boolean,
        default: false
    },
    size: {
        type: String,
        default: ''
    },
    circle: {
        type: Boolean,
        default: false
    },
    square: {
        type: Boolean,
        default: false
    },
    round: {
        type: Boolean,
        default: false
    },
    ghost: {
        type: Boolean,
        default: false
    },
    dashed: {
        type: Boolean,
        default: false
    },
    quaternary: {
        type: Boolean,
        default: false
    }
})

const btnStyle = computed(() => {
    let modifier = ''
    if (props.ghost) {
        modifier = '-ghost'
    } else if (props.dashed) {
        modifier = '-dashed'
    } else if (props.quaternary) {
        modifier = '-quaternary'
    }
    return props.type ? `tk-btn-${props.type}${modifier}` : (props.ghost || props.dashed || props.quaternary ? `tk-btn${modifier}` : '')
})
</script>

<style scoped lang="scss">
.tk-btn {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    background-color: #FFFFFF;
    color: var(--textColor);
    padding: 8px 10px;
    border: 1px solid var(--borderColor);
    border-radius: 3px;
    font-size: var(--textSizeMedium);
    height: var(--textSizeMedium);
    cursor: pointer;
    transition: color .3s var(--bezier), background-color .3s var(--bezier), opacity .3s var(--bezier), border-color .3s var(--bezier), transform .1s var(--bezier);

    .tk-btn-loading {
        display: inline-block;
        aspect-ratio: 1 / 1;
        width: 15px;
        margin-right: 8px;
        background-color: currentColor;
        mask-size: 100%;
        mask-repeat: no-repeat;
        mask-position: center;
        mask-image: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0nMjQnIGhlaWdodD0nMjQnIHN0cm9rZT0nIzAwMCcgdmlld0JveD0nMCAwIDI0IDI0JyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnPjxzdHlsZT4uc3Bpbm5lcl9WOG0xe3RyYW5zZm9ybS1vcmlnaW46Y2VudGVyO2FuaW1hdGlvbjpzcGlubmVyX3pLb2EgMnMgbGluZWFyIGluZmluaXRlfS5zcGlubmVyX1Y4bTEgY2lyY2xle3N0cm9rZS1saW5lY2FwOnJvdW5kO2FuaW1hdGlvbjpzcGlubmVyX1lwWlMgMS41cyBlYXNlLW91dCBpbmZpbml0ZX1Aa2V5ZnJhbWVzIHNwaW5uZXJfektvYXsxMDAle3RyYW5zZm9ybTpyb3RhdGUoMzYwZGVnKX19QGtleWZyYW1lcyBzcGlubmVyX1lwWlN7MCV7c3Ryb2tlLWRhc2hhcnJheTowIDE1MDtzdHJva2UtZGFzaG9mZnNldDowfTQ3LjUle3N0cm9rZS1kYXNoYXJyYXk6NDIgMTUwO3N0cm9rZS1kYXNob2Zmc2V0Oi0xNn05NSUsMTAwJXtzdHJva2UtZGFzaGFycmF5OjQyIDE1MDtzdHJva2UtZGFzaG9mZnNldDotNTl9fTwvc3R5bGU+PGcgY2xhc3M9J3NwaW5uZXJfVjhtMSc+PGNpcmNsZSBjeD0nMTInIGN5PScxMicgcj0nOS41JyBmaWxsPSdub25lJyBzdHJva2Utd2lkdGg9JzMnPjwvY2lyY2xlPjwvZz48L3N2Zz4=);
    }

    &:hover {
        border: 1px solid var(--primaryColorHover);
        color: var(--primaryColorHover);
    }

    &:active {
        transform: scale(.95);
    }

    &:disabled:active {
        transform: scale(1);
    }

    &:disabled {
        cursor: not-allowed;
        opacity: 0.5;
    }

    &:disabled:hover {
        border: 1px solid var(--borderColor);
        color: var(--textColor);
    }

    &.tk-btn-ghost {
        background-color: transparent;
    }

    &.tk-btn-dashed {
        background-color: transparent;
        border: 1px dashed var(--borderColor);
    }

    &.tk-btn-dashed:hover,
    &.tk-btn-dashed:active {
        background-color: transparent;
        border: 1px dashed var(--primaryColorHover);
    }

    &.tk-btn-quaternary,
    &.tk-btn-quaternary:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--textColor);
    }

    &.tk-btn-quaternary:hover {
        border: 0;
        color: var(--textColor);
        background-color: var(--backgroundColor);
    }

    /* block按钮 */
    &.tk-btn-blok {
        text-align: center;
        display: block;
        width: 100%;
    }

    /* 大小 */
    &.tk-btn-mi {
        padding: 4px 6px;
        height: var(--textSizeMini);
        font-size: var(--textSizeMini);
    }

    &.tk-btn-sm {
        padding: 5px 7px;
        height: var(--textSizeSmall);
        font-size: var(--textSizeSmall);
    }

    &.tk-btn-md {
        padding: 8px 10px;
        height: var(--textSizeMedium);
        font-size: var(--textSizeMedium);
    }

    &.tk-btn-lg {
        padding: 10px 12px;
        height: var(--textSizeLarge);
        font-size: var(--textSizeLarge);
    }

    /* 圆角 */
    &.tk-btn-circle {
        padding: 7px 6px;
        border-radius: 50%;
    }

    &.tk-btn-round {
        padding: 8px 15px;
        border-radius: 20px;
    }

    &.tk-btn-square {
        padding: 7px 6px;
    }

    /* primary */
    &.tk-btn-primary,
    &.tk-btn-primary:disabled:hover {
        background-color: var(--primaryColor);
        border: 1px solid var(--primaryColor);
        color: #FFFFFF;
    }

    &.tk-btn-primary:hover {
        background-color: var(--primaryColorHover);
        border: 1px solid var(--primaryColorHover);
        color: #FFFFFF;
    }

    &.tk-btn-primary:active {
        background-color: var(--primaryColorPressed);
        border: 1px solid var(--primaryColorPressed);
        color: #FFFFFF;
    }

    &.tk-btn-primary-ghost,
    &.tk-btn-primary-ghost:hover,
    &.tk-btn-primary-ghost:active,
    &.tk-btn-primary-ghost:disabled:hover {
        background-color: transparent;
        color: var(--primaryColor);
        border: 1px solid var(--primaryColor);
    }

    &.tk-btn-primary-dashed,
    &.tk-btn-primary-dashed:hover,
    &.tk-btn-primary-dashed:active,
    &.tk-btn-primary-dashed:disabled:hover {
        background-color: transparent;
        color: var(--primaryColor);
        border: 1px dashed var(--primaryColor);
    }

    &.tk-btn-primary-quaternary,
    &.tk-btn-primary-quaternary:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--primaryColor);
    }

    &.tk-btn-primary-quaternary:hover {
        border: 0;
        color: var(--primaryColor);
        background-color: var(--backgroundColor);
    }

    /* info */
    &.tk-btn-info,
    &.tk-btn-info:disabled:hover {
        background-color: var(--infoColor);
        border: 1px solid var(--infoColor);
        color: #FFFFFF;
    }

    &.tk-btn-info:hover {
        background-color: var(--infoColorHover);
        border: 1px solid var(--infoColorHover);
        color: #FFFFFF;
    }

    &.tk-btn-info:active {
        background-color: var(--infoColorPressed);
        border: 1px solid var(--infoColorPressed);
        color: #FFFFFF;
    }

    &.tk-btn-info-ghost,
    &.tk-btn-info-ghost:hover,
    &.tk-btn-info-ghost:active,
    &.tk-btn-info-ghost:disabled:hover {
        background-color: transparent;
        color: var(--infoColor);
        border: 1px solid var(--infoColor);
    }

    &.tk-btn-info-dashed,
    &.tk-btn-info-dashed:hover,
    &.tk-btn-info-dashed:active,
    &.tk-btn-info-dashed:disabled:hover {
        background-color: transparent;
        color: var(--infoColor);
        border: 1px dashed var(--infoColor);
    }

    &.tk-btn-info-quaternary,
    &.tk-btn-info-quaternary:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--infoColor);
    }

    &.tk-btn-info-quaternary:hover {
        border: 0;
        color: var(--infoColor);
        background-color: var(--backgroundColor);
    }

    /* success */
    &.tk-btn-success,
    &.tk-btn-success:disabled:hover {
        background-color: var(--successColor);
        border: 1px solid var(--successColor);
        color: #FFFFFF;
    }

    &.tk-btn-success:hover {
        background-color: var(--successColorHover);
        border: 1px solid var(--successColorHover);
        color: #FFFFFF;
    }

    &.tk-btn-success:active {
        background-color: var(--successColorPressed);
        border: 1px solid var(--successColorPressed);
        color: #FFFFFF;
    }

    &.tk-btn-success-ghost,
    &.tk-btn-success-ghost:hover,
    &.tk-btn-success-ghost:active,
    &.tk-btn-success-ghost:disabled:hover {
        background-color: transparent;
        color: var(--successColor);
        border: 1px solid var(--successColor);
    }

    &.tk-btn-success-dashed,
    &.tk-btn-success-dashed:hover,
    &.tk-btn-success-dashed:active,
    &.tk-btn-success-dashed:disabled:hover {
        background-color: transparent;
        color: var(--successColor);
        border: 1px dashed var(--successColor);
    }

    &.tk-btn-success-quaternary,
    &.tk-btn-success-quaternary:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--successColor);
    }

    &.tk-btn-success-quaternary:hover {
        border: 0;
        color: var(--successColor);
        background-color: var(--backgroundColor);
    }

    /* warning */
    &.tk-btn-warning,
    &.tk-btn-warning:disabled:hover {
        background-color: var(--warningColor);
        border: 1px solid var(--warningColor);
        color: #FFFFFF;
    }

    &.tk-btn-warning:hover {
        background-color: var(--warningColorHover);
        border: 1px solid var(--warningColorHover);
        color: #FFFFFF;
    }

    &.tk-btn-warning:active {
        background-color: var(--warningColorPressed);
        border: 1px solid var(--warningColorPressed);
        color: #FFFFFF;
    }

    &.tk-btn-warning-ghost,
    &.tk-btn-warning-ghost:hover,
    &.tk-btn-warning-ghost:active,
    &.tk-btn-warning-ghost:disabled:hover {
        background-color: transparent;
        color: var(--warningColor);
        border: 1px solid var(--warningColor);
    }

    &.tk-btn-warning-dashed,
    &.tk-btn-warning-dashed:hover,
    &.tk-btn-warning-dashed:active,
    &.tk-btn-warning-dashed:disabled:hover {
        background-color: transparent;
        color: var(--warningColor);
        border: 1px dashed var(--warningColor);
    }

    &.tk-btn-warning-quaternary,
    &.tk-btn-warning-quaternary:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--warningColor);
    }

    &.tk-btn-warning-quaternary:hover {
        border: 0;
        color: var(--warningColor);
        background-color: var(--backgroundColor);
    }

    /* error */
    &.tk-btn-error,
    &.tk-btn-error:disabled:hover {
        background-color: var(--errorColor);
        border: 1px solid var(--errorColor);
        color: #FFFFFF;
    }

    &.tk-btn-error:hover {
        background-color: var(--errorColorHover);
        border: 1px solid var(--errorColorHover);
        color: #FFFFFF;
    }

    &.tk-btn-error:active {
        background-color: var(--errorColorPressed);
        border: 1px solid var(--errorColorPressed);
        color: #FFFFFF;
    }

    &.tk-btn-error-ghost,
    &.tk-btn-error-ghost:hover,
    &.tk-btn-error-ghost:active,
    &.tk-btn-error-ghost:disabled:hover {
        background-color: transparent;
        color: var(--errorColor);
        border: 1px solid var(--errorColor);
    }

    &.tk-btn-error-dashed,
    &.tk-btn-error-dashed:hover,
    &.tk-btn-error-dashed:active,
    &.tk-btn-error-dashed:disabled:hover {
        background-color: transparent;
        color: var(--errorColor);
        border: 1px dashed var(--errorColor);
    }

    &.tk-btn-error-quaternary,
    &.tk-btn-error-quaternary:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--errorColor);
    }

    &.tk-btn-error-quaternary:hover {
        border: 0;
        color: var(--errorColor);
        background-color: var(--backgroundColor);
    }

    /* link */
    &.tk-btn-link {
        padding: 0;
        border: 0;
        background-color: transparent;
        text-decoration: underline;
        color: var(--primaryColor);
    }

    &.tk-btn-link:hover,
    &.tk-btn-link:disabled:hover {
        border: 0;
        background-color: transparent;
        color: var(--primaryColor);
    }
}
</style>
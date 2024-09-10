<template>
    <table class="tk-table">
        <thead>
            <tr>
                <slot name="header" />
            </tr>
        </thead>
        <tbody>
            <slot name="body" />
        </tbody>
    </table>
</template>

<style lang="scss">
.tk-table {
    display: flex;
    flex-direction: column;
    width: 100%;

    thead {
        background-color: #fafafc;
        border: 1px solid #efeff5;

        tr {
            display: flex;

            th {
                flex: 1;
                text-align: center;
                font-size: var(--textSizeMedium);
                font-weight: bold;
                color: var(--textColor);
                padding: 6px 0;
                border-right: 1px solid #efeff5;

                &:last-child {
                    border-right: 0;
                }
            }
        }
    }

    tbody {
        display: flex;
        flex-direction: column;
        border-left: 1px solid #efeff5;
        border-right: 1px solid #efeff5;
        border-bottom: 1px solid #efeff5;

        tr {
            display: flex;
            text-align: center;
            border-bottom: 1px solid #efeff5;

            &:last-child {
                border-bottom: 0;
            }

            td {
                flex: 1;
                padding: 6px 0;
                border-right: 1px solid #efeff5;

                &:last-child {
                    border-right: 0;
                }
            }
        }
    }
}
</style>
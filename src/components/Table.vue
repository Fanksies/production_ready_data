<template>
    <div id="table" class="px-1 py-2">
        <p class="title">{{title}}</p>
        <b-table striped hover :items="items" :fields="fields"></b-table>
    </div>
</template>

<script>
export default {
    name: "Table",
    props: {
        items: Array,
        fields: Array,
        title: String
    }
}
</script>

<style lang="scss">
    #table {
        background-color: white;
        border-radius: 5px;
        .title {
            color: rgb(102, 102, 102);
            font-family: 'Roboto', sans-serif;
            font-size: 18px;
            font-weight: 700;
        }
        table {
            thead {
                tr {
                    height: 26px;
                    background-color:#1b70c5 !important;
                    th {
                        color: white;
                        font-family: 'Roboto', sans-serif;
                        font-size: 14px;
                        font-weight: bold;
                        padding-top: 0.25rem;
                        padding-bottom: 0.25rem;
                        padding-left: 0.75rem;
                        padding-right: 0.75rem;
                    }
                }
            }
            tbody {
                tr {
                    td {
                        color: rgb(67, 67, 67);
                        font-family: 'Roboto', sans-serif;
                        font-size: 14px;
                        padding-top: 0.35rem;
                        padding-bottom: 0.35rem;
                        padding-left: 0.75rem;
                        padding-right: 0.75rem;
                    }
                }
            }
        }
    }
</style>

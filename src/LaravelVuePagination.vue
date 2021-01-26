<template>
    <renderless-laravel-vue-pagination
        :data="data"
        :limit="limit"
        :show-disabled="showDisabled"
        :show-first-last="showFirstLast"
        :size="size"
        :align="align"
        v-on:pagination-change-page="onPaginationChangePage">

        <ul class="pagination"
            :class="{
                'pagination-sm': size == 'small',
                'pagination-lg': size == 'large',
                'justify-content-center': align == 'center',
                'justify-content-end': align == 'right'
            }"
            slot-scope="{ data, limit, showDisabled, size, align, computed, prevButtonEvents, nextButtonEvents, firstButtonEvents, lastButtonEvents, pageButtonEvents }">

            <li class="page-item pagination-first-nav" :class="{'disabled': !computed.firstPageUrl || computed.currentPage === 1}" v-if="computed.firstPageUrl || showFirstLast">
                <a class="page-link" href="#" aria-label="First" :tabindex="!computed.firstPageUrl && -1" v-on="firstButtonEvents">
                    <slot name="first-nav">
                        <span aria-hidden="true">First &laquo;</span>
                        <span class="sr-only">First</span>
                    </slot>
                </a>
            </li>

            <li class="page-item pagination-prev-nav" :class="{'disabled': !computed.prevPageUrl}" v-if="computed.prevPageUrl || showDisabled">
                <a class="page-link" href="#" aria-label="Previous" :tabindex="!computed.prevPageUrl && -1" v-on="prevButtonEvents">
                    <slot name="prev-nav">
                        <span aria-hidden="true">&laquo;</span>
                        <span class="sr-only">Previous</span>
                    </slot>
                </a>
            </li>

            <li class="page-item pagination-page-nav" v-for="(page, key) in computed.pageRange" :key="key" :class="{ 'active disabled': page == computed.currentPage }" :disabled="page == computed.currentPage">
                <a class="page-link" href="#" v-on="pageButtonEvents(page)">
                    {{ page }}
                    <span class="sr-only" v-if="page == computed.currentPage">(current)</span>
                </a>
            </li>

            <li class="page-item pagination-next-nav" :class="{'disabled': !computed.nextPageUrl}" v-if="computed.nextPageUrl || showDisabled">
                <a class="page-link" href="#" aria-label="Next" :tabindex="!computed.nextPageUrl && -1" v-on="nextButtonEvents">
                    <slot name="next-nav">
                        <span aria-hidden="true">&raquo;</span>
                        <span class="sr-only">Next</span>
                    </slot>
                </a>
            </li>

            <li class="page-item pagination-last-nav" :class="{'disabled': !computed.lastPageUrl || computed.currentPage === computed.lastPage}" v-if="computed.lastPageUrl || showFirstLast">
                <a class="page-link" href="#" aria-label="Next" :tabindex="!computed.lastPageUrl && -1" v-on="lastButtonEvents">
                    <slot name="last-nav">
                        <span aria-hidden="true">Last &raquo;</span>
                        <span class="sr-only">Last</span>
                    </slot>
                </a>
            </li>

        </ul>

    </renderless-laravel-vue-pagination>
</template>

<script>
import RenderlessLaravelVuePagination from './RenderlessLaravelVuePagination.vue';

export default {
    props: {
        data: {
            type: Object,
            default: () => {}
        },
        limit: {
            type: Number,
            default: 0
        },
        showDisabled: {
            type: Boolean,
            default: false
        },
        showFirstLast: {
            type: Boolean,
            default: false
        },
        size: {
            type: String,
            default: 'default',
            validator: value => {
                return ['small', 'default', 'large'].indexOf(value) !== -1;
            }
        },
        align: {
            type: String,
            default: 'left',
            validator: value => {
                return ['left', 'center', 'right'].indexOf(value) !== -1;
            }
        }
    },

    methods: {
        onPaginationChangePage (page) {
            this.$emit('pagination-change-page', page);
        }
    },

    components: {
        RenderlessLaravelVuePagination
    }
}
</script>

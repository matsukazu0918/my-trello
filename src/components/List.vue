<template>
    <div class="list">
        <div class="listheader">
            <p class="list-title">{{ title }}</p>
            <p class="list-counter">total: {{ totalCardList }}</p>
            <div class="deletelist" @click="removeList">✖︎</div>
        </div>
        <draggable group="cards"
            :list="cards"
            @end="$emit('change')"
        >
            <card v-for="(item, index) in cards"
                :body="item.body"
                :key="item.id"
                :cardIndex="index"
                :listIndex="listIndex"
            />
        </draggable>
        <card-add :listIndex="listIndex" />
    </div>
</template>

<script>
import draggable from 'vuedraggable'
import Card from './Card.vue'
import CardAdd from './CardAdd'

export default {
    components:  {
        draggable,
        CardAdd,
        Card,
    },

    props: {
        title: {
            type: String,
            required: true
        },
        cards: {
            type: Array,
            required: true
        },
        listIndex: {
            type: Number,
            required: true
        }
    },
    computed: {
        totalCardList() {
            return this.cards.length
        }
    },
    methods: {
        removeList: function() {
            if(confirm('本当にこのリストを削除しますか？')) {
                this.$store.dispatch('removelist', { listIndex: this.listIndex })
            }
        }
    },
}
</script>
<template>
    <div>
        <input type="checkbox" v-model="el.done" @change="updateDone">
        <span :class="{ done: el.done }">{{ el.title }}</span>
        <button @click="remove(el)">X</button>
    </div>
</template>

<script>
export default {
    name: "ListComp",
    props: {
        item: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            el: { ...this.item },
        };
    },
    methods: {
        remove(el) {
            this.$emit("remove-item", el);
        },
        updateDone() {
            this.$emit("update-done", this.el.done);
        },
    },
};
</script>

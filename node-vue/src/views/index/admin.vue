<template>
	<div class="homeContainer">
		<Header></Header>
		<Slide></Slide>
		<div class="viewBox noCopy" :class="{ 'collapse': collapse }">
			<Tags></Tags>
			<div class="viewBoxIn">
				<router-view v-slot="{ Component }">
					<transition name="move" mode="out-in">
						<keep-alive :include="tagsList">
							<component :is="Component" />
						</keep-alive>
					</transition>
				</router-view>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
	import { defineComponent, computed } from "vue";
	import { useStore } from "vuex";
	import { useRoute } from "vue-router";

	import Header from "@c/header.vue";
	import Slide from "@c/slide.vue"
	import Tags from "@c/tags.vue"
	export default defineComponent({
		components: {
			Header,
			Slide,
			Tags
		},
		setup() {
			const store = useStore();
			const collapse = computed(() => store.state.collapse);

			const tagsList = computed(() =>
				store.state.tagsList.map((item) => item.name)
			)
			return {
				collapse,
				tagsList
			}
		}
	})
</script>

<style lang="scss" scoped>
	.homeContainer{
		overflow-y: hidden;
		.viewBox {
			position: absolute;
			left: 200px;
			right: 0;
			top: 70px;
			bottom: 0;
			box-sizing: border-box;
			-webkit-transition: left .2s ease-in-out;
			transition: left .2s ease-in-out;
			background: #f8f8f8;
			overflow-y: hidden;
			&.collapse {
				left: 64px;
			}
			.viewBoxIn{
				height: calc(100% - 40px);
				overflow-y: auto;
				padding: 16px;
				box-sizing: border-box;
			}
		}
	}
</style>

<template>
	<div class="body">
		<div class="con">
			<Player />
<!--			<Ad />-->
			<expand />
			<Crad />
      <SwitchTheme />
		</div>
	</div>
</template>

<script setup>
import Player from './components/Player.vue'
import Expand from './components/Expand.vue'
import Ad from './components/Ad.vue'
import Crad from './components/Crad.vue'
import SwitchTheme from './components/SwitchTheme.vue'
</script>

<style scoped>
.body {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.con {
	width: var(--con-width);
	margin: 30px 0;

	@media only screen and (max-width:768px) {
		width: 100%;
		padding: 12px;
		margin: 0px;
	}

}
</style>

<template>
<MkModal ref="modal" :prefer-type="'dialog'" :z-priority="'high'" @click="success ? done() : () => {}" @closed="emit('closed')">
	<div class="iuyakobc" :class="{ iconOnly: (text == null) || success }">
		<i v-if="success" class="ti ti-check icon success"></i>
		<MkLoading v-else class="icon waiting" :em="true"/>
		<div v-if="text && !success" class="text">{{ text }}<MkEllipsis/></div>
	</div>
</MkModal>
</template>

<script lang="ts" setup>
import { watch, shallowRef } from 'vue';
import MkModal from '@/components/MkModal.vue';

const modal = shallowRef<InstanceType<typeof MkModal>>();

const props = defineProps<{
	success: boolean;
	showing: boolean;
	text?: string;
}>();

const emit = defineEmits<{
	(ev: 'done');
	(ev: 'closed');
}>();

function done() {
	emit('done');
	modal.value.close();
}

watch(() => props.showing, () => {
	if (!props.showing) done();
});
</script>

<style lang="scss" scoped>
.iuyakobc {
	position: relative;
	padding: 32px;
	box-sizing: border-box;
	text-align: center;
	background: var(--panel);
	border-radius: var(--radius);
	width: 250px;

	&.iconOnly {
		padding: 0;
		width: 96px;
		height: 96px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	> .icon {
		font-size: 32px;

		&.success {
			color: var(--accent);
		}

		&.waiting {
			opacity: 0.7;
		}
	}

	> .text {
		margin-top: 16px;
	}
}
</style>
